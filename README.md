# Project: Continuous Control (Reacher)

## Introduction

In this project, I trained a deep reinforcement learning agent using [DDPG](https://arxiv.org/pdf/1509.02971.pdf) (Deep Deterministic Policy Gradient) algorithm to move a double jointed arm to target location on Unity ML-agent. Unity Machine Learning Agents (ML-Agents) is an open-source Unity plugin that enables games and simulations to serve as environments for training intelligent agents. This particular setting is known as the [reacher](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#reacher) environment.

Here is how the trained agent behave:

[image_1]: reacher.gif "Trained Agents"
![Trained Agents][image_1]


## The Environment

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of the agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with 4 numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

The task is episodic. The benchmark performance is an average score of +30 over 100 consecutive episodes.

## How to Navigate this Repo
The repo consists of the following files:
- `Continuous_Control.ipynb` contains a jupyter notebook with all the codes to train and run the agent.
- `Report.MD` contains description of the algorithm and hyper-parameters and performance of the algorithm.
- `checkpoint_actor_final.pth` and `checkpoint_critic_final.pth` contains the weights of trained actor and critic neural network. See  `Continuous_Control.ipynb` for example of how to load these weights.

## Setup / How to Run?

I trained the agent using gpu in a workspace provided by Udacity. However, the workspace does not allow to see the simulator of the environment. So, once the agent is trained, I load the trained network in a Jupyter Notebook in macbook and observed the behavior of the agent in a pre-built unity environment. The steps for the setup is as follows:

- Follow the instruction in the [DRLND GitHub repository](https://github.com/udacity/deep-reinforcement-learning#dependencies) to set up your Python environment. 
- Once the setup is done, we can activate the environment and run notbook as follows:
```
source activate drlnd
jupyter notebook
```
This will open a notebook session in the browser.
- The pre-build unity environment `Reacher.app.zip` is also included in this repo.
- So, we can just use the ` `Continuous_Control.ipynb` notebook to train and run the agent. All codes are included in that notebook.



