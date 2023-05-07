#sensor #acc

### Overall Notes 

- Utilize the \_H, highbit, features for accelerometer and gyro values. This allows for easy access to lowbit values by simply interating the address value. 

- Device Address, utilize 0x68. 

- Entire system runs on the sample rate, which we can set, with a default accelerometer sample rate of 1kHZ and gyroscope sample rate of 8kHz (which can be changed to 1kHz).

## Registers written to in sample code and their functionality

#### SMPLRT_DIV:

- *Sample code was set to 7.* 
- This write sets the sample rate with the following equation: 
*Sample Rate = Gyroscope Output Rate / (1 + SMPLRT_DIV)*
- The Gyroscopic Output Rate is 8kHz when DLPF is disabled (as we intend on using it) and 1kHz when enabled.

#### PWR_MGMT_1:

- *Set to 1 in the sample code.*
- The lower 3 bits are used for CLKSEL, this chooses a clock reference for the system. The spec sheet strongly recommends picking one of the gyroscopes as the reference, CLKSEL 1, 2 & 3.
*google PLL*
- Bit 3 is for TEMP_DIS which is not necessary for our purposes
- Bit5 is for CYCLE, and when this bit is set and SLEEP is disabled, the device cycles between sleep mode and waking up to take a single sample as a rate determined by LP_WAKE_CTRL
- Bit6 is for SLEEP, and when it is set the device enters sleep mode.
- Bit7 is for DEVICE_RESET, and when this bit is set all internal registers return to their default values.

#### Config:
 
- *Set to 0 in the sample code.* 
- The bottom 3 bits are used to set the DLPF_CFG which corresponds to the accelerometer and gyroscope bandwidth and delay (and frequency for gyroscope).
| DLPF_CFG | Acc B-width (Hz) | Acc Delay (ms) | Gyro B-width (Hz) | Gyro Delay (ms) | Gyro Fs (kHz) |
| ---- | - | - | - | - | - |
| 0 | 260 | 0 | 256 | 0.98 | 8 |
| 1 | 184 | 2.0 | 188 | 1.9 | 1 |
| 2 | 94 | 3.0 | 98 | 2.8 | 1 |
| 3 | 44 | 4.9 | 42 | 4.8 | 1 |
| 4 | 21 | 8.5 | 20 | 8.3 | 1 |
| 5 | 10 | 13.8 | 10 | 13.4 | 1 |
| 6 | 5 | 19.0 | 5 | 18.6 | 1 |
| 7 | reserved | reserved| reserved | reserved | 8 |
- The 3-5 bits are used to set the EXT_SYNC_SET which is currently set to input disabled, otherwise it allows for short strobes to be captured wiht the FSYNC pin.

#### GYRO_CONFIG:

- *Set to 24 in the sample code.*
- The 3&4 bits correspond to FS_SEL which selects the full scale range of gyroscope outputs.
| FS_SEL | Full Scale Range |
| ---- | - | 
| 0 | +- 250 | 
| 1 | +- 500 | 
| 2 | +- 1000 | 
| 3 | +- 2000 | 
- Bit 5 is the ZG_ST, and setting this bit causes the Z axis gyroscope to perform self test.
- Bit 6 is the YG_ST, and setting this bit causes the Y axis gyroscope to perform self test.
- Bit 7 is the XG_ST, and setting this bit causes the X axis gyroscope to perform self test.

#### INT_ENABLE: 

- *Set to 1 in our sample code*
- Bit 0 is DATA_RDY_EN and when set to 1, this bit enables the Data Ready interrupt, which o ccurs each time a write operation to all of the sensor registers has been completed.
- Bits 4&5 also have purpose, however we will not use them in our project.