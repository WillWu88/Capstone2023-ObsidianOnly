#actuator #design #control 

## Purpose for System ID

In order to model the PiCar we need different constants associated with the system.  The need for these different values led us to conducting several system identification tests. 

## Images of Tests

![Weighing Experiment](Figures/Weighing_picar.jpg)
![Experiment to determine these physical parameters](Figures/Center_of_gravity.jpg)

## Values we needed for the model

Constants We Need:
[Sample with constants](https://www.politesi.polimi.it/bitstream/10589/137501/1/Tesi%20Ravizzoli%20Carlo.pdf)
| Property | unit | Quantity | Note |
| ---- | - | - | - |
| Chassis Mass | $g$ | 1545.1 | |
| Battery Mass | $g$ | 395.5 | |
| Sensor housing Mass | $g$ | 187.3 | |
| Lidar Mass | $g$ | 210.5 | |
| Raspberry Pi | $g$ | 82.9 | |
| Pi Hat | $g$ | 37.8 | |
| PiTop | $g$ | 118.07 | *May no longer be accurate after remodel* |
| Center of gravity | - | | center of gravity will be marked on the car |
| a | $cm$ | 18.3 | |
| b | $cm$ | 10.5 | |
| J | $kg*m^2$ | 0.0359 | |
| C_x | $m$ | ~0 | *were not able to definitively determine this value* |
| C_y | $m$ | ~0 | *were not able to definitely determine this value* |
| Front wheel slip | - | 0.02 | |
| Back wheel slip | - | 0.05 | |

### Guide 
- a: Distance from center of gravity to front axle wheels
- b: Distance from center of gravity to back axle wheels
- J: Moment of Inertia
	- Approximated as a rectngular slab
	- Equation used: $(1/12)(massChassis)(length^{2}+width^{2})$
- C_x: Longitudenal tire stiffness [Tire Stiffness Forum](https://engineering.stackexchange.com/questions/19253/questions-regarding-cornering-stiffness-for-rc-car)
- C_y: Lateral tire stiffness
- slip [measuring slip](http://salesmanual.deere.com/sales/salesmanual/en_NA/tractors/2012/feature/ballasting_and_optimizing_performance/7/7r_ballast_wheel_slip.html) [more about slip](https://en.wikipedia.org/wiki/Slip_(vehicle_dynamics)) 

## Other Values we found
| Property | unit | Quantity | Note |
| ---- | - | - | - |
| Car length | $cm$ | 42 | |
| Car width | $cm$ | 32.5 | |
| Car width without wheels | $cm$ | 22 | |
| GR | - | 0.1553 | |

### Guide
- GR: Gear ratio = # gears on small gear / # gears on large gear
	- Number of teeth on small gear: 16
	- Number of teeth on big gear: ~103


