# Project 2: Continuous Control
## Introduction
In this project, we will explore how policy-based methods can be used to learn the optimal policy in a model-free Reinforcement Learning setting using a Unity environment. In Unity enviroment as shown in figure below, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

![image](https://user-images.githubusercontent.com/128342152/226425641-aee7986b-dbfa-4bf4-b3c6-5ee71bacd2be.png)

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

## Implementation
The algorithm used for this project is DDPG (Deep Deterministic Policy Gradient). In DDPG, there are two networks: an actor network and a critic network. The actor network maps the current state to an action, while the critic network estimates the value of the current state-action pair. The actor network is updated using the deterministic policy gradient, which is the gradient of the expected return with respect to the parameters of the actor network. The critic network is updated using the TD (temporal difference) error, which is the difference between the estimated value of the current state-action pair and the expected value of the next state-action pair. 
One of the key innovations of DDPG is the use of a replay buffer, which stores experience tuples (state, action, reward, next state) and randomly samples them during training. This helps to decorrelate the data and reduces the variance of the gradient updates.

#### Model Architecture
The actor (and the actor target) network uses 3 fully-connected layers:
•	batch normalization -> state_size -> 128 -> Leaky ReLU
•	128 -> 128 -> Leaky ReLU
•	128 -> action_size -> tanh

The critic (and the critic target) network uses 3 fully-connected layers:
•	batch normalization -> state_size -> 128 -> ReLU
•	128 + action_size -> 128 -> ReLU
•	128 -> 1

#### Parameters
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR_ACTOR = 1e-3         # learning rate of the actor 
LR_CRITIC = 1e-3        # learning rate of the critic
WEIGHT_DECAY = 0        # L2 weight decay
LEAKY =0.01             # slope for negative values

#### Performance
The agents solved the environment (by reaching a collective average reward of 30.0 over 100 episodes) in 11 episodes, before the 1000 episode limit.
![image](https://user-images.githubusercontent.com/128342152/226429499-1014582b-9e15-437c-9410-9169b7afea54.png)

## Future Work
Several improvements to DDPG can be considered to improve the performance:
1.	PPO (Proximal Policy Optimization): PPO is a policy gradient method that is more sample-efficient than DDPG and other actor-critic algorithms. It uses a clipped surrogate objective function to update the policy parameters, which prevents large policy updates that can lead to instability.
2.	HER (Hindsight Experience Replay): HER is a technique that can be used with any reinforcement learning algorithm, including DDPG, to improve sample efficiency in tasks with sparse rewards. It works by relabeling the achieved goal in a trajectory with a different goal, which is a state that the agent could have reached from the initial state. This allows the agent to learn from failures and achieve more successful trajectories.
