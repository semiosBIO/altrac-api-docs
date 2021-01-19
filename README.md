# altrac-api-client

## API Client NodeJS library

Initially Altrac is providing a NodeJS library capable of communicating with the Altrac System.

### API Version
To avoid incompatibilities between versions of the back-end code a version ID in the form `YYYY.MM.DD` may be provided, there is only 1 currently available version - `2018.11.30`.

The API version is set in the HTTP request header as the variable `accept-version`, example:

```accept-version: 2018.11.30```

## Credentials

Access to the Altrac system is controlled via credentials.

Credentials are generated uniquely for each customer at the customer's request and are not stored within Altrac. If credentials for a customer are lost Altrac must be contacted to generate a new set.

Credentials are composed of: 
* API_KEY
    - identifies the access to data for _a single customer_

* API_SECRET
    - password for the access to the individual customer

* API_VERSION
    - as discussed above

A successful authorization will return the relevant customer_id.

### secrets.js

This file provides the credentials to the api-client library to be used by the example code `example.js`. 

**This file is not used in a production system.**

The ```secrets.js``` file should be edited to include the API_KEY and API_SECRET provided, example: 
```javascript
const API_KEY='a7dace06dbe8be7100de7c78b6580f07'
const API_SECRET='jMevPbh4NCFb1qW4ikkUzw=='
module.exports = { API_KEY, API_SECRET };
```

## Entities

The ```example.js``` file illustrates the use of the following access functions for entities in the Altrac system.

All requests to the Altrac client api return data encoded in JSON format.

### AltracClient Object
AltracClient is a Javascript Class that can be used to control the interaction with the Altrac API.

Each instance of AlracClient implements HTTP communication while maintaining the authentication token for a single customer.

For each customer a new instance of the object should be created with the customer's credentials provided to the constructor:

```javascript
const altracClientForCustomerA = new AltraClient(customerACredentials);
```
During construction the client instance will automatically authenticate with the API back end, and will keep the authentication current for all future communication.

see: [AlracClient detailed documentation](http://github.com/altrac/altrac-api-client/docs/AltracClient.html)
### Customers
#### Reading the Customer Record

In the Altrac system each Customer has detailed information that can be accessed.
```javascript
let customer = await altracClient1
  .get("customers", { id: customerID })
  .catch(error => console.log(error));
```
example of Customer record:  
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
      "deviceGroup": "Wind Machine"
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

#### Reading the Customer's Device Details

Altrac Devices control one or more pieces of equipment.

Typically customers who own many devices can *group* devices together for easier organization of their view into the system.

To request a collection of *all* of the devices owned by a customer the **deviceGroup** parameter may be left out in the following example.

If converted readings are needed, pass the param convert=true

```javascript
  const windMachineGroup = customer.applications.find(
    (group) => group.name === "Wind Machines"
  );
  let groupName = windMachineGroup.name;
  let groupID = windMachineGroup.groupID;

  let params = { convert: true, includeDevices: true };

  // get a list of devices in a customer's group
  let group = await altracClient1
    .get("groups", { id: groupID, params })
    .catch((error) => handleError(error));

  let { devices: deviceList } = group;

```
### Devices

#### Device details from deviceID

A single device record can be retrieved using its **deviceID**.
```javascript
let device = await altracClient1
  .get("devices", {
    id: deviceID,
    params: { convert: true, is_active: true },
  })
  .catch((error) => handleError(error));
```
example of Device record returned (condensed for values that can be ignored): 
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
  - If the option convert = true is passed, reading0 will be converted as per below

* reading1
  - Object
  - second last raw device reading sent
  - If the option convert = true is passed, reading0 will be converted as per below

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

#### Device details from device address

A device can also be retrieved using its **address** (cellular network identity).
```javascript
deviceList = await altracClient1
  .get(`devices/address/${address}`, { params: { convert: true } })
  .catch((error) => handleError(error));
```
### Readings

Devices report their status periodically as Readings. The frequency at which they report is based on a number of factors including whether the equipment that they are controlling is currently operating, the state of the internal battery and possibly other factors such as weather conditions, date and time.

Readings include values from sensors, control input and output values and device diagnostics.

#### Reading device telemetry data

##### getReadingsWithAddress
Wind Machine sensor data readings may be retrieved using the device **address**.

***WARNING*** The below function and this library use the flag `convert: true`. By setting this flag to true, our API is doing a conversion from raw data to easily parsable data. The feature is new and ***EXPERIMENTAL***. We welcome input on the feature in order to make it robust and easily understandable.

```javascript
const readingsWithAddress = await altracClient1
  .get(`readings/address/${address}`, { params: { customerID, convert: true } })
  .catch((error) => handleError(error));

```

##### getReadingsWithDeviceId
Wind Machine sensor data readings may be retrieved using **deviceID**.

***WARNING*** The below function and this library use the flag `convert: true`. By setting this flag to true, our API is doing a conversion from raw data to easily parsable data. The feature is new and ***EXPERIMENTAL***. We welcome input on the feature in order to make it robust and easily understandable.

```javascript
const readingsWithDeviceID = await altracClient1
  .get(`readings/deviceId/${deviceID}`,
    {
      params: {
        dateBegin,
        dateEnd,
        convert: true
      }
    }
  )
  .catch((error) => handleError(error));
```

Example of wind machine Reading response: 
```JSON
[
  {
    "date": "2020-07-10T19: 46: 45.000Z",
    "temperature": 26.6,
    "internalBattery": 80,
    "rssiDb": 69,
    "engineState": 1,
    "running": 0,
    "rpm": 0,
    "batteryExternalV": 13.7,
    "fuelLevel": "Not Connected",
    "temperatureStart": 0.3,
    "temperatureStop": 4.4,
    "autoMode": 1
  },
  ... (additional readings)
]
```
*Note*:  returned reading data that is invalid will be indicated by the value "ERR".

#### Data Fields
* date
  - string ISO UTC date

* engineState
  - integer value in range 0-16

| value | description                  |
|:-----:|:-----------------------------|
|  0    | Is in ECU delay              |
|  1    | Is stopped                   |
|  2    | Controller in standby |
|  3    | In prestart delay 1          |
|  4    | In checksafe                 |
|  5    | In prestart delay 2          |
|  6    | Crank on                     |
|  7    | Crank rest                   |
|  8    | False start                  |
|  9    | In warmup delay              |
| 10    | In line fill 1               |
| 11    | In line fill 2               |
| 12    | Running loaded               |
| 13    | In cooldown delay            |
| 14    | Energize to stop             |
| 15    | In spindown delay            |
| 16    | In wait to start delay       |
| >16   | Unexpected engine state      |

Example code for condensing this into five understandable states:
```javascript
switch (engineState) {
  case 0:
  case 1:
  case 2:
    engineState = MACHINE_OFF;
    if (engineStateTransition) { engineState = engineStateTransition === 2 ? MACHINE_OFF : MACHINE_RUN; }
    tileColor = colors.normal;
    break;
  case 3:
  case 4:
  case 5:
  case 6:
  case 7:
  case 8:
    engineState = MACHINE_START;
    if (engineStateTransition) { engineState = engineStateTransition === 2 ? MACHINE_STOP : MACHINE_START; }
    tileColor = colors.info;
    break;
  case 9:
    engineState = MACHINE_WARMUP;
    if (engineStateTransition) { engineState = engineStateTransition === 2 ? MACHINE_STOP : MACHINE_WARMUP; }
    tileColor = colors.info;
    break;
  case 10:
  case 11:
  case 12:
    engineState = MACHINE_FULLRUN;
    if (engineStateTransition) { engineState = engineStateTransition === 2 ? MACHINE_STOP : MACHINE_FULLRUN; }
    tileColor = colors.success;
    break;
  case 13:
  case 14:
  case 15:
  case 16:
    engineState = MACHINE_COOLDOWN;
    if (engineStateTransition) { engineState = engineStateTransition === 2 ? MACHINE_COOLDOWN : MACHINE_RUN; }
    tileColor = colors.info;
    break;
  default:
    engineState = '-';
    tileColor = colors.danger;
}
```

* running
  - integer value 0-1
  - 0 = Machine is running and/or machine should be running
  - 1 = Machine is stopped and/or machine should be stopped
  - Note: The output of this value is dependent on auto mode. 
    - If the Altrac is in auto mode -- 1 will correspond to when the Altrac is telling the machine to run and 0 will correspond to when Altrac is telling the machine to stop
    - If the Altrac is in manual mode -- 1 will correspond to when the Altrac senses the machine is running and 0 will correspond to when Altrac senses the machine is stopped. This is used to capture times when the person operating the wind machine is running the wind machine manually 

* fuelLevel
  - float in range 0.0 - 1.0 (empty to full)
  - string value indicates warning or error condition, examples: "ERR", "Not Connected", …
  - relative level of fuel in tank

* internalBattery
  - integer % (percent)
  - state of charge of Altrac device internal battery

* rpm
  - integer
  - engine RPM (revolutions per minute) at time of reading

* rssiDb
  - integer decibels
  - cellular radio signal strength

* temperatureStart
  - float °C
  - when ambient temperature is below this value start signal will be sent to the controlled equipment when machine is in AUTO START mode

* temperatureStop
  - float °C
  - when ambient temperature is above this value stop signal will be sent to the controlled equipment when machine is in AUTO START mode

* autoMode 
  - integer

| value | description  |
|:-----:|:-------------|
|   0   | MANUAL       |
|   1   | AUTO         |

* temperature
  - float °C
  - ambient atmospheric temperature recorded by Altrac device attached sensor

### Settings
#### Updating device settings
```javascript
const newSettings = {
  address,
  settings: {
    tempStart: 2, // set start temperature °C
    tempStop: 3, // set stop temperature °C
    sleepInterval: 900, // set interval between readings (seconds)
  }
};

const settingsResult = await altracClient1
  .post(`settings`, { address }, newSettings)
  .catch((error) => handleError(error));
```

#### Data Fields

##### Settings available for Wind Machines

* auto
  - integer

| value | description  |
|:-----:|:-------------|
|   0   | MANUAL  |
|   1   | AUTO    |

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


##### Settings available for Pumps and Valves

***TBD***
