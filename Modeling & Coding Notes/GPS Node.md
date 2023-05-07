#programming #ros2  #sensor 

A [[ROS2]] node that publishes the GPS location of the PiCar.

## I. GPS driver

The GPS parsing module sends commands to, and parse simple NMEA data sentences from serial and I2C GPS modules to read latitude, longitude, and more. The driver and the system can be downloaded using:
```linux
pip3 install adafruit-circuitpython-gps
sudo pip3 install adafruit-circuitpython-gps
```

UART communication is used as the serial communication protocol. This is called in the initializing function.
```python
uart = serial.Serial("/dev/ttyS0", baudrate=9600, timeout=10)

gps = adafruit_gps.GPS(uart, debug=False)  # Use UART/pyserial
```

The PMTK314  command is called to get GGA and RMC info (time, position, date, course, speed and fix related data). In our case, we just want the latitude and longitude in degrees and minutes.
```python
self.gps.send_command(b"PMTK314,0,1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0")
```
The set up rate is once a second with the PMTK220:
```python
self.gps.send_command(b"PMTK220,100")
```

The gps readings are saved in an array organized as:
```python
self.index_dict = {

            "lat_deg": 0,
            "lat_min": 1,
            "long_deg":2,
            "long_min": 3
        }
```

## II. GPS publisher node

Important aspects include:
- - Calling the class's constructor and give the node a name:
```python
# Super constructor
super().__init__('gps_publisher')
```
- Creating a publisher object which will be received by the motor driver.
```python
# Publisher
self.publisher = self.create_publisher(GPS, 'gps_raw', 10)
```
- A `timer_callback` which increments the message field and call the publisher method at a frequency of 10Hz (so every 0.1s) to publish the message. The message is then published to the console with `get_logger().info` method.
- A `populate message` function which assigns data using the followgin custom `.msg` file:
```python
std_msgs/Header header
float64 latdeg # latitude in degrees
float64 longdeg # longitude in degrees
float64 latmin # latitude in minutes
float64 longmin # longitude in minutes
bool flag # boolean which indicates if there is a fix
```

## References
- [Adadfruit Ultimate GPS](https://learn.adafruit.com/adafruit-ultimate-gps/overview)
- [Raspberry Pi UART communication](https://www.electronicwings.com/raspberry-pi/raspberry-pi-uart-communication-using-python-and-c)