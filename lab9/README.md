I tried two different approches:

GA works only with problem size small like 1 or 2 and it needs a lot of fitness calls around 6000. I you want to try the code you should set the EPOCH constant to 1000.

ES works in all the cases and with few hundreds fitness calls. The main different in my opinions are changing more bits per time and also working with the probability to go from 0->1 different than 1->0