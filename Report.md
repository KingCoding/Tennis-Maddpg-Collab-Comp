## INTRODUCTION
This project about collaboration and competition of AI agents to solved a Tennis environment, has been a challenging, exciting and enriching experience. And I think that achieving a professional solution for this project helped me discover a general framework for training Multi-Agent environments with Reinforcement Learning.
There has been a lot of try and fail in the process of finding the right method. A great part of the complexity came from the fact that there was quite some ambiguities in understanding the environment. I could not make good sense of the data provided in the state of each agent. Until I went in the student group and searched posts related to the project. Then I was able to make a clear sense of the states provided to the agents. Each state contains 3 frames of data taken at distinct but very close times. When I understood that, I first tried to simplify the states by considering only one frame of data; but despite all my efforts, I couldn't find a working solution.
After I stopped trying to simplify the states by considering only one of the 3 frames, I started learning from the course notes and from the preparation lab project; to try to find an intuition for solving this environment. Understanding the preparation project took me a lot of time. Because I was trying to make sense of almost all its parts in order to equip myself optimally for this project.
And after trying many solutions in vain, I finally got a very bright intuition that led to a very flexible solution that is also fast. One of the best aspect of my solution is that it utilizes a lot more collaboration of both agents than just sharing the same replay buffer without compromising the competition between the agents. That is, at each learning step, agents really utilize each other observations to perform their trainings.

## LEARNING ALGORITHM
The bright intuition I had for this project came when I first considered a practical approach by thinking about what a tennis player in the real physical world needs to take decisions while playing.
In other words, what are all the information he needs on the court either from him and from his opponent, to take good decisions for his actions?
After pondering on the above question for a while, I realized that all what is needed by an agent to take good decisions for his actions are:
- His position (or in the context of our environment, the position of his racket)
- The position of the ball (his observation of the ball should be sufficient)
- The position of his oponent (similarly of the racket of his opponent)

I first tried manipulating the states of the environemnt to extract just the informations I needed to pass them to the actor network. Because I used a DDPG approach to solve this problem. It was easy to take this route not only because that was the algorithm presented in the course notes, but that was also the algorithm used to solved the lab project that was given for preparation. So I thought to myself that there is certainly a way to solve to project using the DDPG algorithm.

So when I tried to extract just the information I needed from the states, I didn't get good training performances. And plus, my implementation was complicated.

The solution came when I tried to consider the whole court as a state. And the actions that lead to next states are the individual (not collective as I was first trying to do) actions of each agent. So when an agent takes an action, it leads to a new state. I was very confident about this intuition and it led to a very simple implementation.

In fact, to derive a state that represents the whole state of the court, it's just necessary to concatenate the states of both agents. It's true that there is quite some redundant information like the positions of the ball for both agents (because I believe that one position of the ball from the observation of the agent taking action is sufficient), but I later learned that just concatenating both agent states is much efficient than trying to remove redundant information; and it doesn't add too much cost to the training

So after concatenating the states of both agents, we get a general states that give a full representation of the court. I named it statefull (and similarly nextStateFull for the next state).
This is how I achieved improved collaboration between both agents than just using the same replay buffer for both agents. And more importantly, the improved collaboration was achieved without sacrificing the competition between the agents. MY SOLUTION first MAde THE 2 agents play the tennis like a real world game by trying to win the game instead of just collaborating through a repetitive routine (like always sending the ball towards the center) as it is with many other solutions without enough collaboration.
In fact, I thought of using a solution with minimal collaboration that just relies on using the same replay buffer; and I even saw many students implement that. But not only their solution was not flexible (because they are trying to guess the best action to take without considering the position of their opponent, so they will get into a sort of routine like trying to always send the ball towards the center), but their solution was almost exactly similar to that of the previous project that we did for continous control, the only difference being that they were using individual actor and critic networks for each agents instead of the same actor and critic network for all agents.
So I wasn't satisfied with that minimal collaboration solution, because I thought that if that was a good solution, then we would not even need to understand the course notes for this project.

So like I said, while trying to improve the collaboration, I concatenated the states and the concatenated state is enough as entry to the actor network, to predict the next action for each agent. And while we get the predicted action, the rest of the training proceds like a regular DDPG training. We feed the concatenated state and the predicted action to the critic network for each agent to get an estimate of the expected reward; and the rest is pretty much theoretical routine.

So far it worked very well. But there was a problem. Because I fed the actor networks with the concatenated states, I violated the multi agent algorithm presented in the course notes on the diagram below.
The multi agent algorithm encourages collaboration for training the critic networks during training, but it discourages collaboration for the actor networks either during training or during execution.
And the reason being that if there is collaboration for training actor networks, we will not be able to execute actors networks using only personal observations of agents during execution.
And that's the mistake I made. I maximized the collaboration to facilitate the training of the actor networks. SO the actor networks were taking as input the full observation of the game instead of individual observations for each agents.

So the final adjustement was to reduce the input of actor networks to just the individual observations of the agents. But still consider the full state of the game made of observations of all agents when training the critic networks. It's true that it sacrifices a bit of collaboration and flexibility, but the solution is more independent. Agents won't have to consider their opponent positions during execution time.

The general diagram given in the course notes for the MUlti-Agent Reinforcement Learning course can help summarize the way our algorithm works. In green we can see the flow of process and information during training. We see that each critic network (Qi) utilizes information (observations/actions) from many agent in order to make prediction and perform the training and actors (Pi) rely on critics during training. But in red, the flow of process and information during execution shows us that each agent utilizes its personal observation and actions to perform the execution.

![MADDP diagram](https://github.com/KingCoding/Tennis-Maddpg-Collab-Comp/blob/main/pictures/MADDPG%20diagram.png)


At the end, I came to realize that both solutions are very good depending on the goals to achieve. If we want to make the agents play independently, we have to implement the exact solution of the MUlti-Agent Reinforcement Learning course. That is, feed the actors networks with individual observations of agents without using any collaboration. But using collaboration for the critic networks.
But if we want a more robust, flexible and practical solution, we should also use collaboration for actor networks by feeding the whole observation of the game (made from observations of all agents) to the actors networks while maintaining the rest of the implementation unchanged.

The architecture for the networks used was pretty simple. Because I had the bright ideas to take the architecture from a functional DDPG project and start from there; then with that, I was confident that if there was a problem, it was probably due to my algorithm and not to the architecture I borrowed from a functional DDPG project.
The first architecture I borrowed was from the lab project, I tried to change some parameters to fit their netowrk to the size of my project (states, actions). But since I was struggling to make things work, I got the idea to reach out to the student community and borrow the architecture of a student who had already solved the project and who had used the DDPG algorithm. It still didn't work well until I merged the architecture from the lab project and that of a student.
And that's the final architecture I used to make the project work. What I learned from the student was the utilization of batchnorm between entwork layers. Beside that, the concept of the architecture relied of that of the preparation project.

# Networks configuration
- Number of Actor networks = 2 (1 for each agent)
- Number of critic networks = 2 (1 for each agent)
- Size of each actor network = (24, 400, 300, 2)
- Size of each critic network = (48, 256, 128, 1)
- Batchnorm implemented between the first and second and the second and third layers of each networ type

Tunning the hyperparameters to find acceptable parameters was the last technical challenge I face while implementing this project.
I first started with the hyperparameters given in the lab preparation project, but the solution was very unstable. At some point, it looked like the training was unlearning all its progress and the rewards were decreasing almost back to zero.
Sometimes it would work, sometimes it would unlearn and decrease completely.
So the solution came when I decreased my learning rate for both the actor and critic networks. What I copied from the lab preparation project was 10^-2, I decreased them to 10^-3 and voila! the program started working excellently. The training was stable and there was no longer significant deep in rewards values.

# Hyperparameters
- Replay buffer size = 1e6
- Minibatch size = 128
- Discount factor = 0.99
- Soft update of target parameters coefficient(TAU) = 1e-3
- Actor network learning rate = 1e-3
- Critic network learning rate = 1e-3
- How often to update the network (time steps) = 20
- How many times to update networks for each agent = 8

Another bright idea I applied to make to implement my algorithm, was to inspire myself from the previous project to find a way to access and sample the replay buffer.
I first tried to use the method of the lab preparation project, but it seemed that it was inappropriate. Because the preparation project had 4 parallels environments and the way they were accessing and sampling their replay buffer seemed to not be appropriate for my project with no parallel environment.
So I tried to replicate the way the replay buffer was sampled and accessed in the previous project; Continous control. That is, for about 20 iterations, we perform no training, but we keep adding experiences to the replay buffer. Then the 20th iteration, we sample the replay buffer and perform the training afor a certain number of times, for both agents.
And after we run the soft updates for the target networks. This idea worked fine for the first time and I had no problem trying to toubleshoot it. I think it was straigth form heaven; in fact all the inspiration for this project was divine (I remember that before getting the working intuition I prayed to Jesus when I was stucked); because I haven't seen any student implement a solution as flexible, robust and fast as mine.

Lastly, the experiences I saved in the replay buffer were not individual experiences like many students did (which is very similar from that of the previous project for continous control), they were collective experiences for both agents saved in the same list. That is what I learned and utilized the most from the preparation lab project; how to save the experiences of many agent in the same list of experience and how to retrieve them without losing that association. Without this technic of how to save experiences of both agents for a time step collectively, it would have been very difficult, and nearly impossible to improve collaboration and implement a more flexible solution.

The max average score achieved during training was 1.82 at around 1100-1200 episodes, and the environment was solved at around 780 episodes in about 25 minutes.

![Training progress](https://github.com/KingCoding/Tennis-Maddpg-Collab-Comp/blob/main/pictures/Training%20progress.png)

![Training Result Graph](https://github.com/KingCoding/Tennis-Maddpg-Collab-Comp/blob/main/pictures/Tennis-MADDP-Result-Graph.png)
...

## IDEAS FOR FUTURE WORK
These are some improvements ideas from the least important to the most important.

1- Since I didn't try a lot of hyperparameters once i got the algorithm working, it's possible to try new combinations of values for some hyperparameters in order to see if we could achieve better rewards.

2- IMPROVE THE SOLUTION TO MAKE IT FASTER BY DECREASING THE SIZE OF THE STATE FED TO NETWORKS. That can be achieved by minimizing the amount of information considered in the state like the position of the ball could only be considered for 1 agent; not for both agents. We could also find a way to simlify the states even more by considering only one frame of data among the 3 frames provided. That will lead to much lighter stater and will contribute to achieve much faster solutions.
We could even analyze the frames for each state of observation in order to derive only a single frame that contains only the minimum number of information to represent the state.
This way, we could apply the current solution to solve a real tennis game in 3d and even to play a tennis game with multiple more than 2 players.

3- Another improvement idea could be to make the agents more flexible so that their moves are less predictable; which will increase the degree of competition of the agents.
Instead of always trying to find the best move, which could lead to redundant and very predictable behaviors, thus decreasing the competition, we could chose between a set of limited number of moves ranging from a best move with a highest probability and an acceptable move with a least priority; while always being able to counter the best reaction of the opponent.

For instance, if the opponent is in the middle, we could deduct other acceptable moves from the best move by harnessing the symetrical aspect of the game (a great move on the left has a symetrical equivalent move on the right), or by using best predicted moves to generate other acceptable moves by considering varying depth of the ball through controlling the intensity of the impact on the ball.
Then each acceptable move should have a probability depending on the quality of the move (the better the quality, the higher the probability). And each time the agent will chose among one of the acceptable moves based on their probabilities.
This improvement will make the agent more unpredictable and increase their level of competition. This way, due to the great improvement in competition, agents could be used to play even against an experienced human tennis player and even win.
