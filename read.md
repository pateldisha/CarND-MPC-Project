
### CarND-Controls-MPC
   Self-Driving Car Engineer Nanodegree Program
---

### The Model
This project implements a Model Predictive Controller in the given simulator. 

Kinematic bicycle model is being used for this. The state includes position x and y, orientation, velocity, cross-track error and orientation error. State values are calculated using the respective equations for each. 

      // x_[t+1] = x[t] + v[t] * cos(psi[t]) * dt
      // y_[t+1] = y[t] + v[t] * sin(psi[t]) * dt
      // psi_[t+1] = psi[t] + v[t] / Lf * delta[t] * dt
      // v_[t+1] = v[t] + a[t] * dt
      // cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt
      // epsi[t+1] = psi[t] - psides[t] + v[t] * delta[t] / Lf * dt

### Timestep Length and Elapsed Duration (N & dt)

The horizon T is the one which is predicted and it is calculated as N * dt. N is the number of the timesteps and dt is the elapsed duration which indicates the time passes between every actuation. 

The values I chose for N and dt are N = 10 and dt = 0.1 i.e. 100 ms.

I started with using values N = 25 and dt = 0.05 and tried few more values so that the vehicle does not osciallte more and completes the track correctly. 
What we can notice here is, it will oscillate more at high value of N and vice versa. 

### Polynomial Fitting and MPC Preprocessing

Here, we convert the points from global coordinate system to the car coodinate system. So, the first point will be taken as origin and the orientation will also be made 0 as car will be pointing to the reference trajector ( it will be straight to it). 

Then will use polyfit function for points to be fitted to a 3rd order polynomial. Then will calculate the cross track error using the polyeval function.

### Model Predictive Control with Latency


Here we take care of delay of 100ms, because of which the model will not oscillate near the reference trajectory. So, introducing this delay will make sure about the future position of the vehicle.
For example, the time elapsed between when we command a steering angle to when that angle is being achieved. So, we introduce latency to take care of this situation.
