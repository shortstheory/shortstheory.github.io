
Model Predictive Control is a popular technique for optimal closed loop control of a system. Historically, MPC was first used in the industrial oil refinery, back in the days when compute was scarce and expensive. By applying an MPC control scheme, plant operators could save a lot of money which more than justified the cost of using expensive computers. Over time, with increases in compute and efficiency, it became feasible to run optimization procedures for realtime applications. MPC is now used in areas such as aircraft autopilot, traction control in modern vehicles, and even HVAC systems to reduce energy costs.

In robotics, MPC plays an important role in trajectory generation and path following applications. We can even define what optimal means depending on our application. For instance, for an autonomous drone, an optimal trajectory may be one that minimizes the energy used. On the other hand, for a robotic manipulator, we would want to optimize for minimizing the deviation from a planned trajectory.

One of my favorite parts of MPC is that the theory delves into a number of diverse topics such as control theory, convex optimization, and even computational geometry. With this blog post, I want to give an introduction to MPC and some of its associated concepts.

## Discrete Systems

Most practical MPC implementations work with linear, discrete time systems. As the name implies, these are represented with a linear equation as:

$$x_{t+1}=Ax_t+Bu_t$$

where $x$ represents the state of the system, $A$ is the state transition matrix, $B$ is the input matrix, and $u$ is the input. $t$ refers to a particular timestep and the value depends on how you choose to discretize your system. For example, if your control loop runs at 10Hz, you would want to set $delta_t$ to 0.1s. For a drone, our choice of state $x$ may be a vector representing roll, pitch, and yaw, and our control input $u$ can be a vector of throttle inputs to the drone's motors. 

What the above equation is telling us is that given the current state and control input of our system, we can calculating the state of the system at the next timestep. This is the 'model' of the system we use in our control scheme. 

This simple equation is surprisingly powerful and it can be used to represent many different types of control systems. Systems with non-linear functions (such as sine, cosine, etc.) in their state transition matrix can be linearized about an operating point.

## Cost Function

There are many ways to formulate an MPC problem, but the one we're going to look at here is picking a series of $n$ control inputs $u_0, u_1,..,u_n$ to stabilize a system to equilibrium. This means that by applying these $n$ inputs one after the other, we should be able to make the state of the system result in all zeroes at the end.

