# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## Background

This project aims to drive the car around the track in the Udacity [simulator](https://github.com/udacity/self-driving-car-sim/releases) by leveraging PID control loop.

The following **states** are provided by the simulator at every communication instance:
- **Way Points**: The upcoming centers of the track in meters
- **Car Location**: The car location in the world coordinate in meters
- **Car Orientation**: The car orientation in the world coordinate in [0, 2*PI]
- **Car Speed**: The speed of the car in mph
- **Steering Angle**: The current steering angle of the car in radian

The following **actuations** are given to the simulator by the MPC model:
- **Steering Angle** Positive for turning to the left and vice versa in [-1, 1], corresponding to [-25, 25] degrees.
- **Throttle/Brake**: Positive for throttle and negative for brake in [-1, 1]

The core idea of model predictive control (MPC) is to leverage the knowledge of vehicle dynamics to optimize a finite time-horizon. At each instance only the current control is applied but optimizing both present and future allows MPC to anticipate future events. PID control does not have this predictive capability. 

## Optimization Model

####Vehicle dynamics:

![equations](equations.png "Vehicle Dynamics Equations")

####Actuation limits:
- The steering angle is limited to [-25, 25] degrees
- Brake/Throttle is limited to [-1, 1] (100% brake/100% throttle)

####Cost:
- **Error**: Cross Track Error, orientation error, and speed error. These errors enforce the model to stay on desired track and speed.
- **Actuation**: The amount of actuation being used. This aims to achieve the optimal path with minimal amount of actuation being used.
- **Actuation Delta**: The rate of actuation change. This avoid erratic actuation being applied to upset the vehicle.

####Finite time-horizon:
The extend of future to be optimized is being controlled by time step **dt** and number of time steps **N**.

####Optimization
**Ipopt** is used to minimize the cost using the actuations under the constraint of vehicle dynamics and actuation limits in the finite time-horizon.

## Hyper-parameters Tuning



## Example
<p align="center">
  <img src="example.gif" alt="MPC Control Loop at Work"/>
</p>

Full video can be found [here](https://youtu.be/BIWhmaCRS7Q)

## Other Notes

