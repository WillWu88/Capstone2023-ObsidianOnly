#sensor

## See [[Sensor SetUp]] for sensor setup instructions

- [Old Groups Github](https://github.com/esecapstone/picar)

## I. Lidar

![Lidar Test Code Results](Figures/Lidar.png)
### Links about Lidar Specs/Pinout:

- [Lidar Spec Sheet](https://www.digikey.dk/htmldatasheets/production/3265529/0/0/1/a1m8.html)
- [Lidar pinout](https://bucket-download.slamtec.com/e680b4e2d99c4349c019553820904f28c7e6ec32/LM108_SLAMTEC_rplidarkit_usermaunal_A1M8_v1.0_en.pdf)

### Basic Notes about Lidar:

- Laser triangularation measurement system
- System can perform 360 degree within 6 meter range
- Scanning frequency reached 5.5hz when sampling 360 points each round. Can be configured to 10 hz max.
- Rotates and scans clockwise
- Comes with a speed detection and adaptive system, and the system can then adjust frequency.
- System measures distance data in more than 2000 times' per second.

### Measurements:
- Distance Range
- Angular Range
- Distance Resolution
- Angular Resolution
- Sample Duration
- Sample frequency
- Scan Rate
#### Optical:
- Laser wavelength
- Laser power
- Pulse length
#### Measures:
- Ranges
- Line following
- Grass Avoiding

## II. Encoder 

![Filtered and Unfiltered RPM Data](Figures/rpm_sensor.jpg)
### Basic Notes on Encoder

- Measure angular or linear distance
- Can be used to measure velocity
- RPM sensor
- refer to data sheet: "use gpio #16, pin5 on connector j6"

### Implementation
- See [[RPM Publisher]] for ROS2 implementation

## III. IMU / accelerometer - gyroscope

### Basic Notes on IMU:

- Acceleration in x,y,z
- Angular speed around x, y, z
- Model: [MPU6050](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf)
- Acceleration in x,y,z

### Pinout:
| Pin | Full Name | Function |
| - | - | - |
| *SCL* | Serial Clock | |
| *SDA* | Serial Data | |
| *INT* | Interrupt Digital Output | |

### Bus Communication Protocol:

- Sensor uses [[I2C]] for communication to PI
- [Example Raspberry Pi Code](https://www.electronicwings.com/raspberry-pi/mpu6050-accelerometergyroscope-interfacing-with-raspberry-pi)

```shell
# smbus has a permission problem, so use the following command (carefully) to change permission of the file
sudo chmod 777 /dev/i2c-1
```

example reading out from IMU below:
![IMU Reading][Figures/IMU-Reading.png]![IMU Reading][Figures/IMU-Reading-acc.png]
![IMU Reading][Figures/IMU-Reading-gyro.png]
### ROS2 Publisher Implementation

- See [[IMU Publisher]]


## IV. Level Shifter

## V. Servo Control Board
- [Data Sheet](https://cdn-shop.adafruit.com/datasheets/PCA9685.pdf)


## VI. XL-5 Traxxas ESC

### ESC Spec sheet and key notes:

- **Only run startup once: otherwise it fails, but once startup successfully runs, it is programmable**

- [Traxxas notes](Documents/Electronic_Speed_Control.pdf)
- Electronic Speed Control
- Forward, Reverse, and Breaking

Forward Specs:
- Resistance 0.005 ohms
- Peak Current 100A

Reverse Specs:
- Resistance 0.014 ohms
- Peak Current 60A

Other Specs: 
- **PWM freq: 1700 Hz**
* Braking Current: 60A
- Continuous Current: 15A

**The ESC uses 5V j connectors, not the servo board**


## VII. Camera
[Raspberry Pi camera spec sheet](https://www.raspberrypi.com/documentation/accessories/camera.html)

## VIII. GPS

- Obtains location and velocity reading
- Heading information? 

## Appendix: References
- [Raspberry PI GPIO Setup](https://ubuntu.com/tutorials/gpio-on-raspberry-pi#1-overview)
- 