# Jaylen + Ryan OH
- each ball is represented with a different thread
- ```board.cc, board.hh, breakout61.cc``` are important files
- ```board.hh``` has definitions
- pong balls = x,y location; dx,dy direction
- pong warp = object that can be located in shell, if ball hits it, the ball enters the warp and comes out of a different end
- board = made of cells
- add mutex, conditional variable into different objects
- different cells and balls accessed concurrently --> What objects can be accessed at the same time by many threads (e.g. the board, individual cells if there is a collision)?
- coarse-grained = locking the entire board, so one thread can access the board one at a time
- fine-grained = locking specific areas of the board, so that multiple threads can access the same area of the board at the same time

Start by locking entire board.
- use one lock
- try to eliminate data races
- look at ```move``` function, which will have data races
- how to syncrhonize
- thinking of warps
- first three functions in ```breakout61.cc``` are important (first one is running ball thread)

std::library stuff
- mutex
- unique_lock
- scoped_lock
- conditional_variable_any
- atomic for single ints that are simply incremented

Goal: want to block in pset, not polling
