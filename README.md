# Project: Continuous Control (Reacher)

## Introduction

In this project, I trained a deep reinforcement learning agent using [DDPG](https://arxiv.org/pdf/1509.02971.pdf) (Deep Deterministic Policy Gradient) algorithm to move a double jointed arm to target location on Unity ML-agent. Unity Machine Learning Agents (ML-Agents) is an open-source Unity plugin that enables games and simulations to serve as environments for training intelligent agents. This particular setting is known as the [reacher](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Examples.md#reacher) environment.

Here is how the trained agent behave:

[image_1]: reacher.gif "Trained Agents"
![Trained Agents][image_1]


## The Environment

In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of the agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with 4 numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

## How to Navigate this Repo
