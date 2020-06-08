# Homebridge-Envisalink-Ademco

  

This is a Homebridge/HOOBS plug-in leveraging a modified version of a node-red implementation ( https://www.npmjs.com/package/node-red-contrib-envisalink-ademco ) and a Homebridge envisalink DSC module ( https://www.npmjs.com/package/homebridge-envisalink )

This module was designed to work with Ademco Envisalink module with the Vista series alarm boards. 

Limits:

* Ademco panels provide limited zone information to their peripherals. The panel only provide real-time information when a zone is faulted (opened) but not when it is restored (closed). However, the virtual key panel is continuously updated with zones information. This module auto set the faulted zone (opened) to restored (close) after 30 second once the virtual panel no longer reports a fault for the specific zone.

  

* When system is "Armed" the panel no longer report the state of each zone. All zone will age out and be considered close once armed. Note: A bypass zone will automaticly show as fault (open) once the alarm is disarmed.

  

Please note: As of version 1.0.8 the attribute for zones "type" attribute has been changed to "sensorType" this is to enabled the UX Hoobs configuration manager.

  

## Configuration options

  
| Attributes     | Description                                                         |
| -------------- | ------------------------------------------------------------------- |
| host           | Envisalink server host IP Address                                   |
| port           | Envisalink server Port address. Default is 4025.                    |
| deviceType     | Device Model                                                        |
| password       | Envisalink server password. Default is "user".                      |
| pin            | Your local alarm PIN                                                |
| panelzones     | (optional) Max number of zone supported by panel. Default is 64.    |
| panelpartition | (optional) Max number of paratition defined by panel. Default is 1. |
| **partitions** | partition to monitor in homekit                                     |
| **zones**      | zones to appear and monitor in homekit                              |


**partitions**

> - name : partition name (default "house")

**zones**

> - name: zone name
> - sensorType :  door | leak | motion | smoke | window
> - partition : sensor partition number
> - zoneNumber : panel zone number for sensor


  
Example configuration is below.

  
  

```javascript

"platforms": [

{

    "platform": "Envisalink-Ademco",
    "host": "192.168.YYY.XXX",
    "deviceType": "Honeywell Vista",
    "password": "---envisalink password (default is user)---",
    "pin": "---panel pin for disarming---",
    "partitions": [
    {
        "name": "House"
    }

    ],

    "zones": [
        {
        "name": "Front Door",
        "sensorType": "door",   
        "partition": 1
        },
        {
        "name": "Master Bedroom Window",
        "sensorType": "window",
        "partition": 1
        }
    ]
}

```

  

## Non-Consecutive Zones (Recommended)

If your system has unused zones, simply include a *zoneNumber* integer property on ***each*** zone you have in the config. Make sure you put the property on each zone. This is the recommended configuration if you don't wish to monitor (display) all the zones within HomeKit or don't know the ordering of your system zone.

  

Examaple:

```javascript

...

"zones": [
    {

    "name": "Front Entry",
    "sensorType": "door",
    "partition": 1,
    "zoneNumber": 9
    },
    {
    "name": "Patio Door",
    "sensorType": "door",
    "partition": 1,
    "zoneNumber": 12
    },
    {
    "name": "Garage Door",
    "sensorType": "door",
    "partition": 1,
    "zoneNumber": 5
    }
]

...
