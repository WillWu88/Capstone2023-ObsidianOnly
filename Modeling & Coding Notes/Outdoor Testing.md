In preparations for ESE Day we began running our PiCar outdoors, these tests allowed us to gather sensor data to later be used in a Kalman Filter, see the current issues with the Picar's running ability, and test our current scripts for running the PiCar.

## Day 1: 4/10/2023

### Test intention

The first outdoor test day was ran mainly with the purpose of gathering RPM and IMU data. These tests went better than expected and we were able to essentially manually run the PiCar completely around the track using our motor and servo control programs. 

### Other things learned

From our outdoor test we also learned that our servo angle of 0 wasn't actually straight ahead, and that a servo angle of 3 is in fact much closer to straight ahead. The outdoor test also allowed us alot of insight into the speed the car should be running around the track, both on the straight aways and when turning, and gave a clear indication that a PID controller to keep the car from veering off is essential for our final product. 

### Goals for next test

In our next test we hope to gather GPS data of the track. In addition, we hope to run more specific/intentioned RPM and servo tests including on straight aways, during curves, and potentially inside, this will allow us to better see the effects kalman filtering is having on our data and have a better indication of what the data should look like after our outdoor test is complete. 