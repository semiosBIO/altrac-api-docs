# Altrac API Documentation
## Authentication

There are two authentication paths:
1. User credential authentication, documented below
2. API client authentication, see https://github.com/altracio/altrac-api-client

### POST: User Credential Login
Request:
```Shell
curl --request POST \
  --url https://altrac-api.com/users/login \
  --header 'content-type: application/json' \
  --data '{
  "email": "{email}",
  "password": "{password}"
}'
```
Response:
```javascript
{
  "id": "{id}",
  "email": "{email}",
  "first_name": null,
  "last_name": null,
  "company": null,
  "position": null,
  "country": null,
  "state": null,
  "city": null,
  "main_phone": null,
  "secondary_phone": null,
  "address": null,
  "zip_code": null,
  "customer_id": null,
  "token": "{token}"
}
```
You will be able to use the token generated in the response in order to perform the queries detailed below.

## User Model
### GET: User Information
This endpoint supplies the customer_id needed for other queries below.

Query:

```Shell
curl --request GET \
  --url https://altrac-api.com/users/authID \
  --header 'authorization: Bearer {token}'
```
Response:
```javascript
{
  "id": "{user_id}",
  "customer_id": "{customer_id}",
  "locale": {
    "tempConv": "f"
  },
  "permissions": {
    "pin": "{hash}",
    "level": 0 // {permission_level}
  },
  "created_at": "{date}",
  "updated_at": "{date}",
  "email": "{email}",
  "name": "{nickname}"
}
```

## Customer Model
### GET: Customer by ID

This endpoint is used by the web application to determine what applications the customer has and to build links for these applications.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/customers/{customer_id}' \
  --header 'authorization: Bearer {token}'
```
Response:
```JSON
{
  "id": "112345678901234567890",
  "applications": [
    {
      "name": "Moisture",
      "deviceType": "moistureSensor",
      "deviceGroup": "Moisture"
    },{
      "name": "Pumps",
      "deviceType": "pump",
      "deviceGroup": "Pumps"
    },{
      "name": "Valves",
      "deviceType": "valve",
      "deviceGroup": "Valves"
    },{
      "name": "Wind Machines",
      "deviceType": "windMachine",
      "deviceGroup": "Garden Valley"
    },{
      "name": "Weather",
      "deviceType": "temperature",
      "deviceGroup": "Weather"
    }
  ],
  "customer_name": "ABC Fruit",
  "groups_enabled": false,
  "is_active": true,
  "created_at": "2015-11-10T19: 49: 59.029Z",
  "updated_at": "2020-04-30T17: 18: 46.873Z",
  "security": null,
  "brand": { "name": "altrac", ... }
}
```
#### Data Fields
* id
  - string
  - unique customer identifier

* applications
  - Array of device group info
  
| field       | description                                           |
|:------------|:------------------------------------------------------|
| name        | display name of device group                          |
| deviceType  | type of equipment controlled by devices in this group |
| deviceGroup | ID of group                                           |

* customer_name 
  - string

* brand 
  - Object
  - Applies CSS and formatting to Altrac UI for customers of a brand

* deviceType 
  - string
  - equipment type controlled by device

| deviceType     | description                            |
|:---------------|:---------------------------------------|
| binDicator     | automated storage bin                  |
| coldAirDrain   | fan similar in purpose to wind machine |
| flow           | flow sensor                            |
| moistureSensor | moisture sensor                        |
| pump           | pump                                   |
| pumpFrostWater | water sprayer pump                     |
| temperature    | weather station                        |
| valve          | valve                                  |
| windMachine    | machine                                |

* is_active
  - boolean

| value | description                             |
|:-----:|:----------------------------------------|
| true  | supported by the system                 |
| false | customer is not supported by the system |

### GET: All Customer Devices by Type and Group

This endpoint returns all customer devices by Device Type and Device Group
- Device Type: This is the type of Altrac device -- an example would be `windMachine` which equals "Wind Machine". You can find values for this in the Customer Information query.
- Device Group: This is the group the device belongs to -- an example from Customer Information above would be `Garden Valley`. Groups are used primarily to distinguish between ranches.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/customers/{customer_id}/devices?deviceType={deviceType}&deviceGroup={deviceGroup}' \
  --header 'authorization: Bearer {token}'
```
Response:

The response is an array of devices depending on the number of devices that meet the queries criteria. The data for each device will match that outlined below in Device Model.

```JSON
[
  { "example": "Device Model" },
  { "example": "Device Model" },
  { "example": "Device Model" }
]
```

## Device Model
### GET: Device by ID

This endpoint is used to get information about a specific device. It returns the same response as the above request for all devices of a certain type and group.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/devices/{device_id}' \
  --header 'authorization: Bearer {token}'
```
Response:
```JSON
{
  "id": "2197657034483041797",
  "address": "3c0030000247373430333032",
  "application_settings": {
    "date": "2020-05-26T23: 44: 00.922Z",
    "address": "3c0030000247373430333032",
    "user_id": "1719045011913311727",
    "settings": {
      "run": 0,
      "auto": 1,
      "update": 0,
      "tempStop": 4.44,
      "tempStart": 0.28,
      "sleepInterval": 900
    },
    "created_at": "2020-05-26T23: 33: 38.856Z",
    "updated_at": "2020-05-26T23: 34: 09.297Z"
  },
  "application_settings_new": {
    "id": "2317906584119805923",
    "date": "2020-05-26T23: 44: 00.922Z",
    "status": "onDevice",
    "address": "3c0030000247373430333032",
    "user_id": "1719045011913311727",
    "settings": {
      "tempStop": 4.44,
      "tempStart": 0.28
    },
    "created_at": "2020-05-26T23: 33: 38.856Z",
    "updated_at": "2020-05-26T23: 34: 09.297Z"
  },
  "customer_id": "1472639155912574382",
  "customer_id_old": "",
  "address_alias": "WM36662",
  "created_at": "2019-12-13T01: 39: 15.476Z",
  "updated_at": "2020-07-10T10: 26: 22.383Z",
  "installed_at": null,
  "interface": null,
  "interface_id": "1738173727062885911",
  "is_active": true,
  "activated_at": null,
  "deactivated_at": null,
  "monitor_status": "none",
  "physical": {
    "deviceType": "windMachine",
    "deviceGroup": "Wind Machine",
    "deviceNumber": "01S"
  },
  "configuration": { ... },
  "reading0": {
    "128": 26.75,
    "129": 0.8,
    "130": 0.635,
    ... (more reading data)
  },
  "reading1": {
    "128": 26.75,
    "129": 0.8,
    "130": 0.635
    ... (more reading data)
  },
  "interface_versioned": { ... }
}
```

#### Data Fields

* address
  - string
  - cellular network ID of device

* address_alias
  - string
  - display name of device

* application_settings
  - Object
  - current device configuration

* application_settings_new
  - Object
  - device configuration change values - either waiting to be applied, or the last change that has been applied

* auto 
  - integer
  - auto start mode

| value | description  |
|:-----:|:-------------|
|   1   | AUTO start   |
|   0   | MANUAL start |

* date
  - string ISO UTC date

* configuration
  - Object
  - Altrac internal data - please ignore

* customer_id
  - string

* customer_id_old
  - string
  - deprecated - please ignore

* deviceGroup
  - string
  - an arbitrary group of devices of a certain type that will be grouped together on UI panels and in reports

* deviceNumber
  - string
  - additional device alias for display to user in UI and reports

* deviceType 
  - string
  - equipment type controlled by device

| deviceType     | description                            |
|:---------------|:---------------------------------------|
| binDicator     | automated storage bin                  |
| coldAirDrain   | fan similar in purpose to wind machine |
| flow           | flow sensor                            |
| moistureSensor | moisture sensor                        |
| pump           | pump                                   |
| pumpFrostWater | water sprayer pump                     |
| temperature    | weather station                        |
| valve          | valve                                  |
| windMachine    | machine                                |

* interface
  - Object 
  - internal use - please ignore

* interface_id
  - string 
  - internal use - please ignore

* interface_versioned
  - Object 
  - internal use - please ignore

* monitor_status
  - string
 
|       value       | description                                   |
|:-----------------:|:----------------------------------------------|
|     noAlerts      | alerts are suspended for this device          |
|      triage       | problem detected requiring manual examination |
|       none        | monitored                                     |
| offlineSeasonally | alerts temporarily suspended                  |

* physical
  - Object 
  - low level detail elements of machine configuration

* reading0
  - Object
  - last raw device reading sent

* reading1
  - Object
  - second last raw device reading sent

* run
  - integer
 
| value | description |
|:-----:|:------------|
|   1   | running     |
|   0   | stopped     |
 - integer

* settings
  - Object
  - device configuration values

* sleep_interval 
  - integer seconds
  - interval between sending readings

* status
  - string
 
|  value   | description                                |
|:--------:|:-------------------------------------------|
|   new    | change has not been acknowledged by device |
| onDevice | change has been applied                    |

* tempStart
  - float °C
  - when ambient temperature is below this value start signal will be sent to the controlled equipment when machine is in AUTO START mode

* tempStop
  - float °C
  - when ambient temperature is above this value stop signal will be sent to the controlled equipment when machine is in AUTO START mode

* type
  - string
  - see ***deviceType***

* user_id
  - string
  - ID of user who performed last update

## Device Model
### GET: Devices by Address

Devices can also be retrieved using an **address** (cellular network identity). This endpoint is used primarily to obtain devices that share the same address. It returns an array of Devices that match the address provided. Multiple device IDs for the same address are used when device functionality needs to be split between multiple user interface objects.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/devices/address/{address}' \
  --header 'authorization: Bearer {token}'
```
Response:

The response is an array of devices depending on the number of devices that share the same address. The data for each device will match that outlined below in Device Model.

```JSON
[
  { "example": "Device Model", "address": "identical" },
  { "example": "Device Model", "address": "identical" },
  { "example": "Device Model", "address": "identical" }
]
```

## Reading Model
### GET: Readings for a Device
This endpoint supplies readings (sensor) information for a given device id or address

Query:

By device ID:
```Shell
curl --request GET \
  --url 'https://altrac-api.com/readings/deviceId/{deviceId}?=&dateBegin={date_begin}&dateEnd={date_end}' \
  --header 'authorization: Bearer {token}'
```
By device address:
```Shell
curl --request GET \
  --url 'https://altrac-api.com/readings/address/{address}?=&dateBegin={date_begin}&dateEnd={date_end}' \
  --header 'authorization: Bearer {token}'
```
Response:
```JSON
{
  "Items": [
    {
      "128": 17.87,
      "129": 0.79,
      "130": 0.575,
      "131": 0,
      "132": 1536875539,
      "133": 0.001,
      "134": 0,
      "135": 60,
      "140": 1,
      "143": 0,
      "179": 21785,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "address": "{address}",
      "date": "2018-10-02T16:58:50.000Z"
    },
    {
      "128": 17.87,
      "129": 0.79,
      "130": 0.575,
      "131": 0,
      "132": 1536875539,
      "133": 0.001,
      "134": 0,
      "135": 60,
      "140": 1,
      "143": 0,
      "179": 21785,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "address": "{address}",
      "date": "2018-10-02T16:57:27.000Z"
    },
    {
      "128": 17.87,
      "129": 0.79,
      "130": 0.575,
      "131": 0,
      "132": 1536875539,
      "133": 0.001,
      "134": 0,
      "135": 60,
      "140": 1,
      "143": 0,
      "179": 21785,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "address": "{address}",
      "date": "2018-10-02T16:56:06.000Z"
    },
    {
      "128": 17.87,
      "129": 0.79,
      "130": 0.575,
      "131": 0,
      "132": 1536875539,
      "133": 0.001,
      "134": 0,
      "135": 60,
      "140": 1,
      "143": 0,
      "179": 21785,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "address": "{address}",
      "date": "2018-10-02T16:54:45.000Z"
    },
    {
      "128": 17.75,
      "129": 0.79,
      "130": 0.575,
      "131": 0,
      "132": 1536875539,
      "133": 0.001,
      "134": 0,
      "135": 60,
      "140": 1,
      "143": 0,
      "179": 21785,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "address": "{address}",
      "date": "2018-10-02T16:53:23.000Z"
    }
  ],
  "Count": 5,
  "ScannedCount": 5
}
```

Fields Key:

| Field   | Purpose                                            | Unit / Value                                                                                                                       |
|---------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| 0       | Unused                                             | N/A                                                                                                                                |
| 1 - 100 | General purpose, including moisture sensor values  | %VWC                                                                                                                               |
| 128     | Ambient Temperature                                | °C                                                                                                                                 |
| 129     | Internal Battery                                   | %                                                                                                                                  |
| 130     | External Battery                                   | Multiply by 21.633 to get Voltage                                                                                                  |
| 131     | Engine State                                       | Running or Not Running                                                                                                             |
| 131     | machine-state                                      | (on/off boolean)                                                                                                                   |
| 131     | pump relay output                                  | (boolean)                                                                                                                          |
| 132     | Engine State Change Time                           | Time Since Epoch                                                                                                                   |
| 132     | machine-state                                      | timestamp                                                                                                                          |
| 133     | Fuel Level                                         | % of 5.5V. Find conversion factor in device interface and physical                                                                 |
| 133     | fuel-level                                         |                                                                                                                                    |
| 133     | pump pressure                                      |                                                                                                                                    |
| 134     | RPM                                                | Raw RPM value, find multiplier in device interface                                                                                 |
| 134     | tach-or-speed                                      |
| 134     | pump run-signal (boolean)                          |                                                                                                                                    |
| 134     | GPM???                                             |                                                                                                                                    |
| 134     | pump v1 pulse counter                              |                                                                                                                                    |
| 135     | sleep-time                                         | seconds                                                                                                                            |
| 136     | pressure                                           | psi                                                                                                                                |
| 137     | not saved                                          |                                                                                                                                    |
| 138     | power-current                                      | %                                                                                                                                  |
| 139     | power-frequency                                    | Hz                                                                                                                                 |
| 140     | Engine State                                       |   0-is in ECU delay, 1-is stopped, 2-MPC-20 controller in standby, 3-in prestart delay 1, 4-in checksafe, 5-in prestart delay 2, 6-crank on, 7-crank rest, 8-false start, 9-in warmup delay, 10-in line fill 1, 11-in line fill 2, 12-running loaded, 13-in cooldown delay, 14-energize to stop, 15-in spindown delay, 16-in wait to start delay |
| 140     | vapor pressure                                     | kPa                                                                                                                                |
| 140     | moisture-1                                         |                                                                                                                                    |
| 141     | shutdown-status                                    |                                                                                                                                    |
| 141     | barometer                                          | kPa                                                                                                                                |
| 141     | moisture-2                                         |                                                                                                                                    |
| 141     | pump flow                                          | 4-20mA                                                                                                                             |
| 142     | warning-status                                     |                                                                                                                                    |
| 142     | humidity                                           | %                                                                                                                                  |
| 142     | moisture-3                                         |                                                                                                                                    |
| 143     | Auto Switch                                        | Analog value of temperature switch. 0 - 10000 converts to 0 - 100%. Should be 100% when auto switch is in auto position            |
| 143     | controller-status-auto                             |                                                                                                                                    |
| 143     | solar                                              | W/m2                                                                                                                               |
| 144     | controller-status-auto (controller-status-modbus) |                                                                                                                                    |
| 144     | precipitation now                                         | mm                                                                                                                                 |
| 145     | pond level                                         |                                                                                                                                    |
| 145     | strikes                                            |                                                                                                                                    |
| 146     | strike dist                                        | km                                                                                                                                 |
| 147     | dew point                                          |                                                                                                                                    |
| 148     | wind direction                                     |                                                                                                                                    |
| 149     | wind gust                                          |                                                                                                                                    |
| 150     | pressure switch                                    |                                                                                                                                    |
| 151     | pressure switch                                    |                                                                                                                                    |
| 153     | north wind speed                                   | m/s                                                                                                                                |
| 154     | east wind speed                                    | m/s                                                                                                                                |
| 162     | pond level min today                               |                                                                                                                                    |
| 17 - 32 | salinity values                                    | ECe, dS/m                                                                                                                          |
| 179     | cellular dB                                        |                                                                                                                                    |
| 185     | power-fault                                        |                                                                                                                                    |
| 186     | power-state                                        |                                                                                                                                    |
| 187     | humidity temp                                      |                                                                                                                                    |
| 200     | Temperature Start Setting confirmed by device      | °C                                                                                                                                 |
| 201     | Temperature Stop Setting confirmed by device       | °C                                                                                                                                 |
| 202     | Auto mode confirmed by device                      | 1 = Auto, 0 = Manual                                                                                                               |
| 203     | manual run state confirmed by device               | On/Off boolean                                                                                                                     |
| 33 - 48 | temperature values                                 | °C                                                                                                                                 |
| date    | Date of Reading                                    | ISO-8601 date format                                                                                                               |

### GET: Readings for a Device -- Data Translated and Converted -- ***EXPERIMENTAL***
This endpoint supplies readings (sensor) information for a given device address or ID. The readings are translated and converted for ease of use

Query:


By device ID:
```Shell
curl --request GET \
  --url 'https://altrac-api.com/readings/deviceId/{address}?=&dateBegin={date_begin}&dateEnd={date_end}&convert=true' \
  --header 'authorization: Bearer {token}'
```
By device address:
```Shell
curl --request GET \
  --url 'https://altrac-api.com/readings/address/{address}?=&dateBegin={date_begin}&dateEnd={date_end}&convert=true' \
  --header 'authorization: Bearer {token}'
```
Response:
```JSON
[
  {
    "date": "2020-07-15T21:24:38.000Z",
    "temperature": 1,
    "engineState": 12,
    "callToRun": 1,
    "rpm": 2482,
    "batteryExternalV": 12.2,
    "fuelLevel": "Not Connected",
    "temperatureStart": 0,
    "temperatureStop": 1.1,
    "autoMode": 1
  },
  {
    "date": "2020-07-15T21:22:32.000Z",
    "temperature": 1,
    "engineState": 12,
    "callToRun": 1,
    "rpm": 2532,
    "batteryExternalV": 12.2,
    "fuelLevel": "Not Connected",
    "temperatureStart": 0,
    "temperatureStop": 1.1,
    "autoMode": 1
  },
  {
    "date": "2020-07-15T21:20:25.000Z",
    "temperature": 1,
    "engineState": 12,
    "callToRun": 1,
    "rpm": 2462,
    "batteryExternalV": 12.2,
    "fuelLevel": "Not Connected",
    "temperatureStart": 0,
    "temperatureStop": 1.1,
    "autoMode": 1
  },
  {
    "date": "2020-07-15T21:18:19.000Z",
    "temperature": 1,
    "engineState": 12,
    "callToRun": 1,
    "rpm": 2472,
    "batteryExternalV": 12.2,
    "fuelLevel": "Not Connected",
    "temperatureStart": 0,
    "temperatureStop": 1.1,
    "autoMode": 1
  }
]
```

## Settings Model
### POST: Create a new setting for a device

This endpoint is used to send a setting to a device. 

The endpoint automatically merges settings that are going to the same device which still have the status "new". For example, you could send a setting payload for a device that just has the "tempStart" settings, then follow it with a setting for a device with the "tempStop" setting. These two will be merged together in the backend and sent to the device in one payload. 

Once a setting is created for a device, the device model property "application_settings_new" will be updated with the setting status. Once the setting has been sent to the device, the device model properties "application_settings" and "application_settings_new" will be updated, reflecting status. To be sure a setting has been accepted, you should also check the latest reading on the device model for the properties "200", "201" and "202", please reference above for their meaning.

Query:

```Shell
curl --request POST \
  --url 'https://altrac-api.com/settings' \
  --header 'authorization: Bearer {token}'
  --header 'content-type: application/json' \
  --data '[
  {
    "address": "{address}",
    "status": "new",
    "settings": {
      "tempStart": -1,
      "tempStop": 2
    }
  }
]'
```
Response:
```javascript
[
  {
    "id": "{id}",
    "address": "{address}",
    "settings": {
      "tempStop": 2,
      "tempStart": -1
    },
    "status": "new",
    "user_id": "{user_id}",
    "created_at": "2018-11-10T21:14:50.494Z",
    "updated_at": "2018-11-10T21:15:50.440Z"
  }
]
```

Settings available for Wind Machines

* auto
  - integer

| value | description |
|:-----:|:------------|
|   0   | MANUAL      |
|   1   | AUTO        |

* run
  - integer
  - 0 is the only valid value for Wind Machines. They cannot be started remotely

| value | description |
|:-----:|:------------|
|   0   | stop        |

* sleepInterval
  - integer seconds

* tempStart
  - float °C
  - when ambient temperature is below this value start signal will be sent to the controlled equipment when machine is in AUTO START mode

* tempStop
  - float °C
  - when ambient temperature is above this value stop signal will be sent to the controlled equipment when machine is in AUTO START mode

* update
  - integer
  - device firmware auto update mode

| value | description |
|:-----:|:------------|
|   0   | OFF         |
|   1   | ON          |

Settings available for Pumps and Valves

***TBD***

