#theory #admin 

In order to begin the PI car project we began by reviewing relevant topics. 

The first of these topics was the [[Linux Command Line]], as some knowledge of navigating the command line is necessary for completing our first task in the project timeline of compiling ROS2 on the Raspberry PI.

The next topic reviewed was the broad topic of [[Feedback Systems]]. The first of the subtopics reviewed was [[Feedback Loops]], as having a good conceptual understanding of how feedback loops play into control is instrumental for beginning to model the PI car and handling the sensors. Then, as we began to model the system we reviewed the subtopic of [[System Dynamics and Modeling]], to refresh on dynamical systems before beginning on our state space model of the PI car.

After completing the barebones Matlab model, we began running system identification tests in order to find our Picar's system constants. However, in order to get some of these constants we needed to observe the car moving, so we detoured from modelling to [[Sensors & Electronics]] which will allow for the rest of the system identification components to be collected, and further for our model to be completed. In our detour, we will be learning the setup, specifications, features, inputs/outputs, and run and understand sample code for each of our sensors we will later assemble onto our Picar. We also explore the workings of the sensors and their [[Bus Communication Protocols]]. Beyond gathering the system identificaiton components, this study should also make programming our components with [[ROS2]] simplier later on. 

After running the priminary tests on each of our sensors to ensure correct wiring and functioning, we began to design filters to better sensor performance. We then began research on [[Estimators]] as we began work on our IMU and encoder . Our estimator research began with the base level [[Alpha-Beta-Gamma Filter]], and then quickly moved to the [[Kalman Filter]]. Then we began work on the motor and servo ([[Actuators]]) in conjunction with the IMU and encoder. In addition to testing our code for the IMU and encoder this process allowed us to understand the limits of the motor and servo, and have a better idea of what running the car around the track would look like. Finally, we began looking into [[Sensor Fusion]] with our GPS and the [[Extended Kalman Filter]]. Sensor fusion is utilized with the GPS to help control for some of its weaknesses especially on a small scale (like the track we are testing on). Our sensor fusion research lead us to the [[Robot_Localization Package]]
in ROS2, which while it ended up past our bandwidth, could potentially be a useful package for sensor fusion. In addition, we looked into [[SLAM]] as a reach point for this semester, and though it will remain a reach goal (replaced with sensor fusion) we did some preliminary research on the topic that may be useful later on. 

## Topics to Explore

- [ ] [[Sensors & Electronics]]
- [ ] [[Estimators]]
- [ ] [[Continuous Integration]]
- [ ] [[ROS2]]
- [ ] [[Bus Communication Protocols]]
- [ ] [[Numpy]]
- [ ] [[SLAM]]
- [ ] Land-mark based navigation: determine the sensing range, vSLAM (overkill)

## Spring Break Topics

- [ ] GPS
- [ ] PiTop Completion
- [ ] ROS 4-6