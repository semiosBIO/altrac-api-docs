# Readme
## API Client NodeJS library
Initially Altrac is providing a NodeJS library capable of communicating with the Altrac System.

## Credentials
Access to the Altrac system is controlled via credentials.
Credentials will be :
* ALTRAC_KEY
    - identifies the access to data for _a single customer_

* ALTRAC_SECRET
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
example of returned data: 
```JSON
{
  "id":"112345678901234567890",
  "applications":[
    {
      "name":"Moisture",
      "deviceType":"moistureSensor",
      "deviceGroup":"Moisture"
    },{
      "name":"Pumps",
      "deviceType":"pump",
      "deviceGroup":"Pumps"
    },{
      "name":"Valves",
      "deviceType":"valve",
      "deviceGroup":"Valves"
    },{
      "name":"Wind Machines",
      "deviceType":"windMachine",
      "deviceGroup":"Wind Machine"
    },{
      "name":"Weather",
      "deviceType":"temperature",
      "deviceGroup":"Weather"
    }
  ],
  "customer_name":"ABC Fruit",
  "groups_enabled":false,
  "is_active":true,
  "created_at":"2015-11-10T19:49:59.029Z",
  "updated_at":"2020-04-30T17:18:46.873Z",
  "security":null,
  "brand":{
    "name":"altrac",
    "display_name":"Altrac",
    "css":"{ body { color:#284047;background-color:#d9d9d9; }",
      "sales_email":"sales@altrac.io",
      "sales_phone":"+1 510 248 4141",
      "support_email":"support@altrac.io",
      "support_phone":"+1 510 248 4141",
      "locale":{
        "language":"en",
        "tempConv":"f",
        "timeZone":"US/Los Angeles"
      },
      "splash_img":"...jpg",
      "icon_png":"...png",
      "icon_svg":"<svg >...</svg>",
      "banner_svg":"<svg>...</svg>",
      "created_at":"2019-08-29T19:27:04.407Z",
      "updated_at":"2019-08-29T19:27:04.407Z"
    }
  }
```
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
      params: {
        deviceType,
        deviceGroup
      },
      path: `devices`,
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
example of returned data:
```JSON
{
  "id":"2197657034483041797",
  "address":"3c0030000247373430333032",
  "application_settings":{
    "date":"2020-05-26T23:44:00.922Z",
    "address":"3c0030000247373430333032",
    "user_id":"1719045011913311727",
    "settings":{
      "run":0,
      "auto":1,
      "update":0,
      "tempStop":4.44,
      "tempStart":0.28,
      "sleepInterval":900
    },
    "created_at":"2020-05-26T23:33:38.856Z",
    "updated_at":"2020-05-26T23:34:09.297Z"
  },
  "application_settings_new":{
    "id":"2317906584119805923",
    "date":"2020-05-26T23:44:00.922Z",
    "status":"onDevice",
    "address":"3c0030000247373430333032",
    "user_id":"1719045011913311727",
    "settings":{
      "tempStop":4.44,
      "tempStart":0.28
    },
    "created_at":"2020-05-26T23:33:38.856Z",
    "updated_at":"2020-05-26T23:34:09.297Z"
  },
  "customer_id":"1472639155912574382",
  "customer_id_old":"",
  "address_alias":"WM36662",
  "created_at":"2019-12-13T01:39:15.476Z",
  "updated_at":"2020-07-10T10:26:22.383Z",
  "installed_at":null,
  "interface":null,
  "interface_id":"1738173727062885911",
  "is_active":true,
  "activated_at":null,
  "deactivated_at":null,
  "monitor_status":"none",
  "physical":{
    "deviceType":"windMachine",
    "deviceGroup":"Wind Machine",
    "deviceNumber":"01S"
  },
  "configuration":{
    "pcb":"endpoint_0305",
    "imei":"356726107636193",
    "model":"windMachineCascade",
    "sales":{ "invoice":"INV-234567890" },
    "simIccid":"89014103271529155978",
    "subassemblies":[
      { "type":"SN100_40_1.3.0" },
      { "type":"WH104_1.0.0" },
      { "type":"ST100.WH104_1.0.0" }
    ],
    "controlLineSerialNumber":"19460294"
  },
  "reading0":{
    "128":26.75,
    "129":0.8,
    "130":0.635,
  },
  "reading1":{
    "128":26.75,
    "129":0.8,
    "130":0.635
  },
  "interface_versioned":{
    "data":{
      "tile":{
        "unit":"Temperature",
        "formula":"temperature",
        "valueKey":"128",
        "multiplier":1,
        "unitAbbreviation":"°"
      },
      "other":{
        "controller":"callToStart"
      },
      "primaryPage":{
        "chart":"windMachine",
        "rules":false,
        "settingTypes":[ "temperatureSet" ],
        "physicalTypes":[],
        "telemetryTypes":[
          "settingAutoModeHauff",
          "settingEngineState",
          "rpm",
          "temperature",
          "batteryExternal",
          "fuelLevel"
        ]
      },
      "advancedPage":{
        "rules":true,
        "settingTypes":[],
        "physicalTypes":[
          "deviceNumber",
          "deviceGroup"
        ],
        "telemetryTypes":[
        {
          "unit":"%",
          "label":"Internal Battery",
          "formula":"default",
          "valueKey":"129",
          "multiplier":0.01
        },{
          "unit":"",
          "label":"RSSI dB",
          "formula":"cellSignalToRssi",
          "valueKey":"179",
          "multiplier":1
        }
        ]
      }
    },
    "type":"windMachine",
    "subtype":"cascade_withFuel",
    "version":"1",
    "created_at":"2018-03-19T02:27:56.752Z",
    "updated_at":"2019-07-05T18:28:32.870Z",
    "id":"1738173727062885911"
  }
}
```
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
#### Reading device telemetry data
Wind Machine sensor data readings must be retrieved using the device **address**.

```javascript

/**
 * @function getReadingsWithAddress
 * @description request Readings for device using device address
 * @param {Credentials} credentials
 * @param {string} address
 * @returns {Device}
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
      params: {
        customerID,
        // dateBegin,
        // dateEnd,
        limit: 10,
        convert: true, // if undefined raw data will be retrieved 
      }
    });
    return readingsList;
  } catch (e) {
    console.error(`\nERROR getReadingsWithAddress ${e}\n`);
  }
}
```

example of returned data for wind machine:
```JSON
[
  {
    "date":"2020-07-10T19:46:45.000Z",
    "temperature":26.6,
    "internalBattery":80,
    "rssiDb":69,
    "engineState":1,
    "engineStateIntention":0,
    "rpm":0,
    "batteryExternalV":13.7,
    "fuelLevel":"Not Connected",
    "startTemperature":0.3,
    "stopTemperature":4.4,
    "startMode":1
  }
]
```
*Note*: returned reading data that is invalid will be indicated by the value "ERR".
