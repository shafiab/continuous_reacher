## Introduction
[DDPG (Deep Deterministic Policy Gradient)](https://arxiv.org/pdf/1509.02971.pdf) is a a model-free, off-policy actor-critic algorithm using deep function approximators that can learn policies in high-dimensional, continuous action spaces. The class of actor-critic algorithms trains two neural network simulaneously - the actor is a policy based RL whereas the critic is a value based RL. In DDPG, the actor network maps input states to an action deterministically (which is useful for continuous action space as in this problem) whereas the critic network maps the (state, action) pair to Q-value. DDPG has some interesting improvement as follows:
  - Similar to Deep Q-learning (DQN) algorithm, DDPG uses replay buffer and sampling to create IID samples.
  - Similar to DQN, DDPG uses target network for both the actor and the critic. So, in total 4 neural networks. The target networks are time-delayed copies of their original networks that slowly track the learned networks. Using these target value networks greatly improve stability in learning. DDPG implements a soft target update rather than directly copying the weights.
  - Exploration policy in DQN is different from the traditional ε-greedy policy in that exploration policy is constructed by adding noise to the actor policy. The original paper as well as this implemation uses an Ornstein-Uhlenbeck process (Uhlenbeck & Ornstein, 1930) to add noise to the action output.
  

## DDPG Implementation and Issues
The original DDPG algorithm from the paper is re-produced below:
![algorithm](ddpg_algorithm.png)

My implementaion pretty much follows the algorithm shown above and is adopted and modified from the udacity example implementaton [here](https://github.com/udacity/deep-reinforcement-learning/blob/master/ddpg-bipedal/ddpg_agent.py). In particular, I was facing issues related to the model convergence and my training was really slow. So, I consulted the chats in Udacity RL course and based on some of the suggestions there, modified the code as follows:

- The original algorithm updates the weights of the actor, critic networks as well as soft update of the both target network in each time step of each episode. However, I faced convergence issue with that approach. So, instead in my approach I included a step-counter and performed these updates every fifth timestep.
- I included a batch normalization step after the first layer in both actor and critic model, which improved the convergence speed.
- Originally I tried the hyperparameter suggested in the paper. However, I had to modify the parameters to get convergence (more on this in the next section).
- In other projects I tried with episodic task, I usually limited my timesteps somewhere from 100 to 500. However, for this project while trying out 500 timesteps per episode, the average rewards over 100 episode started oscillating around 19 and stopped improving. I removed the timesteps limit from the episode and used `while True` and break the episode was `done==True`. This fixed the issue.

## Model Hyperparameters
### Actor Network
- input = state_size
- network = 256 x 128
- output = action_size
- batch normalization after the first layer
- Adam optimizer with learning rate 1e-3
- Weight initialization followed the process described in Section 7 of the paper
- tanh activation in the output layer to limit the range of action. relu activation in other layers.

### Critic Network
- first layer input = states
- first layer size = state_size x 256
- batch normalization after first layer
- second layer input = action (this is similar to the orignal paper's suggestion)
- second layer size = 256+action_size x 128
- output layer = 128 x 1
- Adam optimizer with learning rate 1e-4.
- Weight initialization followed the process described in Section 7 of the paper. However, unlike the paper the final layer was from a uniform distribution [−3 × 10−3, 3 × 10−3] 
- relu activations in the intermediate layer.
- Unlike the paper, I didn't use any weight decay.

### Target actor and critic Networks
- delayed copy of actor and critic network, with soft-update parameter tau set to 1e-3

### Experience Replay
- input buffer size of 1e6
- sample/batch size of 512

### Q value
- discount factor gamma was set to 0.9

### Ornstein-Uhlenbeck Noise Generation
- code is the same from the udacity implementation
-  mu=0, theta=0.15, sigma=0.1 was used (in the paper sigma=0.2)

## Reward Plot and Convergence
A reward vs episode plot is presented below. While others have success in fewer episodes, in my case unfortunately it took quite a while to reach the target average reward of over 30 over 100 epsisodes. The model reached the target reward in 517 episodes.

[image_1]: reward_plot.png "Rewards vs. Episodes"
![Trained Agents][image_1]

## Saved Model
- Saved actor weights [here](https://github.com/shafiab/continuous_reacher/blob/master/checkpoint_actor_final.pth)
- Saved critic weights [here](https://github.com/shafiab/continuous_reacher/blob/master/checkpoint_critic_final.pth)
- The loads can be loaded by following the code in the [notebook](https://github.com/shafiab/continuous_reacher/blob/master/Continuous_Control.ipynb)
- Since I trained the model using gpu on udacity workspace and then loaded the weight on my macbook to see the trained model at work, I faced an error. Some stack overflow search suggested to include `map_location={'cuda:0': 'cpu'}` while loading the models on cpu and it woked for me.
```
    agent.actor.load_state_dict(torch.load('checkpoint_actor_final.pth', map_location={'cuda:0': 'cpu'}))
    agent.critic.load_state_dict(torch.load('checkpoint_critic_final.pth', map_location={'cuda:0': 'cpu'}))
```

## Future Work
Few future ideas and improvement can be done here:
- I trained using the the first version containing a single agent. The second version  contains 20 identical agents, each with its own copy of the environment. In the second version,  multiple agents can potentially share experience to accelerate learning. It would be an interesting extention to try out.
- The convergence took some time in my case. It would be interesting to do some tweaking with the hyperparameters to see if its possible to improve the convergence speed, in particular the UPDATE_EVERY parameter that I added.
- Other algorithms, e.g. Trust Region Policy Optimization (TRPO), Truncated Natural Policy Gradient (TNPG), Distributed Distributional Deterministic Policy Gradients (D4PG) as suggested in the project page could be interesting to try out.
