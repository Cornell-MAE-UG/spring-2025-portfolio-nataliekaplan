---
layout: project
title: Roomba Systems Analysis
description: Analysis of the system dynamics of a Roomba for System Dynamics class
technologies: [MATLAB]
image: /assets/images/roomba.jpg
---

**Overview**

In a group of 4, I studied the localization and path following algorithms of a Roomba. We chose this because many of the group members are interested in robotics and a Roomba is a simple yet insightful example of robot control. We investigated the Kalman filter and the Extended Kalman filter, which are feedback control systems that use sensor data to continuously update the robot's believed pose. We will also investigate the Roomba’s path following control, including both its forward velocity and heading angle controller. Lastly, we will take a look at the control algorithm behind the spinning dust collection system. We studied the state space model, block diagrams, transfer functions, inputs and outputs, and feedback control law of each of these smaller controllers that make up the Roomba. 

My contribution to this project focused on the KF and EKF. I studied the Kalman filter to better understand the EKF by creating a block diagram of the closed loop system. I also simulated the EKF in MATLAB to visualize its accuracy. The section I wrote is below. If you are interested in the full report, it can be downloaded in PDF form by clicking here. <WORK IN PROGRESS - IT WILL BE LINKED SOON>

**Localization of a Roomba**

In order to clean a house, a Roomba must maintain an idea of where it is. To do this, it uses its control inputs combined with sensor measurements and a dynamics model. However, there are many possible sources of error, including but not limited to wheel slippage, sensor noise, mechanical imperfections, and dynamic obstacles. With sources of error in the sensor, dynamics, and control, how does the Roomba know which to trust, particularly when they disagree? 

This introduces the localization problem. Given an initial guess of the robot’s pose (position and orientation in the global coordinate frame) $bel(x_t)$, a model of robot dynamics $p(x_t | x_{t-1}, u_t)$, a measurement function $p(z_t | x_t, m)$, control inputs $u_{1…t}$, and measurements $z_{1…t}$ (where x=position, z=sensor measurements, m=map, u=control inputs), we must find the belief of the current pose $bel(x_t)$. Since there is so much uncertainty, the robot’s pose is modeled as a probability instead of just one value. By using the control inputs, sensor measurements, and dynamics model together, we can reduce the uncertainty in the Roomba’s pose to much lower than it would have been if we had only used one source of data. [1]

Since iRobot does not publicly disclose which localization filter the Roomba uses, I will model it using the Extended Kalman filter. However, in order to study the Extended Kalman filter, we must first know how the Kalman filter works. 

The Kalman filter introduces restrictive assumptions about the dynamics and measurement models to make the Bayes filter implementation plausible and give an exact solution to the robot’s pose instead of an estimation. We assume that the dynamics and measurement models are linear with Gaussian zero-mean noise and that the initial belief is Gaussian. From these assumptions, we know that the belief for any time t must be Gaussian. [1]

The initial distribution is described below:

$$
bel(x_0) \sim \mathcal{N}(0, \Sigma_0)
$$

$$
x_t \in \mathbb{R}^n, \quad z_t \in \mathbb{R}^k, \quad u_t \in \mathbb{R}^m
$$

The state vector of the Roomba has n=3 elements: x, y, and . Elements x and y describe the robot’s position in the global frame, while  describes its orientation. Since we need to use all of this information to describe the robot’s current position, the output vector is the same as the state. The inputs are the speed of each wheel, so m=2. [1] Given this information, the state, input, and output vectors are shown below: 

$$
x_t =
\begin{bmatrix}
x \\ y \\ \theta
\end{bmatrix}
\space\space
u_t =
\begin{bmatrix}
\omega_R \\ \omega_L
\end{bmatrix}
\space\space
y_t =
\begin{bmatrix}
x \\ y \\ \theta
\end{bmatrix}
$$

As mentioned above, the Kalman filter works under the assumption that the dynamics model is linear. The equation is shown below: 
$$
x_t = A x_{t-1} + B u_t + \epsilon_t
$$
[1] where A is an nxn matrix and describes how the state evolves from t-1 to t without controls or noise, B is an nxm matrix and describes how the control ut changes the state from t-1 to t, and $\epsilon_t \sim \mathcal{N}(0, R)$ is the process noise and is represented as a random variable. The process noise is assumed to be independent and normally distributed with covariance R [5]. For more details about the dynamics model, see the Heading Angle Control section of this report. 

The Kalman filter also assumes that the measurement model is linear. The equation is shown below: 
$$
z_t = C x_t + \delta
$$
[1] where C is a kxn matrix that describes how to map the state xt to an observation zt and $\delta \sim \mathcal{N}(0, Q)$ is the measurement noise and is represented as a random variable. The measurement noise is assumed to be independent and normally distributed with covariance Q [5]. 

The Kalman filter cycles between prediction and correction steps to continuously update the robot’s pose. The prediction step, or the dynamics update, updates the pose using the dynamics model. This step uses two equations: one to find the mean $\bar{\eta}_t$, and one to find the standard deviation $\bar{\Sigma}_t$ of the Gaussian describing the pose. The bars represent the prediction step [1]. The equations are shown below: 
$$
\bar{\eta}_t = A \eta_{t-1} + B u_t
$$
$$
\bar{\Sigma}_t = A \Sigma_{t-1} A^T + R
$$
The correction step, or measurement update, uses the measurements from the sensors to adjust our prediction of the pose of the robot. The equations to find the mean and standard deviation are shown below: 
$$
\eta_t = \bar{\eta}_t + K_t \left(z_t - C \bar{\eta}_t\right)
$$
$$
\Sigma_t = (I - K_t C)\bar{\Sigma}_t
$$
The term $\left(z_t - C \bar{\eta}_t\right)$ is the discrepancy between the actual and predicted measurements and its weight is controlled by the Kalman gain $K_t$, which describes how much to trust the sensor measurements. As mentioned above, neither the dynamics model nor the measurement model is free of error, so the Kalman gain is used to balance them. If the sensor is very noisy, $K_t$ will be close to 0 [1]. The equation is shown below: 
$$
K_t = \bar{\Sigma}_t C^T \left(C \bar{\Sigma}_t C^T + Q\right)^{-1}
$$
A block diagram of the closed loop system for calculating the mean position is shown in Figure 1. This diagram illustrates the loop between the prediction and correction stages of the filter.

<p align="center">
  <img src="{{ '/assets/images/KF_block_diagram.png' | relative_url }}" width="550">
  <br>
  <em>Figure 1: Block diagram of the Kalman filter showing the prediction and correction loop.</em>
</p>

We cannot use the Kalman filter on this system since its dynamics model is not linear. However, we can use the Extended Kalman filter, which relaxes the linearity assumption of the Kalman filter by using Jacobians to linearize the dynamics model locally at each time step. Although we are moving from an exact solution to an approximation, the relaxation of this assumption makes the EKF applicable to many more systems, including this one. 

<p align="center">
  <img src="{{ '/assets/images/EKF_plot.png' | relative_url }}" width="550">
  <br>
  <em>Figure 2. True trajectory vs. EKF estimate of robot pose.
  </em>
</p>

The plot in Figure 2 shows the true trajectory compared to the EKF estimate of the pose of a differential drive robot driving with a forward velocity of 0.5 m/s and an angular velocity of 0.1 m/s using a noisy GPS-like sensor model. The plot shows that the EKF does a very good job of estimating the pose of the robot. The filter quickly hones in on the true pose and stays very accurate. The small errors are due to the linearization of a circular path. In reality, at every time step there may be a different control input and GPS cannot be used indoors. 

By repeatedly propagating the dynamics forward and correcting with measurements, the EKF leverages the system model to keep the pose estimate tightly bounded in the presence of noise.

**References**

This is a list of references used in the localization section of the report. The full list can be found in the PDF linked above. 

[1] A. Bizyaeva. MAE 4180. Class Lecture, Topic: “Kalman Filter.” College of Engineering, Cornell University, Ithaca, NY, Feb. 11, 2025

[2] A. Bizyaeva. MAE 4180. Class Lecture, Topic: “Extended Kalman Filter.” College of Engineering, Cornell University, Ithaca, NY, Feb. 20, 2025

[5] T. Bhattacharjee, P. Culbertson. MAE 4760. Class Lecture, Topic: “Kalman Filters.” College of Engineering, Cornell University, Ithaca, NY, Oct. 16, 2025