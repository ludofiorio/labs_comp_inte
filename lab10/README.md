LAB10

I start my implementation with the code provide in class by the professor on the 12/14.

I added a clever strategy that checks if a three symbols in a line is about to be compleate and it acts accordingly. The dictionary has as key both the state and the next move and as the value has the evaluation of the state plus the move that we are going to play.

I changed the reward for winning beacause a state with a winnig move and a losing move is very favorable if you are good enough to play the right move.

I train using the clever strategy and than I play 1000 games (always starting), the result are 850 wins against 120 losses 
