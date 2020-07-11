# Readme

## API Client NodeJS library

Initially Altrac is providing a NodeJS library capable of communicating with the Altrac System.

## Credentials

Access to the Altrac system is controlled via credentials.
Credentials will be : 
* API_KEY
    - identifies the access to data for _a single customer_

* API_SECRET
    - password for the access to the individual customer

### secrets.js

This file provides the credentials to the api-client library.

The ```secrets.js``` file should be edited to include the API_KEY and API_SECRET provided, example: 
```javascript
const API_KEY='a7dace06dbe8be7100de7c78b6580f07'
const API_SECRET='jMevPbh4NCFb1qW4ikkUzw=='
module.exports = { API_KEY, API_SECRET };
```

## Entities

The ```example.js``` file illustrates the use of the following access functions for entities in the Altrac system.

All requests to the Altrac client api return data encoded in JSON format.

### Customers

#### Reading the Customer Record

In the Altrac system each Customer has detailed information that can be accessed.
```javascript
/**
 * @function getCustomer
 * @description request a customer's details.
 * @param {Credentials} credentials
 * @param {string} customerID
 * @returns {Array<Device>}
 */
 const getCustomer = async (credentials, customerID) => {
  try {
    const {
      customers
    } = altrac(credentials);

    const customer = await customers.get(customerID);
    return customer;
  } catch (e) {
    console.error(`\nERROR getDevicesInDeviceGroup ${e}\n`);
  }
}
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
  | :-----      | :---------                                            |
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
  | :---------     | :----------                            |
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
  | :---: | :----------                             |
  | true  | supported by the system                 |
  | false | customer is not supported by the system |

#### Reading the Customer's Device Details

Altrac Devices control one or more pieces of equipment.

Typically customers who own many devices can *group* devices together for easier organization of their view into the system.

To request a collection of *all* of the devices owned by a customer the **deviceGroup** parameter may be left out in the following example.

```javascript
/**
 * @function getDevicesInDeviceGroup
 * @description request a collection of a customer's devices within a specific device group.
 * @param {Credentials} credentials
 * @param {string} customerID
 * @param {string} deviceType
 * @param {string} deviceGroup
 * @returns {Array<Device>}
 */
 const getDevicesInDeviceGroup = async (credentials, customerID, deviceType, deviceGroup) => {
  try {
    const {
      customers
    } = altrac(credentials);

    const deviceList = await customers.getDevices(customerID, {
      params:  {
        deviceType,
        deviceGroup
      },
      path:  `devices`,
    });
    return deviceList;
  } catch (e) {
    console.error(`\nERROR getDevicesInDeviceGroup ${e}\n`);
  }
}
```
### Devices

#### Reading device details using deviceID

A single device record can be retrieved using its **deviceID**.
```javascript
/**
 * @function getADevice
 * @description request a specific device
 * @param {Credentials} credentials
 * @param {string} deviceID
 * @returns {Device}
 */
const getADevice = async (credentials, deviceID) => {
  try {
    const {
      devices,
    } = altrac(credentials);

    const device = await devices.get(deviceID);

    return device;
  } catch (e) {
    console.error(`\nERROR getADevice ${e}\n`);
  }
}
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
  | :---: | :----------  |
  | 1     | AUTO start   |
  | 0     | MANUAL start |

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
  | :---------     | :----------                            |
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

* monitor_staus
  - string
 
  | value             | description                                   |
  | :---:             | :----------                                   |
  | noAlerts          | alerts are suspended for this device          |
  | triage            | problem detected requiring manual examination |
  | none              | monitored                                     |
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
  | :---: | :---------- |
  | 1     | running     |
  | 0     | stopped     |
 - integer

* settings
  - Object
  - device configuration values

* sleep_inteval 
  - integer seconds
  - interval between sending readings

* status
  - string
 
  | value    | description                                |
  | :---:    | :----------                                |
  | new      | change has not been acknowledged by device |
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

#### Reading device details using device address

A device can also be retrieved using its **address** (cellular network identity).
```javascript

/**
 * @function getADeviceWithAddress
 * @description request a specific device using device address
 * @param {Credentials} credentials
 * @param {string} address
 * @returns {Device}
 */
const getADeviceWithAddress = async (credentials, address) => {
  try {
    const {
      devices,
    } = altrac(credentials);

    const deviceList = await devices.getOnAddress(address);

    return deviceList;
  } catch (e) {
    console.error(`\nERROR getADeviceWithAddress ${e}\n`);
  }
}
```
### Settings

### Readings

Devices report their status periodically as Readings, the frequency that they report on is based on a number of factors including whether the equipment that they are controlling is currently operating, the state of the internal battery and possibly other factors such as weather conditions, date and time.

Readings include values from sensors, control input and output values and device diagnostics.

#### Reading device telemetry data

Wind Machine sensor data readings must be retrieved using the device **address**.

```javascript

/**
 * @function getReadingsWithAddress
 * @description request Readings for device using device address
 * @param {Credentials} credentials
 * @param {string} address
 * @returns {Array<Reading>}
 */
 const getReadingsWithAddress = async (credentials, customerID, address) => {
  try {
    const {
      readings,
    } = altrac(credentials);

    // get readings for 1 day previous
    // const lengthOfDay = 24 * 60 * 60 * 1000; // milliseconds
    // const now = new Date().getTime();
    // const dateBegin = new Date(now - (2 * lengthOfDay));
    // const dateEnd = new Date(now - lengthOfDay - 1);

    const readingsList = await readings.getOnAddress(address, {
      params:  {
        customerID,
        // dateBegin,
        // dateEnd,
        limit:  10,
        convert:  true, // if undefined raw data will be retrieved 
      }
    });
    return readingsList;
  } catch (e) {
    console.error(`\nERROR getReadingsWithAddress ${e}\n`);
  }
}
```

example of wind machine Reading record: 
```JSON
  {
    "date": "2020-07-10T19: 46: 45.000Z",
    "temperature": 26.6,
    "internalBattery": 80,
    "rssiDb": 69,
    "engineState": 1,
    "engineStateIntention": 0,
    "rpm": 0,
    "batteryExternalV": 13.7,
    "fuelLevel": "Not Connected",
    "startTemperature": 0.3,
    "stopTemperature": 4.4,
    "startMode": 1
  }
```
*Note*:  returned reading data that is invalid will be indicated by the value "ERR".

#### Data Fields
* date
  - string ISO UTC date

* deviceGroup
  - string
  - an arbitrary group of devices of a certain type that will be grouped together on UI panels and in reports

* deviceType 
  - string
  - equipment type controlled by device

  | deviceType     | description                            |
  | :---------     | :----------                            |
  | binDicator     | automated storage bin                  |
  | coldAirDrain   | fan similar in purpose to wind machine |
  | flow           | flow sensor                            |
  | moistureSensor | moisture sensor                        |
  | pump           | pump                                   |
  | pumpFrostWater | water sprayer pump                     |
  | temperature    | weather station                        |
  | valve          | valve                                  |
  | windMachine    | machine                                |

* engineState
  - integer

  | value | description |
  | :---: | :---------- |
  | 1     | running     |
  | 0     | stopped     |

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

* startTemperature
  - float °C
  - when ambient temperature is below this value start signal will be sent to the controlled equipment when machine is in AUTO START mode

* stopTemperature
  - float °C
  - when ambient temperature is above this value stop signal will be sent to the controlled equipment when machine is in AUTO START mode

* startMode 
  - integer

  | value | description  |
  | :---: | :----------  |
  | 1     | AUTO start   |
  | 0     | MANUAL start |

* temperature
  - float °C
  - ambient atmospheric temperature recorded by Altrac device attached sensor
