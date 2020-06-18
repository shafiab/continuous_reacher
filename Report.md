## Introduction
[DDPG (Deep Deterministic Policy Gradient)](https://arxiv.org/pdf/1509.02971.pdf) is a a model-free, off-policy actor-critic algorithm using deep function approximators that can learn policies in high-dimensional, continuous action spaces. The class of actor-critic algorithms trains two neural network simulaneously - the actor is a policy based RL whereas the critic is a value based RL. In DDPG, the actor network maps input states to an action deterministically (which is useful for continuous action space as in this problem) whereas the critic network maps the (state, action) pair to Q-value.

## DDPG Implementation and Issues
The original DDPG algorithm from the paper is re-produced below:
![algorithm](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/26ab0b5d-bb78-4c2c-bc06-0d9c4b45e139/Screen_Shot_2020-06-11_at_2.38.28_AM.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200618%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200618T034714Z&X-Amz-Expires=86400&X-Amz-Signature=4ab5e4c103e16a5f5e120369e5a5989f3ac62666c88087c0a915249fc8120235&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screen_Shot_2020-06-11_at_2.38.28_AM.png%22)

## Model Hyperparameters
## Reward Plot
[image_1]: reward_plot.png "Rewards vs. Episodes"
![Trained Agents][image_1]

## Saved Model
## Future Work
