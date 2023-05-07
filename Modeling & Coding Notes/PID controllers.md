#programming #ros2 

A [[ROS2]] node that reads Kalman filter estimations and navigation set points and publishes the controlled input of the system.

## 1. Discrete implementation

The overall control function in continuous time can be defined as:
$$u(t) = K_pe(t) + K_i\int_0^te(\tau)d\tau + K_d\frac{d\:e(t)}{dt}$$
Check [[Controller and Estimator Design]].

The discrete algorithm of our controller:
$$u(t_k) = u(t_{k-1}) + (K_p + K_i\Delta t + K_d/\Delta t)e(t_k) + (-K_p - 2K_d/\Delta t)e(t_{k-1}) + (K_d/\Delta t) e(t{k-2}) $$
- $K_p$ is the proportional gain.
- $K_i$ is the integral gain.
- $K_d$ is the derivative gain.
- $\Delta t$ is the interval time which we defined as the same as the publishing frequency (for both PID nodes 20Hz).
- $e(t_k)$ is the current error.
- $e(t_{k-1})$ and $e(t_{k-2})$ are the previous errors.
- $u(t_k)$ is the current state.
- $u(t_{k-1})$ is the previous state.

## 2. PID driver

We implemented the PID controller in a separate driver. Firstly, we initialized all of the parameters. 
```python
def __init__(self, Ki, Kp, Kd, delta, sat_val):

        self.curr_state = 0.

        self.setpoint = 0.

        self.e1 = 0.

        self.e2 = 0.

        self.e3 = 0.

        self.u_curr = 0.

        self.u_last = 0.

        self.Kp = Kp

        self.Ki = Ki

        self.Kd = Kd

        self.delta = delta

        self.sat_val = sat_val
```
In a separate function, we have the current error function which is the current set point substracted by the current reading:
$$e(t_k) = w_{setpoint} - w_{estimation}$$
Once this calculation is done, we can implement the discrete algorithm and return the corrected state. Finally, all of the parameters will be updated accordingly.

## 3. ROS nodes

Two PID nodes were created, one that publishes the velocity input $\tau$ and the steering angle $\theta$. See [[PiCar Modelling and Simulation]] for the detailed state-space model of the car.

### a. Velocity PID publisher

Important aspects include:
- Calling the class's constructor and give the node a name:
```python
# Super constructor
super().__init__('pid_node')
```
- Creating the subscription objects, which are the velocity estimation from the Kalman filter and the Velocity setpoint from the Navigation:
```python
# Subscriber
self.kfx_sub = self.create_subscription(XFiltered, 'x_filtered',
										self.xfiltered_callback, 10)

self.vel_set = self.create_subscription(VelSetpoint, 'vel_setpoint',
										self.velset_callback, 10)
```
- Creating a publisher object which will be received by the motor driver.
```python
# Publisher
self.pid_pub = self.create_publisher(PIDVEL, 'tau', 10)
```
- An `xfiltered_callback` which will receive the velocity estimation from the Kalman filter estimation publication.
- A `velset_callback` a which will receive the velocity setpoint from the navigator.
- A `timer_callback` which increments the message field and call the publisher method at a frequency of 20Hz (so every 0.05s) to publish the message. It will also call back on the driver functions to calculate the current state.
```python
def timer_callback(self):

        msg = self.populate_message()

        self.pid_pub.publish(msg)

        self.pid_driver.update()
```
- A `populate_message` function which will assign the messages accordingly.
```python
def populate_message(self):

        msg = PIDVEL()

        # Header

        msg.header.stamp = self.get_clock().now().to_msg()

        msg.header.frame_id = 'body'

        # Rest of the message

        msg.tau = float(self.pid_driver.pid_calc())

        return msg
```

### b. Steering angle PID publisher

Important aspects include:
- Calling the class's constructor and give the node a name:
```python
# Super constructor
super().__init__('pid_pose_node')
```
- Creating the subscription objects, which are the x and y positions estimation from the Kalman filter and the Theta setpoint from the Navigation:
```python
# Subscriber
self.kfx_sub = self.create_subscription(XFiltered, 'x_filtered',
										self.xfiltered_callback, 10)
self.posesetpoint_sub = self.create_subscription(PoseSetpoint, 'pose_setpoint', 
												 self.poseset_callback, 10)
```
- Creating the publisher object for the steering angle which will be received by the servo driver:
```python
self.pid_pose_pub = self.create_publisher(PIDPOSE, 'theta', 10)
```
- An `xfiltered_callback` and `poseset_callback` functions which gets the x and y estimations from the Kalman filter and the setpoint positions from the navigation. In order to estimate the current angle of the car. Described as:
$$ \theta = atan((\hat{y} - \bar{y})/(\hat{x} - \bar{x})) $$
Where $\hat{x}$ and $\hat{y}$ are the next set points the car must go to. $\bar{x}$ and $\bar{y}$ are the estimation from the Kalman filter.
- A `timer_callback` which increments the message field and call the publisher method at a frequency of 20Hz (so every 0.05s) to publish the message. It will also call back on the driver functions to calculate the current state.
- A `populate_message` function which will assign the correct information to each messages accordingly.

## References:
- [Matlab Vehicle Modelling](https://www.mathworks.com/help/ident/ug/modeling-a-vehicle-dynamics-system.html)
- [Coriolis Force](https://en.wikipedia.org/wiki/Coriolis_force)
