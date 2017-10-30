# homebridge-foscamcamera [![npm version](https://badge.fury.io/js/homebridge-foscamcamera.svg)](https://badge.fury.io/js/homebridge-foscamcamera)
Foscam Security Plugin (Security System + Motion Sensor) for [HomeBridge](https://github.com/nfarina/homebridge) (API 2.1)

Older verion using API 1.0: [homebridge-foscam](https://github.com/rooi/homebridge-foscam)<br>
Older verion using API 2.0: [homebridge-foscam2](https://github.com/luisiam/homebridge-foscam2) (deprecated)

**Due to protocol limitation, users will need to pair with the camera in a HomeKit app separately.<br>**
**Pairing PIN is the same as the HomeBridge pairing PIN.**

# Important Notice
Currently, streaming only works on iOS 10.0. iOS 10.1+ enforces SRTP which is not implemented in the current streaming library. In addition, Foscam C1 streaming will not even work on iOS 10.0 due to funky firmware. Other than streaming, all the other functionalities should work as expected.

# Prerequisites
1. Node.js **v6.6.0** or above
2. HomeBridge **v0.4.6** or above
3. Only H.264 cameras are supported.

# Installation
1. Install homebridge using `npm install -g homebridge`.
2. Install this plugin using `npm install -g homebridge-foscam-security`.
3. Update your configuration file. See configuration sample below.

# Configuration
Edit your `config.json` accordingly. Configuration sample:
```
"platforms": [{
    "platform": "FoscamSecurity",
    "name": "Foscam",
    "cameras": [{
        "username": "admin",
        "password": "password",
        "host": "192.168.1.10",
        "port": 88,
        "stay": 13,
        "away": 15,
        "night": 14
    }, {
        "username": "admin2",
        "password": "password2",
        "host": "192.168.1.20",
        "port": 98,
        "stay": 0,
        "away": 14,
        "night": 13,
        "sensitivity": 2,
        "triggerInterval": 5
    }]
}]

```

| Fields               | Description                                                   | Default       | Required |
|----------------------|---------------------------------------------------------------|---------------|----------|
| platform             | Must always be `FoscamCamera`.                                |               | Yes      |
| name                 | For logging purposes.                                         |               | No       |
| cameras              | Array of camera config (multiple cameras supported).          |               | Yes      |
| \|- username         | Your camera login username.                                   | admin         | No       |
| \|- password         | Your camera login password.                                   |               | Yes      |
| \|- host             | Your camera IP address.                                       |               | Yes      |
| \|- port             | Your camera port.                                             | 88            | No       |
| \|- stay\*           | Configuration for Stay Arm.                                   | 0             | No       |
| \|- away\*           | Configuration for Away Arm.                                   | 0             | No       |
| \|- night\*          | Configuration for Night Arm.                                  | 0             | No       |
| \|- sensitivity      | Motion sensor sensitivity from 0 (lowest) to 4 (high).        | Camera Config | No       |
| \|- triggerInterval  | Time in `s` (5-15) of which motion sensor can be retriggered. | Camera Config | No       |

\*`stay`, `away`, `night` define configuration for different ARMED state.

The supported configurations depend on your device. The Foscam public CGI defines the following:<br>
bit 3 | bit 2 | bit 1 | bit 0<br>
bit 0 = Ring<br>
bit 1 = Send email<br>
bit 2 = Snap picture<br>
bit 3 = Record

The following seems to be valid for the C2 as well (not found in any documentation)<br>
bit 7 | bit 6 | bit 5 | bit 4 | bit 3 | bit 2 | bit 1 | bit 0<br>
bit 0 = Ring<br>
bit 1 = Send email<br>
bit 2 = Snap picture<br>
bit 3 = Record<br>
bit 7 = Push notification

Note: The configuration is defined as int, thus the followings are valid, e.g. 0 (Do Nothing), 1 (Ring), 2 (Email), 3 (Ring + Email), 4 (Picture), 12 (Picture and Record), 13 (Ring, Picture and Record), etc.

P.S.: Any ARMED state will activate motion detection by default.
