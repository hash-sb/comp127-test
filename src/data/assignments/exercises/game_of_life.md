---
repo: https://github.com/mac-comp127/game-of-life
---

# Game of Life

{:standard_toc}

This take-home exercise gives you more practice with expressions and conditionals — and with the ever-practical skill of reading a large existing codebase to make a small change.


## The Game of Life

The Game of Life is a mathematical “zero-player” game devised by John Conway. It simulates the behavior of populations following simple rules.  Life is played on a grid of cells. Each cell can be either alive or dead.  The game progresses in a series of iterations, called generations.  In each generation, the value of a cell (whether it is alive or dead) is determined by the value of that cell and its neighbors in the previous generation using the following rules:

- If a living cell has less than 2 living neighbors, it will die (of loneliness).
- If a living cell has more than 3 living neighbors, it will die (it’s overcrowded).
- If a dead cell has exactly 3 living neighbors, it will come to life (it is born).
- Otherwise, the cell’s value will not be changed.

This small set of rules gives rise to shockingly rich and complex results. For more information on Life, the [Wikipedia article on the topic](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) is quite informative, as is [Martin Gardner's original article](http://ddi.cs.uni-potsdam.de/HyFISCH/Produzieren/lis_projekt/proj_gamelife/ConwayScientificAmerican.htm).


## Learning goals

- **Reading and understanding code**: one major feature of this assignment is that you will *write* very little code, but you will need to *read* quite a bit of existing code and *think* about it. Moreover, you won't need to completely understand all of the existing code in order to make your changes. This kind of task -- making a small change to a large existing codebase[^brownfield] -- is very common in software development and is an important skill to develop.

- **Unit tests**: you will learn more about unit tests, how they work, and why we write them.

- **Modify, tinker, and experiment with code**: you will practice modifying and experimenting with someone else’s code. The aim is for you to develop some autonomy and a spirit of having fun and doing bold experiments.

- **Simple pieces and complex behavior**: you will see how simple rules in software can create complex and fascinating results---this is an example of the more general notion of "[emergence](https://en.wikipedia.org/wiki/Emergence)" or "emergent complexity". The aim is to inspire you in future experiments of your own.

[^brownfield]: Changing an existing codebase in a way that preserves most of its behavior, structure, and so on, is sometimes called "[brownfield development](https://en.wikipedia.org/wiki/Brownfield_(software_development))" in analogy to building construction on a site with existing development or pollution that needs to be dealt with.

    On the other hand, writing code from scratch, starting from a blank slate, is called "greenfield development", which uses the classical notion in capitalism that land that humans have not altered yet -- it's still covered in green plants -- is "unimproved"; that there is, in some sense, nothing there yet. (Even though there obviously is something there, and that the land does provide value, in the form of environmental services and otherwise, that is not accounted for by traditional capitalist markets and pricing mechanisms...)



## Overview

You start this lab with an existing project, which provides a nice user interface for the Game of Life — but the rules are missing!

You will have the following tasks:

- Implement Conway’s rules.
- Create a new rule set implementing a simple variation on Conway’s rules.

and some hopefully-fun optional tasks related to:

- Altering the behavior of the graphical board.
- Altering the underlying rules for the Game of Life


## Running the tests

This project includes **unit tests**. They are code that tests the correctness of other code. In particular, these verify that the Game of Life code is working correctly.

Look for `BoardTest` and `ConwayTest`, and take a minute to understand their stucture. You will see many individual test cases, each of which has a structure like: “When I do _X_, the result should be _Y_.”

Run the tests by pressing command-shift-P (macOS) or ctrl-shift-P (Window), and searching for the command “Java: Run All Tests.” You should see 4 tests failing. That is because the rules are not implemented yet!

This is a common and effective approach to development: write some tests that describe what you intend, then write the code that will make those tests pass.


## Implementing Conway’s Rules

Your first task is to implement Conway’s rules for the Game of Life. These are the rules described above in the game description.

`Conway.java` contains a method, `applyRules()`, which will be used by the game board to compute the next state in each generation. `applyRules()` takes two parameters: a boolean value representing the cell’s current value, and an integer representing the number of living neighbors the cell has. It must return the value for that cell in the next generation. true represents a living cell, and false represents a dead cell.

When you think you’ve got it, try running the tests again. If they all pass, you can see your rules come to life!


## Experimenting

Once you have the rules implemented and the test cases pass, play with the game by right opening the `MainWindow` class, finding its `main` method, and clicking the “Run” link just above it.

It will present you with a game grid and some controls. Create an initial state by clicking various cells. clicking a cell toggles whether it is alive or dead. Then use the ‘Step’ button to run one generation, or the ‘Run’ button to start the simulation performing four generations per second. See if you can create simple shapes that move continuously. Try setting lots of cells randomly and see what happens.


## HighLife

Your next task is to implement a variant of Conway’s rules, called HighLife. HighLife is exactly like Conway’s game, except there is one additional rule: any dead cell with 6 living neighbors comes to life (this is in addition to the other rules, so cells with 3 neighbors still come to life).

Copy the `Conway.java` and `ConwayTest.java` files, creating `HighLife.java` and `HighLifeTest.java` files. Also change `HighLifeTest` so that the `setUp()` method uses `HighLife` instead of `Conway`.

Next, go through the test code in `HighLifeTest.java` and adjust it so that it is checking for correct play by the HighLife rules rather than the Conway rules.

Add a test method to check that 6 living neighbors bring a cell to life, and modify the other tests as needed to reflect the change in rules.  Once you have the tests implemented, change HighLife to implement the new rules and make the tests pass.

Change HighLife’s `getName()` method to return the string `"HighLife"`.

Finally, once you have the rules implemented, open the `MainWindow.java` file. In this file, there is a block of code that has been placed in a comment with a note “Uncomment for HighLife”. Remove the comment symbols around this code.

When you run `MainWindow`, you will now be able to select rule sets.  Have some fun with HighLife.

## Optional: Make up your own rules

Now you have implemented two rule sets for the Game of Life. But for
both, we prescribed the exact rules; now, *you* get to experiment and
play with the rules.

Following the steps you used to add the HighLife rules, add another rule
set. You can name it whatever you like; perhaps just `<yourname>Life`.
(Or, if you're getting stuck, just modify the HighLife class and
associated tests.)

Now make up rules. Your rules can be *anything*. The `applyRules` method
takes two parameters, and you can implement absolutely any kind of logic
you want based on those. You can even do other things, such as having
the output depend on the time, or be random, or ignore the parameters
entirely, or...anything.

Your goal here: find something interesting! If you have something
interesting, share it with a neighbor, the preceptors, and your
instructor. Maybe even the entire class: share your rule set. What are
your rules? What did you try while coming up with them?? How did you
decide on those rules? What makes them interesting?

<callout>
💡 *This is what makes programming fun!* You can do anything! You don't have to follow the rules -- not Conway's rules for the Game of Life, not the rules for this activity imposed on you by your instructors. You are bound by the physical limits of the computer and the semantics of the Java programming language[^1], and beyond that, only your own imagination. **Be bold! Have fun! Find beautiful things!**
</callout>

[^1]: Regarding "the limits of the semantics of Java" as a programming language, you may later learn about [Turing completeness](https://en.wikipedia.org/wiki/Turing_completeness) and discover that, in some sense, *there aren't any limits whatsoever*. Anything that can be computed can, in principle, be computed by Java. (But one has to say exactly what is meant by "computing"...)


## Optional: Change the Speed

Faster is funner! Right now, the simulation only runs four generations per second when you click “Run.” Can you find the spot in the code that sets the speed?

As a programmer, you often have to make a change to a program that is unfamiliar, and possibly too large for you to fully understand in the time available. There is a lot of code in this project. See if you can find where the speed is set, without having to understand everything.


## Optional: Add Some Color

If you've made it this far, congratulations!  As an extra challenge, try to improve the visualization of the game board by coloring the live cells according to how many neighbors are currently alive.  Some hints:

- Take a look at `paintComponent()` in the LifeComponent class.
- What color are the alive cells painted right now?
- Where is that color specified in the `paintComponent()` method?
- Does the board have a method that can help you in calculating the number of live neighbors?


## Last Step: Don’t Forget!

Once you have finished:

- Check your code against the Rubric: Game of Life
- Answer the questions in the self evaluation in the `SELF_EVALUATION.md` file.
- After doing that, commit and push your code!

See the [Take-Home Exercise Procedure](https://comp127.innig.net/resources/take-home-procedure/#self-evaluation) for full instructions.


## Acknowledgments

This lab was originally developed by [Michael Ekstrand](https://www.boisestate.edu/coen-cs/people/faculty/michael-d-ekstrand/) and is used with his permission. Shilad Sen brought it to Macalester and adapted it for Comp 127. Paul Cantrell rewrote the instructions for GitHub and added a bit of flair.

Thanks to everybody who contributed!

----
