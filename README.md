# CarND-Controls-PID
---

## Reflection

### Describe the effect each of the P, I, D components had in your implementation.

P is a direct proportional factor of the cross track error which adjust the steering with an "immediate" response to the bias of the trajectory. In my implementation, it's represented by PID.Kp that is multiplied by the CTE to produce the first part of steering change.

I is an integral control that can correct the long term system bias rather than responses to a single measurement cycle. In my implementation,  it's represented by PID.Ki multiplied by sum of all CTE. I found the system bias is trivial in the simulator, give Ki a large value will cause failure soon, so finally I give it a very small value 0.001.

D is the differential part makes the vehicle reach the trajectory gracefully. it's represented by PID.Kd, which is CTE at t minus CTE at t-1 with fixed internal.  This is important to keep the vehicle on track, like in video "p_only.avi", the vehicle will toward the edge more and more over time, if only with P control.

### Describe how the final hyperparameters were chosen.

I start from **P[1, 1, 1]**, and adds a error calculation for manual twiddle tuning. The P got stable soon at around 1 and I chose 0.9 finally, the I like mentioned above, need to be a trivial value, and the Kd didn't perform well with one stepping, so I increased it to ten stepping and my final choice is 20. 

Although P[0.9, 0.001, 20] can pass, it jiggles a lot since the speed is at about 10 m/h, like in video "pid_steering_only.avi". So I tried to decrease the Kp to lower value like **P[0.1, 0.001, 2]**, then it runs smoothly and the speed can reach ~30m/h, but the vehicle will be close to fail at some large curve like following picture.

![Screenshot](/Screenshot.png)

So I added the 2nd PID for throttle, which will reduce the throttle when the steering changed a lot. The vehicle now can "brake" sometimes.

My final PID of steering is P**[0.5, 0, 3]**. 



