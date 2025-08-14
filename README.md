# Tennis-Maddpg-Collab-Comp
![Tennis snapshot](https://video.udacity-data.com/topher/2018/May/5af7955a_tennis/tennis.png)
## Description
Implementation of a solution to training a Tennis environment with 2 agents using the Multi agent reinforcement Learning with DDPG algorithm and with collaboration and competition.

The environment of this Tennis game provides observation states for 2 agents whose positions are represented by the positions of two rakets. The goal of the 2 agents is to collaborate while competing in order to maximize their collective rewards.
Precisely, If an agent hits the ball over the net, it receives a reward of +0.1. If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01. Thus, the goal of each agent is to keep the ball in play.\
\
The task is episodic:
- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. We then take the     maximum of these 2 scores.
- This yields a single score for each episode.\
The environment is considered solved, when the average (over 100 episodes) of those scores is at least +0.5.

The observation space consists of 3 consecutive frames of 8 variables corresponding to the position and velocity of the ball and racket. The size of an observation for each agent is 24. The environment is in 2 dimension; so 2 variables are used to represent the position and velocity of the ball and of the racket. Which gives a size of 2x2 +2x2 = 8 for each frame.
## REQUIREMENTS
To set up your python environment to run the code in this repository, follow the instructions below.\
1- Create (and activate) a new environment with Python 3.6.
- **Linux** or **Mac:**
  ```
  conda create --name drlnd python=3.6
  source activate drlnd
  ```
- ***Windows:***
  ```
  conda create --name drlnd python=3.6 
  activate drlnd
  ```

2- Install OpenAi gym from [this repository](https://github.com/openai/gym)
- install the classic control environment group through [this link](https://github.com/openai/gym#classic-control)
- install the box2d environment group through [this link](https://github.com/openai/gym#box2d)

3- If you haven't yet done it, clone this repository, then navigate to the main Tennis-Maddpg-Collab-Comp/ folder.\
   There is no need to run the command pip install . in order to make initial python installation, because the main project file does it first
  ```
  git clone https://github.com/abhismatrix1/Tennis-Maddpg-Collab-Comp.git
  cd Tennis-Maddpg-Collab-Comp
  ```
4- Download the Unity environment for this project that mathes your operating system:
- Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
- Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip)
- Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)
- Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)

(For Windows users) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

(For AWS) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux_NoVis.zip) to obtain the "headless" version of the environment. You will not be able to watch the agent without enabling a virtual screen, but you will be able to train the agent. (To watch the agent, you should follow the instructions to [enable a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md), and then download the environment for the Linux operating system above.)


5- Create a folder /data/Tennis_Linux_NoVis/ in the Tennis-Maddpg-Collab-Comp/ repository folder and Place the downloaded Unity environment file in this GitHub repository, unzip (or decompress) the file.

## EXECUTION
Run the intructions in the Tennis-MADDPG-COLL-COMP.ipynb file to train the Tennis agents.
