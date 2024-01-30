## Ludovico Fiorio s306058 report
### Halloween challenge October 31
- to solve this challenge I've implemented a simple tweak that considers if we covered the set or not and so it acts accordingly
- this are the final results:
 ````
(0, 0)
(3483, -1)
(4534, -2)
(4853, -3)
(4951, -4)
(4986, -5)
(4995, -6)
(4998, -7)
(4999, -8)
(5000, -9)
(5000, -8)
number of fitness 161

 ````

### LAB 2 November 13
- I've implemented 4 more strategies to play Nim and I've optimized the optimal strategy, this last solution was communicated to the teacher assistant with an email
- the fitness function is simply playing 100 matches (50 playing first and 50 playing second) after less than 50 games the code is able to spot the optimal strategy among the other fives
### LAB 9 December 3 (final commit)
-TODO
### LAB 10 December 19 
-TODO
## Issues
### Nov 23-24 lab 2 
#### review of Diegomille99 
- Areas for Improvement:
  - The "optimal" function is redundantly written at both line 28 and line 70. This redundancy might lead to compiling problems, similar to what occurs at line 68. It seems likely that line 68 
 was inadvertently overlooked
  - The code lacks result reporting. I recommend incorporating print statements to display results. Additionally, consider adding comments within the code, not solely in the markdown file.
  - The "embed-nim" function returns an unused value, and "strategy2" is defined but not utilized afterward.
  - When the tournament is played, I suggest periodically changing the starting player. Also, consider varying the size of the nim game for a deeper evaluation.
  - Constant values should be in capital letters also for better readability.
- Positive Aspects:
  - The code demonstrates a robust mathematical foundation, particularly in the application of the Dirichlet distribution.
  - The pick strategy is well-implemented and concise.
  - The sections involving mutation, crossover, and slicing appear to be effective and well-implemented.
#### review of Francesco1102
- What I Liked About the Code:
  - The code functions very well, and it is exceptionally clear with numerous helpful comments.
  - I particularly enjoyed the implemented strategies and the use of prints to display results, especially towards the end.
  - The (ES) part seems theoretically flawless to me, and the comments guiding the switch to (μ+λ) are quite clever.
- Areas for Improvement, in My Opinion:
  - While the code works, the optimal strategy isn't truly optimal. Testing against a perfect winning strategy could assess whether the solution converges to it or not.
  - The fitness function consistently employs Nim(5); experimenting with games of different sizes might provide additional accuracy.
  - I couldn't grasp the rationale behind using different weights against different strategies. While I understand the desire to train more against the optimal strategy, I would suggest either incorporating your rules into the opponents' strategies or, alternatively, removing the purely random strategy also to save time.

 Overall I really liked your work and I wish you good luck for the future labs
 ### Nov 07-08 lab 9
 #### review of lorecalo99
 - What I liked:
   - Your code is well-documented and clear.
   - I appreciate the way you initially boost performance by dividing the population into champions.
- Areas for improvement:
  - Consider calculating fitness as soon as a new individual is created to avoid cycling through offspring each time.
  - It might be beneficial to define population class.
  - Explore the option of having more bits changed in each mutation. Additionally, consider reducing the tournament size; while 5 is
acceptable, 2 or 3 might be more optimal.
  - In Box 6, constants should be in capital letters.
  - Explore the possibility of pipelining crossover and mutation.
  - Address code redundancy. You simply change the mutation or crossover functions in the improvements there is no need to rewrite all
the code every time.
  - Your code should check if fitness one is reached to stop calculating fitness. The main reason you never reached fitness one is because
you never considered a different rate of changing 1->0 and vice versa
#### review of RaffaeleViola
Hello,
- Positive aspects:
  - The README file and comments are well-crafted. Note that there is a slight mix-up between "fitness" and "calls" in the final results section.
  - Effective use of asserts to ensure code reliability.
   - The island approach is intriguing and appears theoretically sound.
- Areas for improvement:
  - Consider mutating more than one bit at a time, especially since a genome of 1000 bits may not see significant impact with only a single bit mutation.
   - To enhance clarity, reduce the number of print statements. Include information about the specific instance [1, 2, 510] of the problem being addressed at each step.
  - Implement periodic checks for fitness == 1 to minimize unnecessary calls to the fitness function.
   - Think about the use of a population other than an individual class.
### Dec 26-27 lab 10
#### review of jackcauda00 
Hi,
- What I appreciate about your code:
   - The initial agent is a comprehensive and well-executed solution for playing tic-tac-toe using reinforcement learning, yielding impressive results.
   - The second agent represents a more intricate approach, involving a recursive (full game). It explores all possible outcomes after the first three moves, determining the goodness or badness of a state+action pair. While effective, it may be somewhat time-consuming.
   - The comments are clear, and the final results are presented very well.
- Areas for improvement:
   - There are duplicate definitions of the agent function in the last and third-from-last notebook cells. Removing one of these redundant definitions would enhance clarity.
   - Instead of consistently playing against a random opponent, consider incorporating a more sophisticated adversary, such as a player capable of recognizing when it's about to lose and responding accordingly.
   - Adjust the rewards system: currently, if a state has two moves leading to a loss and one move resulting in a win, it is considered a bad state (-1-1+1=-1 overall). A more effective approach would be to assign a higher reward for winning, such as 10 (-1-1+10=8), to have alse a faster convergence.
#### review of FedeBucce
hi,
- what I appreciate about your code:
   - the code is very well documented; almost every line has a comment, making the code very understandable.
   - a very clever idea is sorting the state before creating the key for a dictionary/list. Otherwise, the same state reached with two different trajectories is not treated the same.
   - the minmax function is very sound and well done with two caches to save time.
- areas of improvement:
  - the final results are high but not optimal against a completely random opponent. In my opinion, you should try using different rewards, like 10 for winning and -1 for losing.
constants should be in capital letters.
   - another nice idea may be playing and training against a more clever opponent. For example, an opponent that checks if it has a winning move or if the other player has a winning move and acts accordingly.
