#programming #ros2 #acc #sensor 

A [[ROS2]] node that publishes raw IMU readings

## I. Node Design

Important aspects include:
- Using datatype: `IMU` from `sensor_msgs`
	- Refer to "geometry_msgs" [[#Appendix: References]] 
- Calling the class's constructor and give the node a name:
```python
super().__init__('imu_publisher')
```
- Creating a publisher object which is in charge of creating the topic and publishing the messages. We used the public method of `rclcpp:Node`: `create_publisher` which returns the `rclcpp:: Publisher` object. 
- Creating a `timer_callback` which increments the message field and call the publisher method at a frequency of 100Hz (so every 0.01s) to publish the message. The message is then published to the console with `get_logger().info` method.
- Creating a `populate_message` function which assigns the sensor readings to the datatype.
- Defining the main function which initializes the rclpy library and "spins" the node using the `rclpy.spin` method to call the callbacks.

## Appendix: References

- [ROS Index: Geometry_msgs](https://index.ros.org/p/geometry_msgs/github-ros2-common_interfaces/#humble)
- [rclpy reference](https://docs.ros2.org/foxy/api/rclpy/index.html)
- [[Python Notes]]
- [ROS 2 Documentation: How to write a publisher node] (https://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)
