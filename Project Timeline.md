#design #admin 

## I. Overall timeline

- Our overall project contains three stages. Though the details might change, the overall theme of each stage will remain largely the same throughout the semester, guided by control system design principles.
- The <ins>first</ins> stage begins with [[PiCar Modelling and Simulation]], where we model the PiCar mathematically and simulate the system Numerically. 
	- We will also conduct [[System Identification]] experiments, where we identify actuator dynamics
- The <ins>second</ins> stage targets [[Controller and Estimator Design]], where we design controller and estimator systems for the PiCar. We want our system to have a robust backbone
- The <ins>third</ins> stage builds on the two preceeding pages and builds high-level navigation algorithms. Our system is loaded with Lidar, an advance sensor that enables advanced navigation algorithms. Refer to [[Navigation Design]]

## II. Meeting Logs

- 1/15/23: Initial Meeting

- 1/17/23: Start process for compiling ROS2 onto the Raspberry PI

- 1/19/23: Compiled ROS2 onto the Raspberry PI and established remote connection

- 1/20/23: Start modeling the PI car with space models

* 1/22/23: Modelling PiCar, sensor research

* 1/25/2023: Modelling PiCar, starting System Identification

- 1/26/2023: PiCar System ID

- 1/30/2023: Accelemeter python code and register sheet

- 2/2/2023: Lidar Setup and sample code

- 2/3/2023: IMU data collection into Matlab, encoder setup and sample code, and servo setup and sample code.

- 2/5/2023: Finalized projected timeline and got the motor to run.

- 2/8/2023: Restored encoder and set up magnet.

- 2/9/2023: Created python program to measure RPMs

- 2/10/2023: SSH into Raspberry Pi to program the motor and programmed car to run to measure the slip of the tires.

- 2/15/2023: Worked on formal proposal, found tire stiffness

- 2/16/2023: Exported RPM data to matlab and checked that our filtered RPM data removes the noise that is inherent to the encoder.

- 2/17/2023: Worked on Formal Proposal

- 2/19/2023: Finished Formal Proposal

- 2/20/2023: Began work on IMU ROS node and PiTop redesign

- 2/21/2023: Continued work on above

- 2/23/2023: Finished IMU ROS node, and PiTop first draft redesign, Began Encoder ROS node

- 2/24/2023: Started 3D print of PiTop in maker space, did work on creating the skeleton of our Kalman Filter

- 2/26/2023: First attempt at new PiTop implementation, made changes and reprinted new model and new front of car mountings that were also remodeled. Statistical analysis of the noise in the sensors via Matlab.

- 2/27/2023: Spent time working on PID equations for our model and began implementing the PID in Matlab integrated with our previous model.

- 3/1/2023: (hopefully) final changes on PiTop model; adding in screw drops, thinning the screw holes, adding a drop spot for Lidar motor, and a slit for the Lidar cord. Began making servo code more user friendly.

- 3/2/2023: Finished the PiTop model, worked on the encoder publisher and server, and worked on controller model.

- 3/3/2023: Met with Professor Kantaros about navagation system approach, and walked the trail for the picar.

- 3/4/2023: Walked through PID error functions as would apply to our code, and attempted to model it in Matlab.

- *SD card snap*

- 3/9/2023: Finished final PiTop design. IMU ROS node finished as well.

- 3/11/2023: Finished making servo code more user friendly. Decided to move to a GPS/Lidar/IMU sensor fusion approach.

- 3/16/2023-3/24/2023: Spring break; Ordered GPS, began sensor fusion research and extended kalman filter research. 

- 3/25/2023: Found the servo angles and switched from encoder node approach to a I2C peripheral approach. Printed a PiTop with a spot to mount the GPS.

- 3/27/2023: Remodeled Picar mounts and began robot_localization_node notes. Started building the I2C peripheral for RPM sensor.

- 3/28/2023: Assembled the Picar to completion. I2C peripheral implementation in progress

- 3/30/2023: Obsidian Notes page restructure and notes of what notes need to be filled out/updated were added. 

- 4/1/2023: *Meeting I missed, need help filling this out*

- 4/6/2023: Working encoder with Raspberry Pi pico node

- 4/7/2023: Able to connect to the GPS with a hotspot and run GPS test code.

- 4/7/2023 - 4/9/2023: Worked on Kalman filter for IMU and GPS script.

- 4/11/2023: First outdoor PiCar test, gathered IMU and Encoder data.