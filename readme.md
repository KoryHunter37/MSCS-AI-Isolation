# MSCS Project

This repository overviews an Artificial Intelligence design project which I completed while completing my Master's in Computer Science at Georgia Tech. The source code for this project is not publically available, in compliance with the Georgia Institute of Technology Academic Honor Code<sup><a href="https://policylibrary.gatech.edu/student-affairs/academic-honor-code">[1]</a></sup>.

Instead, I have substituted my raw code with a guided walkthrough of the project itself, the steps I went through to complete it, and the skills I acquired by solving it.

# AI Isolation

Isolation<sup>[\[2\]](https://en.wikipedia.org/wiki/Isolation_(board_game))</sup> is a two player board game in which players attempt to "isolate" their opponent's piece, which makes it an excellent AI challenge to tackle!

A simple version of the game might look like this:

<p align="center"><img width="182" height="182" src=images/isolation-basic.png></img></p>
<div align="center"><b>Fig 1. Basic Isolation Board</b></div>

* Players each place their tile on a 7x7 grid.
* Take turns moving their tile any number of spaces, in a straight horizontal or diagonal line, but not into a space occupied by another player.
* Spaces which previously contained a player **are no longer accessible.**
* A player wins the game when their opponent can no longer move.


Because the game can be modified easily, numerous AI challenges can be generated. 

For example, here is a game of Isolation played using the "Knight" piece from chess, meaning it can only move in an "L" pattern:

<p align="center"><img width="480" height="318" src=images/isolation-demo.gif></img></p>
<div align="center"><b>Fig 2. Isolation with Knights</b><sup><a href="http://the-charlie.com/ml/index.php/project/winning-at-isolation/">[3]</a></sup></div>

# The Problem

For my project, I was given a specific twist on the classic Isolation formula, which implemented a sumo wrestling themed mechanic! The rules of this "Sumo Isolation" work just like the basic version, only now, players may also "shove" their opponents into an adjacent empty square. A player cannot shove their opponent into a square which has previously held a player, however, they **can** shove their opponents off the edge of the grid. If this happens, the shover wins immediately! This makes edge spaces very dangerous to play around.

I was tasked with designing an AI agent which could most consistently win in this game. My AI agent competed against random selection, as well as increasingly difficult opponent AI agents. I also competed with other students who designed AI solutions of their own.

Here are the series of assigned tasks I completed, in ascending order of difficulty:

| Points    | Condition                                |
| --------- | ---------------------------------------- |
| 5 points | You write an evaluation function, OpenMoveEval, which returns the number of moves that the AI minus the number of moves opponent can make, and your evaluation function performs correctly on some sample boards we provide. |
| 30 points | Your AI defeats a random player >= 90% of the time. |
| 20 points | Your AI defeats an agent with OpenMoveEval function that uses minimax to level 2  >= 65% of the times. |
| 20 points | Your AI defeats an agent with OpenMoveEval function that uses alphabeta to level 4  >= 65% of the times. |
| 20 points | Your AI defeats an agent with OpenMoveEval function that uses iterative deepening and alpha-beta pruning >= 65% of the time. |
| 5 points | Your AI defeats an agent with Noah's secret evaluation function that uses iterative deepening and alpha-beta pruning and optimizes various aspects of the game player >= 85% of the time  |

# Minimax Algorithm

Minimax<sup><a href="https://en.wikipedia.org/wiki/Minimax">[4]</a></sup> is a prediction technique used to maximize the future 'reward' for a player. This 'reward' is ambiguous-- it can be whatever you want it to be, and deciding what components factor into the reward are an integral part of implementing this algorithm. The core idea of this algorithm is understanding that the player will always attempt to maximize reward on their turn, and the opponent will attempt to minimize reward. By considering which moves the opponent is likely to use, based on how they minimize your reward, you can effectively plan ahead and imagine what the best possible route would be in a future where your oppoent plays optimally.

<p align="center"><img width="480" height="318" src=images/minimax.gif></img></p>
<div align="center"><b>Fig 3. Minimax Algorithm</b><sup><a href="https://www.globalsoftwaresupport.com/minimax-algorithm-explained/">[5]</a></sup></div>

In my implementation, I originally began by making the reward value purely equivalent to the number of moves available to the active player. This was a basic approach, but it was enough to definitively make my AI agent stronger than an AI which always returned random moves.

As I continued, I learned that certain "killer moves" were important in my consideration. For example, due to the special rules of Sumo Isolation, there is a condition which can result in an **instant win.** If my piece sees a future where it can gain an instant win by pushing the other player off the board, then I need to increase the reward value of that node.

By trial and error, I began to learn how much I needed to weigh various aspects of the board. I even considered other aspects, such as the total number of spaces that aren't blocked, or even "partitions" (how well the board is divided with a line).

I was able to monitor the results of games played with my AI agent, and adapt it based on how it performed in trial scenarios. I was even able to manually stop the game and take over as a human player, to better think about how I would implement my human brain's analysis of a board state into an algorithmic form.

While these considerations were able to help me overcome the easier AI opponents, I realized I simply was not looking deep enough into the future to successfully beat the strongest opponents. Minimax has an exponential computation cost, which rapidly grows as the depth of the search increases. To overcome this, I implemented Alpha Beta pruning, which removed vast segments of my computation tree.
