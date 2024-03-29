---
title: Session
sidebar: example
---
# Session Examples

Below are examples on how to use the library with a Hive session.

# Log in - Using Hive Username and Password with MFA(if required)

Below is an example of how to log in to Hive using your Hive Username and Hive password, using 2FA if needed, to create a pyhiveapi `session` object.
NOTE - as part of Hive login it now registers your device as a trusted device on sucessfull login. The device data will be needed for further logins in the future, this libary has a device login method to support this.

```Python
from pyhiveapi import Hive, SMS_REQUIRED

session = Hive(username="HiveUserName", password="HivePassword")
login = session.login()

if login.get("ChallengeName") == SMS_REQUIRED:
    code = input("Enter 2FA code: ")
    session.sms2fa(code, login)

# Device data is need for future device logins
deviceData = session.auth.getDeviceData()
print(deviceData)

session.startSession()
```

# Log in - Using Hive Device Authentication

Below is an example of how to log in to Hive using device authentication, to create a pyhiveapi `session` object.


```Python
from pyhiveapi import Hive, SMS_REQUIRED

session = Hive(
    username="<Hive Username>",
    password="<Hive Password>",
    deviceGroupKey="<Hive Device Group Key>",
    deviceKey="<Hive Device Key>",
    devicePassword="<Hive Device Password>",
)
session.deviceLogin()
session.startSession()
```


## Use the session object to get devices

Below is an example of how to use the `session` object to get all devices of each type from `deviceList` and store in a separate list for each device type.

```Python
BinarySensors = session.deviceList["binary_sensor"]
HeatingDevices = session.deviceList["climate"]
Lights = session.deviceList["light"]
Sensors = session.deviceList["sensor"]
Switches = session.deviceList["switch"]
WaterHeaters = session.deviceList["water_heater"]
```

### Use the session object to interact with heating

Below is an example of how to use the `session` object to interact with all the different heating actions.

```Python
if len(HeatingDevices) >= 1:
    HeatingZone_1 = HeatingDevices[0]
    print("HeatingZone 1 : " + str(HeatingZone_1["hiveName"]))
    print("Get operation modes : " + str(session.heating.getOperationModes()))
    print("Current mode : " + str(session.heating.getMode(HeatingZone_1)))
    print("Current state : " + str(session.heating.getState(HeatingZone_1)))
    print("Current temperature : " + str(session.heating.currentTemperature(HeatingZone_1)))
    print("Target temperature : " + str(session.heating.targetTemperature(HeatingZone_1)))
    print("Get Min / Max temperatures : " + str(session.heating.minmaxTemperature(HeatingZone_1)))
    print("Get whether boost is currently On/Off : " + str(session.heating.getBoost(HeatingZone_1)))
    print("Boost time remaining : " + str(session.heating.getBoostTime(HeatingZone_1)))
    print("Get schedule now/next/later : " + str(session.heating.getScheduleNowNextLater(HeatingZone_1)))
    print("Set mode to SCHEDULE: " + str(session.heating.setMode(HeatingZone_1, "SCHEDULE")))
    print("Current operation : " + str(session.heating.currentOperation(HeatingZone_1)))
    print("Set target temperature : " + str(session.heating.setTargetTemperature(HeatingZone_1, 15)))
    print("Turn boost on for 30 minutes at 15c: " + str(session.heating.turnBoostOn(HeatingZone_1, 30, 15)))
    print("Turn boost off : " + str(session.heating.turnBoostOff(HeatingZone_1)))
```

### Use the session object to interact with hotwater

Below is an example of how to use the `session` object to interact with all the different hotwater actions.

```Python
if len(WaterHeaters) >= 1:
    WaterHeater_1 = WaterHeaters[0]
    print("WaterHeater 1 : " + str(WaterHeater_1["hiveName"]))
    print("Get operation modes : " + str(session.hotwater.getOperationModes()))
    print("Current mode : " + str(session.hotwater.getMode(WaterHeater_1)))
    print("Get state : " + str(session.hotwater.getState(WaterHeater_1)))
    print("Get whether boost is currently On/Off: " + str(session.hotwater.getBoost(WaterHeater_1)))
    print("Get boost time remaining : " + str(session.hotwater.getBoostTime(WaterHeater_1)))
    print("Get schedule now/next/later : " + str(session.hotwater.getScheduleNowNextLater(WaterHeater_1)))
    print("Set mode to OFF : " + str(session.hotwater.setMode(WaterHeater_1, "OFF")))
    print("Turn boost on for 30 minutes : " + str(session.hotwater.turnBoostOn(WaterHeater_1, 30)))
    print("Turn boost off : " + str(session.hotwater.turnBoostOff(WaterHeater_1)))
```

### Use the session object to interact with lights

Below is an example of how to use the `session` object to interact with all the different light actions.

```Python
if len(Lights) >= 1:
    Light_1 = Lights[0]
    print("Light 1 : " + str(Light_1["hiveName"]))
    print("Get state : " + str(session.light.getState(Light_1)))
    print("Get brightness : " + str(session.light.getBrightness(Light_1)))
    print("Get min colour temperature : " + str(session.light.getMinColorTemp(Light_1)))
    print("Get max colour temperature : " + str(session.light.getMaxColorTemp(Light_1)))
    print("Get colour temperature : " + str(session.light.getColorTemp(Light_1)))
    print("Get colour : " + str(session.light.getColor(Light_1)))
    print("Get colour mode : " + str(session.light.getColorMode(Light_1)))
```
