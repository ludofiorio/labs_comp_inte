## Ludovico Fiorio s306058 report
I completed all the labs and reviews on time 
### Halloween challenge October 31
- to solve this challenge I've implemented a simple tweak that considers if we covered the set or not and so it acts accordingly -> if the set is already covered I remove a set if the set is not covered I add one.
- the strategy is an hill climber
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
- I've implemented 4 more strategies to play Nim.
- Alberto Ricatto and me optimized the optimal strategy, this version of optimal was communicated to the teacher assistant with an email
````
def nim_sum(state: Nim) -> int:
    counter = 0
    for c in state.rows:
        if c == 1:
            counter = counter + 1
        elif c != 0:
            counter= False
            break
    if counter != False and counter%2 == 1:
        return -1
    tmp = np.array([tuple(int(x) for x in f"{c:032b}") for c in state.rows])
    xor = tmp.sum(axis=0) % 2
    return int("".join(str(_) for _ in xor), base=2)


def analize(raw: Nim) -> dict:
    cooked = dict()
    cooked["possible_moves"] = dict()
    for ply in (Nimply(r, o) for r, c in enumerate(raw.rows) for o in range(1, c + 1)):
        tmp = deepcopy(raw)
        tmp.nimming(ply)
        cooked["possible_moves"][ply] = nim_sum(tmp)
    return cooked


def optimal(state: Nim) -> Nimply:
    analysis = analize(state)
    logging.debug(f"analysis:\n{pformat(analysis)}")
    spicy_moves = [ply for ply, ns in analysis["possible_moves"].items() if ns == -1]
    if not spicy_moves:
        spicy_moves = [ply for ply, ns in analysis["possible_moves"].items() if ns == 0]
        if not spicy_moves:
            spicy_moves = list(analysis["possible_moves"].keys())
    ply = random.choice(spicy_moves)
    return ply
 ````
- the fitness function is simply playing 100 matches (50 playing first and 50 playing second) after less than 50 games the code is able to spot the optimal strategy among the other fives. That means that at the end the probabilty to pick a strategy other than the perfect one is zero while at the beginning all had the same probabilty.
 ````
 {<function pure_random at 0x0000023CFCDB89A0>: 0.0,
 <function eliminate_one_row at 0x0000023CFCDB8D60>: 0.0,
 <function eliminate_two_row at 0x0000023CFCDB9620>: 0.0,
 <function leave_one_elem_row at 0x0000023CFCDB9E40>: 0.0,
 <function optimal at 0x0000023CFD26A3E0>: 1.4}
 ````
- I tried to adjust the tweaking function to have smaller variations on time, but this doesn't seem to work so I commented the code
### LAB 9 December 3 (final commit)
- to solve the problem I've implemented two strategies:
   - the first one is a classical GA with a lot of try, I've implemented a set of crossover functions, a tournament function, and a mutation function. I can solve the problem for the instances one and two but it takes a lot of epoches
   - the second one is a ES strategy with a sigma both used to change the number of loci change at every mutation and the probabilty to change the value in one or zero.This strategy works perfectly in few epoches.
- In both cases I stop when I reach fitness==1 to have less fitness calls
### LAB 10 December 19 
- I've implemented a clever player to play tic tac toe and I managed with a montecarlo strategy to beat it. This strategy uses a dictionary with key state+move and a value to understand if that move is good given the state. With the help of this dictionary we have 85% winnig percentage against a clever opponent.
- It's important in my opinion to have a greater reward for a final win respect to a final lost
   ````
  def montcarlo_player(state: State):
    """this function chooses based on the dict build before during learning """
    state = deepcopy(state)
    optimal_value = -float('inf')  # smallest float in python
    optimal_move = 10  # just initializing the variable
    avaibles = set(range(1, 9 + 1)) - state.x - state.o
    for option in avaibles:
        hashable_state = (frozenset(state.x), frozenset(state.o), option)
        if value_dictionary[hashable_state] > optimal_value:
            optimal_value = value_dictionary[hashable_state]
            optimal_move = option
    state.x.add(optimal_move)
    return state
    ````
   this is the clever player I train with and then I play against.
    ````
    def clever_game():
    """play a game using the clever strategy"""
    trajectory = list()
    state = State(set(), set())
    available = set(range(1, 9 + 1))
    while available:
        new_state = clever_player(deepcopy(state), player=0)
        x = new_state.x - state.x
        x = x.pop()
        trajectory.append(StateMove(deepcopy(state), x))
        state.x.add(x)
        available.remove(x)
        if win(state.x) or not available:
            trajectory.append(StateMove(deepcopy(state), 100))#100 to signal that there is no move
            break
        new_state = clever_player(deepcopy(state), player=1)
        o = new_state.o - state.o
        o = o.pop()
        state.o.add(o)
        available.remove(o)
        if win(state.o):
            break
    return trajectory
    ````
## Reviews
### Nov 23-24 lab 2 
#### review Diegomille99 
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
#### review Francesco1102
- What I Liked About the Code:
  - The code functions very well, and it is exceptionally clear with numerous helpful comments.
  - I particularly enjoyed the implemented strategies and the use of prints to display results, especially towards the end.
  - The (ES) part seems theoretically flawless to me, and the comments guiding the switch to (μ+λ) are quite clever.
- Areas for Improvement, in My Opinion:
  - While the code works, the optimal strategy isn't truly optimal. Testing against a perfect winning strategy could assess whether the solution converges to it or not.
  - The fitness function consistently employs Nim(5); experimenting with games of different sizes might provide additional accuracy.
  - I couldn't grasp the rationale behind using different weights against different strategies. While I understand the desire to train more against the optimal strategy, I would suggest either incorporating your rules into the opponents' strategies or, alternatively, removing the purely random strategy also to save time.

 Overall I really liked your work and I wish you good luck for the future labs
 ### Dec 07-08 lab 9
 #### review lorecalo99
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
#### review RaffaeleViola
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
#### review jackcauda00 
Hi,
- What I appreciate about your code:
   - The initial agent is a comprehensive and well-executed solution for playing tic-tac-toe using reinforcement learning, yielding impressive results.
   - The second agent represents a more intricate approach, involving a recursive (full game). It explores all possible outcomes after the first three moves, determining the goodness or badness of a state+action pair. While effective, it may be somewhat time-consuming.
   - The comments are clear, and the final results are presented very well.
- Areas for improvement:
   - There are duplicate definitions of the agent function in the last and third-from-last notebook cells. Removing one of these redundant definitions would enhance clarity.
   - Instead of consistently playing against a random opponent, consider incorporating a more sophisticated adversary, such as a player capable of recognizing when it's about to lose and responding accordingly.
   - Adjust the rewards system: currently, if a state has two moves leading to a loss and one move resulting in a win, it is considered a bad state (-1-1+1=-1 overall). A more effective approach would be to assign a higher reward for winning, such as 10 (-1-1+10=8), to have alse a faster convergence.
#### review FedeBucce
hi,
- what I appreciate about your code:
   - the code is very well documented; almost every line has a comment, making the code very understandable.
   - a very clever idea is sorting the state before creating the key for a dictionary/list. Otherwise, the same state reached with two different trajectories is not treated the same.
   - the minmax function is very sound and well done with two caches to save time.
- areas of improvement:
  - the final results are high but not optimal against a completely random opponent. In my opinion, you should try using different rewards, like 10 for winning and -1 for losing.
constants should be in capital letters.
   - another nice idea may be playing and training against a more clever opponent. For example, an opponent that checks if it has a winning move or if the other player has a winning move and acts accordingly.
### Quixo Report
I worked at this project with Umberto Fontanazza and Alberto Ricatto.
The core of the project is using a min_max function with alfa beta pruning. At the beginnig the idea was to use advisors, each advisor analysing the board was giving a value between 0-100 and we used backpropagation to adapt the weights connected to each advisor.
Here I show an example of advisor
 ````
 def compact_board_version2(board: Board, player: PlayerID) -> float:
    """counts the O close to others O and the X close to others X and returns a score
        only takes into consideration the positions inside the board not the perimeter"""
    count_x, count_o = 0, 0
    arr = board.ndarray
    for pos in CENTER:
        player_int = arr[pos]
        if player_int not in (0, 1):
            continue
        #adjacents = [(pos[0]-1,pos[1]-1),(pos[0]-1,pos[1]),(pos[0]-1,pos[1]+1),(pos[0]+1,pos[1]-1),(pos[0]+1,pos[1]),(pos[0]+1,pos[1]+1),(pos[0],pos[1]-1),(pos[0],pos[1]+1)]
        adjacents = [(pos[0] + i, pos[1] + j) for i in range(-1, 2) for j in range(-1, 2) if i != 0 or j != 0]
        for adjacent in adjacents:
            if arr[adjacent] == player_int:
                if player_int == 1:
                    count_x += 1
                else:
                    count_o += 1
    return __rule_advantage(count_o, count_x, player)
 ````
At the end we decided to switch to a faster but equally powerfull strategy. Instead than using advisor we compute stats based on the board. Here's an example of the simplest rule that considers what player has more pieces on the corners:
 ````
 for corner in CORNERS:
            if self[corner] == 0:
                o += 1
            elif self[corner] == 1:
                x += 1
        self.corner_control = (o, x, 4)
 ````
There are two oracle files (the file where a valuation of the board is done) one for the advisor and one for the board stats, the board.py file takes into consideration if there are any symmetries of the board.
 ````
  def filter_out_symmetrics(positions: Iterable[Position], axes: Iterable[Symmetry]) -> set[Position]:
        filtered_positions: set[Position] = set()
        for position in positions:
            symmetrics = position.symmetrics(axes)
            if any(symmetric in filtered_positions for symmetric in symmetrics):
                continue # symmetric already present
            filtered_positions.add(position)
        return filtered_positions
 ````
Our project is winnig with min_max depth 2,3 against a random or clever opponent. It wins also against a human player with depth=4.
