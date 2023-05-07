#programming #ros2 #acc #sensor 

A [[ROS2]] node that publishes both raw and filtered RPM sensor readings

## I. Driver Design

The challenge of designing the encoder node is dealing with the synchronicity of the raw readings and the publishing frequency. Our encoder driver reads the time passed between each magnet triggers, and from this it calculates the speed of the car. Therefore, the RPM calculation is always irregular and depends on how fast the car is going.  However, the publishing frequency of the node itself is regulated at 200Hz. To solve this problem we looked at two different options: creating an action node, or using a micro-controller. 

### 1. Action Server

The action server follows an asynchronous communication model. The service sends a request, the server processes it and sends responses. In our case, the encoder node would request the rpm reading, our driver processes it and sends it back.

The action server offers two types of response: feedback and result. We can design the feedback to be instantaneous while the server processes responses. We thought to have the feeback be the raw RPM reading, and the reponse is the moving average RPM. Because the RPM object uses a circular buffer to filter results, we can return the moving average and the latest result stored in the buffer instanteously as the "feedback". When the server finishes, it returns the updated data. 

Though this model works in theory, it also significant drawbacks. Unlike the IMU, this data is not instanteous. Though the feedback is instantaneous, it is an older version of the reading. The server does not start sampling unless the client tells it to. If we integrate the client into a mision-critical node such as the controller, the node will suffer from significant lag and inaccruate reading. This option did not resolve our issue. We moved on to using a micro-controller.

### 2. Alternative: micro-controller co-pilot

- Setting up a [Raspberry Pi Pico W](https://www.raspberrypi.com/products/raspberry-pi-pico/) as a small [[I2C]] peripheral
- Using the C sdk for Pi Pico, we first migrated the polling calculation onto Pico
	- Circular buffer now keep tracks of time in microseconds instead of recording speed
- The pico is then configured as an I<sup>2</sup>C peripheral with buffer memory. The driver code on Raspberry PI will then read the buffer memory through I<sup>2</sup>C to get the latest reading.

See [[RPM I2C Peripheral]] for detailed implementation.

## II. Publisher node design

###  1. Creating custom srv files

Topics act as a bus for nodes to exchange messages. The structure of each topic is defined by a .srv or .msg file. Unlike the IMU publisher, the encoder does not have a predefined interface definition from existing ROS2 documentation. So, we created a custom .msg file in its own package. 

RPM.msg structure:
```python
std_msgs/Header header
float64 rpmraw
float64 rpmfiltered
```

### 2. Publisher

Important aspects include:
- Importing the built-in message type, which structures the data that passes on the topic.
```python
from tutorial_interfaces.msg import Rpmmsg
```
- Calling the class's constructor and give the node a name:
```python
super().__init__('rpm_publisher')
```
- Creating a publisher object which is in charge of creating the topic and publishing the messages. We used the public method of `rclcpp:Node`: `create_publisher` which returns the `rclcpp:: Publisher` object. 
``` python
self.publisher = self.create_publisher(RPM, 'rpm_raw', 10) # history depth of 10
```
- Creating a `timer_callback` which increments the message field and call the publisher method at a frequency of 200Hz (so every 0.005s) to publish the message. The message is then published to the console with `get_logger().info` method.
```python
def timer_callback(self):

        self.encoder.update()

        msg = self.populate_message()

        self.publisher.publish(msg)

        if (not(self.msg_count)):

            self.get_logger().info('Publishing: "%f"' % msg.rpmfiltered)

            self.msg_count += 1
```
- Creating a `populate_message` function which assigns data to the `header`, `rpmraw` and `rpmfiltered` data types.
```python
def populate_message(self):

        msg = RPM()

        # Header

        msg.header.stamp = self.get_clock().now().to_msg()

        msg.header.frame_id = '' #empty for now

		# RPM readings messages

        msg.rpmraw = 0.0

        filtered_rpm = self.encoder.update()

        msg.rpmfiltered = filtered_rpm

        msg.derivedms = 3.1415*filtered_rpm*wheel_diam/60.0
```
- Defining the main function which initializes the rclpy library and "spins" the node using the `rclpy.spin` method to call the callbacks.
```python
def main(args=None):

    rclpy.init(args=args)
    
    rpm_publisher = RpmPublisher()

    rclpy.spin(rpm_publisher)  

    rclpy.shutdown()
```

## Appendix: References

- [rclpy reference](https://docs.ros2.org/foxy/api/rclpy/index.html)
- [[Python Notes]]
- [Bool Message Type](https://github.com/ros2/common_interfaces/blob/rolling/std_msgs/msg/Bool.msg)
- [Writing an action server in python](https://docs.ros.org/en/foxy/Tutorials/Intermediate/Writing-an-Action-Server-Client/Py.html)