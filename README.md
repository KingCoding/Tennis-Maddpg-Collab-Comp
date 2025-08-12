# Tennis-Maddpg-Collab-Comp
## Description
Implementation of a solution to training a Tennis environment with 2 agents using the Multi agent reinforcement Learning with DDPG algorithm and with collaboration and competition.

The environment of this Tennis game provides observation states for 2 agents whose positions are represented by the positions of two rakets. The goal of the 2 agents is to collaborate while competing in order to maximize their collective rewards.
Precisely, If an agent hits the ball over the net, it receives a reward of +0.1. If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01. Thus, the goal of each agent is to keep the ball in play.\
\
The task is episodic:
- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the     maximum of these 2 scores.
- This yields a single score for each episode.\
The environment is considered solved, when the average (over 100 episodes) of those scores is at least +0.5.

The observation space consists of 3 consecutive frames of 8 variables corresponding to the position and velocity of the ball and racket. The size of an observation for each agent is 24. The environment is in 2 dimension; so 2 variables are used to represent the position and velocity of the ball and of the racket. Which gives a size of 2x2 +2x2 = 8 for each frame.\
## REQUIREMENTS
To set up your python environment to run the code in this repository, follow the instructions below.\
1- Create (and activate) a new environment with Python 3.6.\
- **Linux** or **Mac:**
  '''
  conda create --name drlnd python=3.6
  source activate drlnd
  '''
- ***Windows:***
  '''
  conda create --name drlnd python=3.6 
  activate drlnd
  '''

2- 

