# Pacman Q learning - Yuval Saraf
### Requirements
1. Implement q-learning - fill in missing parts the qlearningAgents.py. (40
pts)
    * I recommend using python’s `deafultdict(float)` for the Q learning table. It lets you access any (state,action) key and it initializes any unseen state-action pair to zero instead of an error.
    * When there is a tie in the greedy action break it randomly.

    **Done, check code.**
2. Run the small grid pacman to see the first 10 epochs by running:  
    ```bash
    python pacman.py -n 10 -l smallGrid -a numTraining=10
    ```
    (-n sets the number of epochs, -a agent options)

    **Here is the result after running the command in terminal:**  
    ```bash
    Beginning 10 episodes of Training
    Pacman died! Score: -512
    Pacman died! Score: -509
    Pacman died! Score: -516
    Pacman died! Score: -504
    Pacman died! Score: -509
    Pacman died! Score: -505
    Pacman died! Score: -510
    Pacman died! Score: -504
    Pacman died! Score: -514
    Pacman died! Score: -517
    Training Done (turning off epsilon and alpha)
    ---------------------------------------------
    ('Average Score:', -510.0)
    ('Scores:       ', '-512.0, -509.0, -516.0, -504.0, -509.0, -505.0, -510.0, -504.0, -514.0, -517.0')
    Win Rate:      0/10 (0.00)
    ('Record:       ', 'Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss, Loss')
    ```
3.
    Test your agent by training 3000 epochs and testing on another 10 by running:
    ```bash
    python pacman.py -x 3000 -n 3010 -l smallGrid
    ```
    (-x sets the number of training epochs).  
    Make sure you win 100% (10 pts)


    **Here is the result after running the command in terminal:**  
    ```bash
    ...
    Reinforcement Learning Status:
        Completed 3000 out of 3000 training episodes
        Average Rewards over all training: 30.05
        Average Rewards for last 100 episodes: 125.61
        Episode took 1.02 seconds
    Training Done (turning off epsilon and alpha)
    ---------------------------------------------
    Pacman emerges victorious! Score: 499
    Pacman emerges victorious! Score: 495
    Pacman emerges victorious! Score: 503
    Pacman emerges victorious! Score: 495
    Pacman emerges victorious! Score: 499
    Pacman emerges victorious! Score: 503
    Pacman emerges victorious! Score: 499
    Pacman emerges victorious! Score: 495
    Pacman emerges victorious! Score: 503
    Pacman emerges victorious! Score: 503
    ('Average Score:', 499.4)
    ('Scores:       ', '499.0, 495.0, 503.0, 495.0, 499.0, 503.0, 499.0, 495.0, 503.0, 503.0')
    Win Rate:      10/10 (1.00)
    ('Record:       ', 'Win, Win, Win, Win, Win, Win, Win, Win, Win, Win')
    ```
4. Modify the runGames function in pacman.py to to save a plot of the reward during training. (20 pts).  
    
    **Added CLI parameter to plot given number of games, for example**
    ```bash
    python pacman.py -x 3000 -n 3010 -l smallGrid -p 3000
    ```
    ![](images/2020-11-16-16-06-51.png)
    * The defualt agent parameters are epsilon=0.05,alpha=0.2,gamma=0.8.  
    Keep alpha and gamma fixed and run with these epsilons 0.,0.1,1.  
    Show average test results over 50 runs and the training plots (run `python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=xxx,alpha=yyy,gamma=zzz`) (-q disables the GUI)

    **Secondly, needed to add averaging function to the plot (jumps so much becuase showing reward per game, and is a huge difference between winning and losing).  
    So I did just that, for example**
    ```bash
    python pacman.py -x 3000 -n 3010 -l smallGrid -p 3000 --numToAverageInPlot 50
    ```
    ![](2020-11-16-16-21-03.png)
    **Now simply ran that for different epsilons as requested**
        * `epsilon=0`  
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=0,alpha=0.2,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-15-23.png)
        * `epsilon=0.1`
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=0.1,alpha=0.2,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-17-22.png)
            **The spike for the last batch is because eplison is zeroed for those runs.**
        * `epsilon=1`
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=1,alpha=0.2,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-18-10.png)
    * Keep epsilon and gamma fixed and run with these alphas 0.1,0.2,0.3 .  
    Show average test results over 50 runs and the training plots

        **Same as before just foir different alphas**
        * `alpha=0.1`
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=0.05,alpha=0.1,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-23-50.png)
        * `alpha=0.2`
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=0.05,alpha=0.2,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-17-22.png)
        * `alpha=0.3`
            ```bash
            python pacman.py -x 3000 -n 3050 -l smallGrid -q -a epsilon=0.05,alpha=0.3,gamma=0.8 -p 3050 --numToAverageInPlot 50
            ```
            ![](2020-11-16-18-24-59.png)
5. The agent with no exploration (epsilon=0) works well, why is that?  
    (hint: think of the values of the rewards in the initial epochs) (15 pts)

    **The reason for no exploration quite works is because in the first runs, with `epsilon=0`, the next step will always be the most beneficial, and losing the game is just such a huge lose in reward points.  In the first few times the next step is not determined, and will choose the next action randomly (`random.choice`).  
    But as training progresses, survival is a bit more beneficial then losing right away, so the algorithm chooses the path with most survival rate, and later discovers that it's also the path for winning, and shifts it's focus to winning instead of surviving.**

6. We will run on medium size pacman for 20000 epochs, run `python pacman.py -x 20000 -n 20050`. How are the results?

    **I ran the code with plot parameters, so more data will be available to show**

7. Modify the agent to keep a count of how many times he visited every state.  
    Rerun the previous agent and answer what percentage of states was visited once?  
    less then 5 time? Does this explain the results on this MDP?  
    How would you change the agent to handle this MDP? (25 pts)

    **Now I added to the agent a dict keeping track of how many states were visited (on `update` method).
    I also added another CLI parameter to plot that histogram to answer and requested questions.
    Here is the graph**
