# Altrac API Documentation
## Authentication
Currently there is one way to authenticate to the Altrac API, User Credential Login. 
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
You will be able to use the token generated in the response in order to make the other documented queries.

## User Model
### GET: User Information
This endpoint supplies the customer_id needed for other queries below.

**_WARNING:_** This API endpoint will be changing in the near future:
- The endpoint location will be moving away from authID
- The customer_id is slated to change due to a normalization effort underway as of 2018-10-08

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
```javascript
{
  "applications": [
    {
      "name": "Wind Machines",
      "deviceType": "windMachine",
      "deviceGroup": "Ranch Rayo"
    }
  ],
  "created_at": "{date}",
  "updated_at": "{date}",
  "customer_name": "{name}",
  "id": "{customer_id}",
  "new_id": "{customer_id_future}",
  "security": {}
}
```

### GET: All Customer Devices by Type and Group

This endpoint returns all customer devices by Device Type and Device Group
- Device Type: This is the type of Altrac device -- an example would be `windMachine` which equals "Wind Machine". You can find values for this in the Customer Information query.
- Device Group: This is the group the device belongs to -- an example from Customer Information above would be `Ranch Rayo`. Groups are used primarily to distinguish between ranches.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/customers/{customer_id}/devices?deviceType={deviceType}&deviceGroup={deviceGroup}' \
  --header 'authorization: Bearer {token}'
```
Response:

The response below is one device. This will return an array of devices depending on the number of devices that meet the queries criteria.

```javascript
[
  {
    "id": "{device_id}",
    "address": "{address}",
    "application_settings": {
      "date": "{date}",
      "address": "{address}",
      "user_id": "{user_id}",
      "settings": {
        "auto": 1,
        "update": 0,
        "tempStop": 1.67,
        "tempStart": 0,
        "calibrate1": 500,
        "calibrate2": 1200,
        "sleepInterval": 0
      },
      "created_at": "{date}",
      "updated_at": "{date}"
    },
    "application_settings_new": {
      "id": "{setting_id}",
      "date": "{date}",
      "status": "onDevice",
      "address": "{address}",
      "user_id": "{user_id}",
      "settings": {
        "update": 0,
        "sleepInterval": 0
      },
      "created_at": "{date}",
      "updated_at": "{date}"
    },
    "customer_id": "{customer_id}",
    "customer_id_old": "",
    "address_alias": "{altrac_internal_name}",
    "created_at": "{date}",
    "updated_at": "{date}",
    "installed_at": null,
    "interface": null,
    "interface_id": "{interface_id}",
    "physical": {
      "rpmSensor": "default",
      "deviceType": "windMachine",
      "deviceGroup": "Ranch Rayo",
      "deviceNumber": "{device_user_name}",
      "rpmMultiplier": 0.6535947712418301
    },
    "configuration": {
      "pcb": "endpoint_0305",
      "imei": "{imei}",
      "model": "windMachineOrchardRiteV10",
      "sales": {
        "invoice": "{invoice_number}"
      },
      "firmware": {
        "osVersion": 393472,
        "productId": 7725,
        "application": 30575,
        "boardVersion": 340,
        "applicationVersion": 44
      },
      "simIccid": "{sim_iccid}",
      "enclosure": "1.2B",
      "pcbWiring": "1.3",
      "subassemblies": [
        {
          "type": "Wiring Harness",
          "model": "1.2"
        },
        {
          "type": "Control Cable",
          "model": "1.3b"
        },
        {
          "type": "Temperature Probe",
          "model": "1.3"
        }
      ],
      "particleSerialNumber": "{serial_number}"
    },
    "reading0": {
      "128": 15,
      "129": 0.78,
      "130": 0.555,
      "131": 0,
      "132": 1536880608,
      "133": 0.001,
      "134": 0,
      "135": 0,
      "140": 1,
      "143": 10000,
      "179": 22297,
      "185": 128,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "date": "{date}",
      "128Date": "{date}",
      "129Date": "{date}",
      "130Date": "{date}",
      "131Date": "{date}",
      "132Date": "{date}",
      "133Date": "{date}",
      "134Date": "{date}",
      "135Date": "{date}",
      "140Date": "{date}",
      "143Date": "{date}",
      "179Date": "{date}",
      "185Date": "{date}",
      "186Date": "{date}",
      "187Date": "{date}",
      "200Date": "{date}",
      "201Date": "{date}",
      "202Date": "{date}",
      "address": "{address}"
    },
    "reading1": {
      "128": 15,
      "129": 0.78,
      "130": 0.555,
      "131": 0,
      "132": 1536880608,
      "133": 0.001,
      "134": 0,
      "135": 0,
      "140": 1,
      "143": 10000,
      "179": 22297,
      "185": 128,
      "186": 52,
      "187": -10000,
      "200": 0,
      "201": 1.67,
      "202": 1,
      "date": "{date}",
      "128Date": "{date}",
      "129Date": "{date}",
      "130Date": "{date}",
      "131Date": "{date}",
      "132Date": "{date}",
      "133Date": "{date}",
      "134Date": "{date}",
      "135Date": "{date}",
      "140Date": "{date}",
      "143Date": "{date}",
      "179Date": "{date}",
      "185Date": "{date}",
      "186Date": "{date}",
      "187Date": "{date}",
      "200Date": "{date}",
      "201Date": "{date}",
      "202Date": "{date}",
      "address": "{address}"
    },
    "interface_versioned": {
      "data": {
        "tile": {
          "unit": "Temperature",
          "formula": "temperature",
          "valueKey": "128",
          "multiplier": 1,
          "unitAbbreviation": "°"
        },
        "other": {
          "controller": "callToStart"
        },
        "primaryPage": {
          "chart": "windMachine",
          "rules": false,
          "settingTypes": [
            "temperatureSet"
          ],
          "physicalTypes": [],
          "telemetryTypes": [
            "settingAutoModeHauff",
            "settingEngineState",
            "rpm",
            "temperature",
            "batteryExternal"
          ]
        },
        "advancedPage": {
          "rules": true,
          "settingTypes": [],
          "physicalTypes": [
            "deviceNumber",
            "deviceGroup"
          ],
          "telemetryTypes": [
            {
              "unit": "%",
              "label": "Internal Battery",
              "formula": "default",
              "valueKey": "129",
              "multiplier": 0.01
            },
            {
              "unit": "",
              "label": "RSSI dB",
              "formula": "cellSignalToRssi",
              "valueKey": "179",
              "multiplier": 1
            }
          ]
        }
      },
      "type": "windMachine",
      "subtype": "orchardRite",
      "version": "1",
      "created_at": "{date}",
      "updated_at": "{date}",
      "id": "{interface_id}"
    }
  }
]
```

## Device Model
### GET: Device by ID
**_WARNING:_** This API endpoint will be changing in the near future:
- The customer_id will be changing for a normalization effort
- The address will be removed in for a normalization effort around the device ID

This endpoint is used to get information about a specific device. It returns the same response as the above request for all devices of a certain type and group.

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/devices/{device_id}' \
  --header 'authorization: Bearer {token}'
```
Response:
```javascript
{
  "id": "{device_id}",
  "address": "{address}",
  "application_settings": {
    "date": "{date}",
    "address": "{address}",
    "user_id": "{user_id}",
    "settings": {
      "auto": 1,
      "update": 0,
      "tempStop": 1.67,
      "tempStart": 0,
      "calibrate1": 500,
      "calibrate2": 1200,
      "sleepInterval": 0
    },
    "created_at": "{date}",
    "updated_at": "{date}"
  },
  "application_settings_new": {
    "id": "{setting_id}",
    "date": "{date}",
    "status": "onDevice",
    "address": "{address}",
    "user_id": "{user_id}",
    "settings": {
      "update": 0,
      "sleepInterval": 0
    },
    "created_at": "{date}",
    "updated_at": "{date}"
  },
  "customer_id": "{customer_id}",
  "customer_id_old": "",
  "address_alias": "{altrac_internal_name}",
  "created_at": "{date}",
  "updated_at": "{date}",
  "installed_at": null,
  "interface": null,
  "interface_id": "{interface_id}",
  "physical": {
    "rpmSensor": "default",
    "deviceType": "windMachine",
    "deviceGroup": "Ranch Rayo",
    "deviceNumber": "{device_user_name}",
    "rpmMultiplier": 0.6535947712418301
  },
  "configuration": {
    "pcb": "endpoint_0305",
    "imei": "{imei}",
    "model": "windMachineOrchardRiteV10",
    "sales": {
      "invoice": "{invoice_number}"
    },
    "firmware": {
      "osVersion": 393472,
      "productId": 7725,
      "application": 30575,
      "boardVersion": 340,
      "applicationVersion": 44
    },
    "simIccid": "{sim_iccid}",
    "enclosure": "1.2B",
    "pcbWiring": "1.3",
    "subassemblies": [
      {
        "type": "Wiring Harness",
        "model": "1.2"
      },
      {
        "type": "Control Cable",
        "model": "1.3b"
      },
      {
        "type": "Temperature Probe",
        "model": "1.3"
      }
    ],
    "particleSerialNumber": "{serial_number}"
  },
  "reading0": {
    "128": 15,
    "129": 0.78,
    "130": 0.555,
    "131": 0,
    "132": 1536880608,
    "133": 0.001,
    "134": 0,
    "135": 0,
    "140": 1,
    "143": 10000,
    "179": 22297,
    "185": 128,
    "186": 52,
    "187": -10000,
    "200": 0,
    "201": 1.67,
    "202": 1,
    "date": "{date}",
    "128Date": "{date}",
    "129Date": "{date}",
    "130Date": "{date}",
    "131Date": "{date}",
    "132Date": "{date}",
    "133Date": "{date}",
    "134Date": "{date}",
    "135Date": "{date}",
    "140Date": "{date}",
    "143Date": "{date}",
    "179Date": "{date}",
    "185Date": "{date}",
    "186Date": "{date}",
    "187Date": "{date}",
    "200Date": "{date}",
    "201Date": "{date}",
    "202Date": "{date}",
    "address": "{address}"
  },
  "reading1": {
    "128": 15,
    "129": 0.78,
    "130": 0.555,
    "131": 0,
    "132": 1536880608,
    "133": 0.001,
    "134": 0,
    "135": 0,
    "140": 1,
    "143": 10000,
    "179": 22297,
    "185": 128,
    "186": 52,
    "187": -10000,
    "200": 0,
    "201": 1.67,
    "202": 1,
    "date": "{date}",
    "128Date": "{date}",
    "129Date": "{date}",
    "130Date": "{date}",
    "131Date": "{date}",
    "132Date": "{date}",
    "133Date": "{date}",
    "134Date": "{date}",
    "135Date": "{date}",
    "140Date": "{date}",
    "143Date": "{date}",
    "179Date": "{date}",
    "185Date": "{date}",
    "186Date": "{date}",
    "187Date": "{date}",
    "200Date": "{date}",
    "201Date": "{date}",
    "202Date": "{date}",
    "address": "{address}"
  },
  "interface_versioned": {
    "data": {
      "tile": {
        "unit": "Temperature",
        "formula": "temperature",
        "valueKey": "128",
        "multiplier": 1,
        "unitAbbreviation": "°"
      },
      "other": {
        "controller": "callToStart"
      },
      "primaryPage": {
        "chart": "windMachine",
        "rules": false,
        "settingTypes": [
          "temperatureSet"
        ],
        "physicalTypes": [],
        "telemetryTypes": [
          "settingAutoModeHauff",
          "settingEngineState",
          "rpm",
          "temperature",
          "batteryExternal"
        ]
      },
      "advancedPage": {
        "rules": true,
        "settingTypes": [],
        "physicalTypes": [
          "deviceNumber",
          "deviceGroup"
        ],
        "telemetryTypes": [
          {
            "unit": "%",
            "label": "Internal Battery",
            "formula": "default",
            "valueKey": "129",
            "multiplier": 0.01
          },
          {
            "unit": "",
            "label": "RSSI dB",
            "formula": "cellSignalToRssi",
            "valueKey": "179",
            "multiplier": 1
          }
        ]
      }
    },
    "type": "windMachine",
    "subtype": "orchardRite",
    "version": "1",
    "created_at": "{date}",
    "updated_at": "{date}",
    "id": "{interface_id}"
  }
}
```
## Reading Model
### GET: Readings for a Device
This endpoint supplies readings (sensor) information for a given device address

**_WARNING:_** This API endpoint will be changing in the near future:
- The address will be removed from these responses in the future in favor of the device's id

Query:

```Shell
curl --request GET \
  --url 'https://altrac-api.com/readings/address/{address}?=&dateBegin={date_begin}&dateEnd={date_end}' \
  --header 'authorization: Bearer {token}'
```
Response:
```javascript
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
| 140     | Engine State                                       | 1-12 value of engine states. To be documented in detail later. 1 = Idle, 6 = Warmup, 8 = False Start, 12 = Full Run, 13 = Cooldown |
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
| 144     | controller-status-auto (controller-status-modbus?) |                                                                                                                                    |
| 144     | precip now                                         | mm                                                                                                                                 |
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
| 200     | Temperature Start Setting confirmed by device      | C                                                                                                                                  |
| 201     | Temperature Stop Setting confirmed by device       | C                                                                                                                                  |
| 202     | Auto mode confirmed by device                      | 1 = Auto, 0 = Manual                                                                                                               |
| 203     | manual run state confirmed by device               | On/Off boolean                                                                                                                     |
| 33 - 48 | temperature values                                 | °C                                                                                                                                 |
| date    | Date of Reading                                    | ISO-8601 date format                                                                                                               |


## Settings Model
### POST: Create a new setting for a device
**_WARNING:_** This API endpoint will be changing in the near future:
- The address field will be changed to device_id related to our normalization effort around the device ID

This endpoint is used to send a setting to a device. 

The endpoint automatically merges settings that are going to the same devie which still have the status "new". For example, you could send a setting payload for a device that just has the "tempStart" settings, then follow it with a setting for a device with the "tempStop" setting. These two will be merged together in the backend and sent to the device in one payload. 

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

Settings Avaiable for Wind Machines

|Field|Purpose|Unit / Value|
|------|-------|------|
|tempStart|Set the start temperature|Celsius, must be less than tempStop or device will overwrite|
|tempStop|Set the stop temperature|Celsius, must be greater than tempStart or device will overwrite|
|auto|Turn Altrac device auto mode on or off|Auto = 1, Manual = 0|
|run|Turn machine off|0 = Turn machine off, sending 1 for manual run will have no effect, by design.|
