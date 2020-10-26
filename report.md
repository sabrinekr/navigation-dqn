# Introduction
In this project, we aim to train the agent to navigate a virtual world in the environment that is similar to Unity's Banana Collector environment. 

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana. Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas.

The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around agent's forward direction. Given this information, the agent has to learn how to best select actions. Four discrete actions are available, corresponding to:

0 - move forward.

1 - move backward.

2 - turn left.

3 - turn right.

The task is episodic, and in order to solve the environment, the agent must get an average score of +13 over 100 consecutive episodes.

# Learning Algorithm
The deep Q-learning came to solve problems with huge space state that the Q-learning is not able to solve. The deep Q-network takes as input the state and outputs Q-values for each possible action in the current state. The biggest q-value corresponds to the best action. We implemented the Double Deep DQN which is an improved version of DQN using the:
* Experience replay buffer: it helps to avoid two problems which are forgetting previous experiences and the correlations in data. In reinforcement learning we receive at each time step a tuple composed by the state, the action, the reward, and the new state. In order to make our agent learn from the past policies, every tuple is stored in the experience replay buffer. To break correlations between data, experiences are sampled from the replay buffer randomly. This will help action values from diverging. 

* Fixed target Q-network: we used to use the same weights on the predicted and the target values. The predicted Q-value is removed closer to the target but also the target is removed with the same steps as the same weights are used for both of them. As a result, we will be chasing the target value and we will have a big oscillation in the training. To break correlations between the predicted and the target value, we use the fixed target Q-network to update the weights of the target. The Q-targets are calculated using the fixed parameters w − of the separate network. 

We chose to train our agent using Double DQn because it came to solve the problem of the overestimation of the Q-values in DQN. At the beginning of the training, we don’t have a lot of information about our environment and we are not sure that the best action is the action with the highest Q-value. To make our implementation more robust, we use two Q-networks. One  is used to select the best action and the other is used to evaluate that action. 

* The Q-network: selects the best action with maximum Q-value of next state.
* The target Q-network: calculates the estimated Q-value the best action selected. 
![Image of Yaktocat](https://github.com/sabrinekr/navigation-dqn/blob/main/images/algorithm.png?raw=true)

# Model architecture
The DQN agent has a target and local networks having the same architecture:
* 1 fully connected layer of size 64.
* 1 fully connected layer of size 64.
* 1 output layer of size 4 (the size of the action space) 

# Hyperparameters 
Our agent was trained using the follwing hyperparameters: 
* Buffer size: the size of the experience replay buffer is 100 000
* Batch size: the batch size of the training is 64 
* Gamma: the discount factor 0.99
* TAU learning rate coefficient for soft update of target parameters is 0.001
* The agent is updated after every 4 time steps

# How to train our agent
For each episode, we start by giving the initial state of our environment to the agent. Then, for each time step we give our agent the current state of our environment and he will return the action that he will perform. After performing this action, the environment will return the new state, the reward and if the game is finished or not. The agent will save this experience in the replay buffer. 

When we reach the score of 13 we stop the training of our agent. 

# Results

The agent was able to solve the environment after 514 episodes with the average score of 13.08 during the last 100 episodes.

![Image of Yaktocat](https://github.com/sabrinekr/navigation-dqn/blob/main/images/dqn1.PNG?raw=true)
![Image of Yaktocat](https://github.com/sabrinekr/navigation-dqn/blob/main/images/dqn2.PNG?raw=true)

# Ideas for Future Work
We still can improve our results by: 
* Implementing Dueling DQN
* Implementing the prioritized experience replay
* Tuning the hyperparameters