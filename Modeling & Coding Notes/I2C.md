#theory #sensor 

## I. Basics

- I<sup>2</sup>C is a serial communication bus protocol
- Supports multi-master, multi-slave
- Two wires:
	- `SDA`: Serial Data
	- `SCL` Serial Clock
	- Both wires are pulled up
- Data synchronized by clock
	- clock signal controlled by master
- Typical I<sup>2</sup>C message is set up as follows
![I2C Message](https://www.circuitbasics.com/wp-content/uploads/2016/01/Introduction-to-I2C-Message-Frame-and-Bit-2-1024x258.png)
- Address frame: slave address
- 

## Appendix: References
- [Wikipedia Page](https://en.wikipedia.org/wiki/I%C2%B2C)

### Setup
[[Sensor SetUp]]

### I2C on Ubuntu:

```shell
# detect i2c address matrix
sudo i2cdetect -y 1
```
