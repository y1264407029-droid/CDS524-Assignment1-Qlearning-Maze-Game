# CDS524-Assignment1-Qlearning-Maze-Game
Maze Treasure Hunt Game Based on Q-Learning
Youtube: 
Github: https://github.com/y1264407029-droid/CDS524-Assignment1-Qlearning-Maze-Game
 1 Introduction 
This project is the submission for CDS524 Assignment 1, which implements a 5x5 grid maze treasure hunt game based on the Q-Learning reinforcement learning algorithm. The core objective is to train an AI agent to navigate the maze, avoid 3 fixed traps, and find the optimal path from the top-left start point to the bottom-right treasure goal through continuous environment interaction. The project uses Python as the core language, with NumPy for Q-Table management, Matplotlib for performance visualization, and Python’s built-in Tkinter for the interactive GUI. All functional modules have been fully tested and verified via actual operation.
 2 Game Design
 2.1 Rules of the Game
Users can observe the agent’s training process via the GUI and test the trained model’s pathfinding ability through single-step tests. The agent’s core goal is to reach the treasure at (4,4) from the start point (0,0), while avoiding traps at (1,1), (2,3) and (3,0) with the minimum steps. The game runs in episodes, each starting with the agent reset to (0,0). An episode ends immediately if the agent reaches the goal, steps into a trap, or exceeds the 30-step maximum limit.
The game’s state space consists of the agent’s (x,y) coordinates in the 5x5 grid, forming 25 discrete states converted to 0-24 IDs as algorithm input. The action space includes 4 discrete movements: up, down, left, and right, with the agent selecting actions based on the current state and trained Q-Table.
2.2 Class Design of the Game
The project adopts a modular object-oriented design with clear division of responsibilities:
MazeGame Class: Manages the game environment, including map initialization, core step() function for state transition and reward calculation, and episode reset function.
QLearningAgent Class: Implements the core Q-Learning algorithm, including Q-Table initialization, epsilon-greedy action selection, Bellman equation-based Q-value update, and training data recording for performance evaluation.
GameUI Class: Builds the interactive GUI, including real-time maze rendering, runtime data display, and core operation buttons.
Performance Evaluation Module: Generates training convergence curves and quantitative performance metrics for the assignment report.

2.3 UI Design
The UI follows a concise, intuitive style with three core areas: a 400x400 canvas on the left for real-time maze and agent rendering (with color-coded start point, goal, traps and agent), a runtime information panel on the right to display training episodes, steps, cumulative reward, current action and exploration rate, and a button area at the bottom for core operations (training start, single-step test, game reset, performance plotting). Pop-up prompts are added for key and illegal operations to guide user operation.

Fig 2.1 Maze Treasure Hunt Game UI-1

Fig 2.2 Training

Fig 2.3 Single-Step Test success

Fig 2.4 Plot Performance
3 Implement of Q-learning Algorithm
Q-Learning is a value-based off-policy reinforcement learning algorithm proposed by Watkins in 1989, which learns optimal strategies by iteratively updating the state-action value Q-Table. For this project’s small-scale discrete state/action space, the classic tabular Q-Learning is adopted for its intuitiveness and ease of implementation.
We use the _pos_to_state() function to convert grid coordinates to 0-24 state IDs for Q-Table indexing, with a 25×4 zero-initialized Q-Table to store the expected cumulative reward of each state-action pair. The agent uses the epsilon-greedy strategy to balance exploration and exploitation: it selects random actions for exploration when the random number is below the current epsilon, and selects the action with the maximum Q-value for exploitation otherwise.
Key hyperparameters are set as follows: learning rate alpha=0.1, discount factor gamma=0.95, initial epsilon=1.0, minimum epsilon=0.01, epsilon decay coefficient=0.995, total training episodes=1000. The core Q-value update follows the Bellman optimal equation: 


Fig 3.1 the Bellman optimal equation
Where [s] is the current state,[a] is the executed action, [r] is the immediate reward, [s'] is the next state, alpha is the learning rate, and gamma is the discount factor.
4 Challenges and Solutions
During development, we solved three core challenges with targeted optimizations:
First, the invalid action bug of the single-step test function. In the initial version, the agent repeatedly executed the "up" action when testing without training, as the all-zero initial Q-Table caused np.argmax to always select the first action, with no training status verification. We fixed this by adding an is_trained flag to block unqualified test operations, and an is_test parameter to enforce a pure greedy strategy during testing.
Second, slow model convergence caused by unreasonable reward function design. The initial version only set goal rewards and trap penalties, leading to long-term random movement of the agent. We redesigned a four-level hierarchical reward mechanism: +100 for reaching the goal, -50 for stepping into traps, -10 for out-of-bounds movement, and -1 for each normal step, which significantly accelerated model convergence.
Third, syntax compatibility issues between Google Colab and local PyCharm. The Colab-specific %matplotlib inline magic command caused syntax errors in PyCharm. We solved this by adding environment detection logic, which only executes the command via exec() when the Colab environment is detected, achieving dual compatibility. 


Fig 4.1 Reward function

5 Conclusion
During 1000 episodes of training, the model’s performance improved continuously: the agent’s average cumulative reward rose from below 0 in the first 100 episodes to a stable 80+ after 400 episodes, with the exploration rate decaying to the minimum 0.01. After full training, the agent can reach the goal in 8 steps (the theoretical optimal minimum) with a 100% success rate, completely avoiding all traps.
This project successfully implements a Q-Learning-based maze treasure hunt game, demonstrating the application of Q-Learning in game and pathfinding scenarios, and providing an intuitive platform for understanding core reinforcement learning logic. The trained model shows excellent and stable performance in actual tests, and the interactive interface fully meets the assignment requirements.
6 Reference
[1] Sutton, R. S., & Barto, A. G. (2018). Reinforcement Learning: An Introduction (2nd ed.). The MIT Press. ISBN: 9780262039248. Official book page: https://mitpress.mit.edu/books/reinforcement-learning-second-edition
[2] Watkins, C. J. C. H. (1989). Learning from delayed rewards (Doctoral dissertation). University of Cambridge, King's College. https://www.researchgate.net/publication/3473299_Learning_from_Delayed_Rewards
[3] Python Software Foundation. (2024). tkinter — Python interface to Tcl/Tk. Python 3.12 Official Documentation. https://docs.python.org/3/library/tkinter.html
[4] Harris, C. R., Millman, K. J., van der Walt, S. J., Gommers, R., Virtanen, P., Cournapeau, D., ... & Oliphant, T. E. (2020). Array programming with NumPy. Nature, 585(7825), 357-362. DOI: 10.1038/s41586-020-2649-2. https://www.nature.com/articles/s41586-020-2649-2
[5] Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. Computing in Science & Engineering, 9(3), 90-95. DOI: 10.1109/MCSE.2007.55. https://ieeexplore.ieee.org/document/4160265
[6] Scikit-learn Developers. (2024). User Guide: Reinforcement Learning. scikit-learn 1.4 Official Documentation. https://scikit-learn.org/stable/modules/linear_model.html
[7] Tokic, M. (2010). Adaptive ε-greedy exploration in reinforcement learning based on value differences. In Annual Conference on Artificial Intelligence (pp. 203-210). Springer, Berlin, Heidelberg. DOI: 10.1007/978-3-642-16111-7_19. https://link.springer.com/chapter/10.1007/978-3-642-16111-7_19
[8] Ng, A. Y., Harada, D., & Russell, S. (1999). Policy invariance under reward transformations: Theory and application to reward shaping. In Proceedings of the 16th International Conference on Machine Learning (pp. 278-287). Morgan Kaufmann. https://dl.acm.org/doi/10.5555/645528.657613
[9] Lapan, M. (2020). Deep Reinforcement Learning Hands-On (2nd ed.). Packt Publishing. ISBN: 9781838826993. GitHub code repository: https://github.com/PacktPublishing/Deep-Reinforcement-Learning-Hands-On-Second-Edition
[10] Bansal, R. (2022). Q-Learning for Grid World Navigation. GitHub Open Source Project. https://github.com/ritvikb99/Q-learning-Gridworld
