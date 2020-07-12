https://youtu.be/3bLvWvgwbtI


this is implementation of Linear model predicitive controller, as example of a dynamical controller to lateral vehicle motion

We use here LQR (Linear Quadratic Regulator) here, because it is the closed form solution of MPC when it is linear.

The controller is applied to discrete control problem

states = [x, y, theta, delta]

input = [v, phi]

theta: heading angle (yaw), angle between current path and desired trajectory

delta: steerting angle

v : vehicle speed

phi: steering angle rate of change (i.e. delta dot)

#Q is the weighted squares of deviation of states from target

Q = np.identity(n)     # n: number of states, a good choice to start with identity matrix

#R is the weighted squares of control activity

R = np.identity(m)     # m: number of inputs, a good choice to start with identity matrix

#you should tune values of Q and R manually

#here n = 4 (4 states), and m = 2 (2 inputs)

#cost function or objective function: here it is required to minimize difference control input magnitude for passenger comfort, and to minimize deviation from reference (desired trajectory)

#cost function: regulation type, and tracking type

#regulation drives all inputs to zeros, we will use this

#tracking drives all inputs to desired values

#I used receding horizon algorithm with T time steps backward

#update state using this equation x(t+1) = A*x(t) + B * u(t)

beta = delta + theta

#we are using lineaized MPC

#so sin(beta) = beta, and cos(beta) = 1, small angle approximation
 
st = time_step

A = np.identity(4)

B = np.array([ [st, 0] , [beta, 0] , [0, v*np.sin(delta)/L] , [0, st] ])
            

finally we solve for new state using A, B, Q, R as inputs to LQR algorithm
