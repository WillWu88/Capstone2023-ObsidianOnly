#design #theory 

## I. Background

Refer to [[System Dynamics and Modeling]] for modelling technique

Inputs of the system:
- Steering angle (delta)
- Longitudinal slip from the front wheels
- Torque from the motor acting on back wheels

## II. Statespace Model of PiCar

### 1. Overview:

To build a model of our car, we must first examine the vehicle dynamics of the PiCar.

We build our model based on two fundamental principles of physics: Newton's Second Law, and Conservation of Net Torque.

$$F = ma,\; \tau = I\alpha$$

On the $x$ axis, the summation of forces can be written as below:

$$F_x = m\cdot a_x = F_{cor} + F_{front} + F_{rear} + F_D$$

Where $F_{cor}$ is the [Coriolis Force](https://en.wikipedia.org/wiki/Coriolis_force) due to rotation, $F_{front}$ and $F_{rear}$ are rolling/slipping forces the wheels exert on the car, and $F_D$ denotes the aerodynamic drag experienced by the car. Because the car travels below $80km/h$, a well-recognized aerodynamic-effect speed threshold, we neglect $F_D$ in our modelling for now. Refer to section [[#IV. Aerodynamics Modelling]] for further resolution to our model.

On the $y$ axis, similar forces come into play:
$$F_y = m\cdot a_y = F_{cor} + F_{front} + F_{rear}$$
We can model the network experienced by the car around the $z$ axis as follows:
$$\tau_z = J\cdot \alpha_{z} = \tau_{front} + \tau_{rear}$$

$\tau_{front}$ and $\tau_{rear}$ respectively denotes the net torque casted by the latitudinal friction of the wheels.

### 2. Forces and Moments

The tire forces can be modeled through the linear appoximation relating the longitudinal and lateral tire stiffness. These parameters for the four tires can be calculated with the longitudinal slip of the tire ($s_i(t)$) and the slip angle ($\alpha(t)$). 

The longitudinal tire forces can be calculated as the linear approximation of the multiplication of the longitudinal tire stiffness $C_x$ and the longitudinal slip of the tires $s(t)$. Noted as:
$$F_{x, FL} = C_x*s_{FL}(t)$$
$$F_{x, FR} = C_x*s_{FR}(t)$$
$$F_{x, RR} = C_x*s_{RR}(t)$$
$$F_{x, RL} = C_x*s_{RL}(t)$$
The latitudinal tire forces can be noted as the linear approximation of the lateral tire stiffness $C_y$ multiplied by the tire slip angle ($\alpha(t)$):
$$
F_{y, FL} = C_y*\alpha_{FL}(t)
$$
$$
F_{y, FR} = C_y*\alpha_{FR}(t)
$$
$$
F_{y, RL} = C_y*\alpha_{RL}(t)
$$
$$
F_{y, RR} = C_y*\alpha_{RR}(t)
$$
The slip angles can be determined by: 

Three differential equations can be derived from the use of Newton's law of motion, some geometric relationships and the tire forces determined above. They describe the longitudinal velocity($v_y(t)$), the lateral velocity($v_y(t)$) and the yaw rate measured around the Center Of Gravity.

$$
\frac{d\mathbf{v_x(t)}}{dt} = v_y(t)*r(t) + 1/m*((F_{x,FL}(t)+F_{x,FR}(t))cos(\delta(t)) - (F_{y,FL}(t)+F_{y,FR}(t))sin(\delta(t) + F_{x,RL}(t)+F_{x,RR}(t) - C_A*v_x(t)^2)
$$
$$
\frac{d\mathbf{v_y(t)}}{dt} = -v_x(t)*r(t) + 1/m*((F_{x,FL}(t)+F_{x,FR}(t))sin(\delta(t)) - (F_{y,FL}(t)+F_{y,FR}(t))cos(\delta(t) + F_{x,RL}(t)+F_{x,RR}(t))
$$
$$
\frac{d\mathbf{r(t)}}{dt} = 1/J *(a*(F_{x,FL}(t)+F_{x,FR}(t))sin(\delta(t)) - (F_{y,FL}(t)+F_{y,FR}(t))cos(\delta(t) -b*( F_{y,RL}(t)+F_{y,RR}(t)))
$$



### 3. State-Space Model
We define the state vector of the car as follows:

$$\vec{x} = \begin{bmatrix}\dot{x} \\ \dot{y} \\ \dot{\psi}\end{bmatrix} = \begin{bmatrix}v_x \\ v_y \\ r_z\end{bmatrix}$$
where $x$ and $y$ are the displacement of the car from an arbitrary starting point. We might extend our model to include these displacement states if we can aquire localization information, i.e global/local position through GPS or SLAM. $v_x$ and $v_y$ denote the translational speed of the car along their respective axis,  while $\psi$ and $r_z$ denotes angular quantities (right-hand rotation around a downward z-axis), yawing angle and yawing velocity to be exact. 

Our dynamic system contains two inputs: steering angle $\theta$ and motor torque $\tau_m$. 
$$\vec{u} = \begin{bmatrix}\theta \\ \tau_m\end{bmatrix}$$
Though we can control $\theta$ with direct command from the Raspberry Pi, we indirectly control $\tau_m$ by varying the voltage. See [[System Identification]] and [[#III. Actuator Modelling]] for details.


## III. Actuator Modelling

- Check with IMU alignment

## IV. Aerodynamics Modelling

## V. Power Modelling

We notice a significant degradation in motor speed due to power consumption. The battery drops in voltage, resulting in a decrease in motor rpm. We assume the following linear relationship for battery/motor degradation.

$$\frac{V_{Bat}}{V_{Full}} = m \cdot \frac{R_{actual}}{R_{commanded}}$$
To determine the empirical value of $m$, we command the motor at a constant speed while monitoring battery voltage and wheel speed. See [[System Identification]] for experiment and result.


## Appendix: References
- [Matlab Vehicle Modelling](https://www.mathworks.com/help/ident/ug/modeling-a-vehicle-dynamics-system.html)
- [Coriolis Force](https://en.wikipedia.org/wiki/Coriolis_force)
