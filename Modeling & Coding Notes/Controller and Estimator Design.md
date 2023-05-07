 #design

Controller 

refer to [[Estimators]] and [[Feedback Loops]] for theoretical background

## I. Kalman Filter Design

There are three pieces to the "puzzle" of implementing a rudimentary Kalman filter. 
1. Discretized dynamics model describing the dynamics of the applied input and the states of interest
2. Process input, coming from the sensor
3. Noise distribution of the sensor, collected at 0 bias.

We are mainly interested in designing three kalman filters, measuring our three interested state variables, as outlined by [[PiCar Modelling and Simulation]]
$$\vec{x} = \begin{bmatrix}v_x \\ v_y \\ r_z\end{bmatrix}$$

### 1. Y-Axis Kalman Filter Design


## 2. Controller Design

- 2 Controllable values: engine pwm $\tau$ and steering angle $\theta$ 

### *Prelude: PID Controllers
- Continuous Time:
$$u(t) = K_pe(t) + K_i\int_0^te(\tau)d\tau + K_d\frac{d\:e(t)}{dt}$$

- Discrete Time (discretized with zero-order hold)
$$u_k = u_{k-1} + K_p[(1+\frac{\Delta t}{K_p}+\frac{K_d}{K_p\Delta t} )e_k + (-1-\frac{2K_d}{K_p\Delta t})e_{k-1}+\frac{K_d}{K_p\Delta t}e_{k-2}]$$
- In large, PID controllers carry out the following 6 steps (Astrom and Murray, P311)
	1. Wait for clock interrupt
	2. Read input from sensor
	3. Compute control signal
	4. Send output to actuator
	5. Update control variables
	6. Repeat

### I. Engine Speed Regulator

- error function
$$e_\tau(t) = V_{m_{ref}} - V_{m_{actual}}$$


### II. Steering Angle Regulator



### III. Anti-windup regulator

## Appendix: References

### IMU Filter Notes
- [Comparison of complementary and Kalman filters for IMU](https://aip.scitation.org/doi/pdf/10.1063/1.5018520#:~:text=Since%20Kalman%20filter%20is%20a,the%20statistically%20most%20optimal%20value.)
- [[Feedback_Systems_Textbook.pdf]]

