# The Rules of Mastermind

At the beginning of every game of Mastermind, the computer chooses a secret “code” composed of 4 colors. Each color is either “red”, “orange”, “yellow”, “green”, “blue” or “purple”. This code is hidden from the player; the object of the game is to guess the code. The player has 10 rounds to guess the code. On each round the player can guess a code (another 4 color sequence). After each guess, the player receives feedback in the form of some number of black and white marks. A black mark is awarded for each position of the guess that matches the code. A white mark is awarded for positions where the player guessed the correct color, but placed it in the wrong position. At most there will be 4 marks, one for each position of the code. For example, if the code is:

        Red         Green        Green        Blue

…and the guess is:

        Green        Green        Purple        Blue

…the response would be two black marks (for the second green and blue) and a white mark (for the first green, which is the correct color, but in the wrong place). The player wins if they correctly guess the code within ten rounds; the player loses if they cannot guess within 10 rounds.

**How would you model this game using Java?**  Come up with an object model — a design for classes, methods, properties, and relationships — that could describe the rules of the game.

Please note that you **do not need to implement the game in code**. Your job is to design the classes that one could use to implement the game, but not to actually code them.

Please note that you are only trying to model the **abstract state of the game**. You do _not_ need to describe classes that implement the user interface.

Some constraints for your model:

- In order to think about their next guess, the player should be able to look over all of their previous guesses during this game and the responses to them.
- It should allow the player to play multiple games of Mastermind in one sitting.
- It should keep track of how many times the player has won and lost.
- It should allow one to implement a variety of different UIs / visualizations of the game, without any of them having to reimplement the game rules.

As a hint: You should use an iterative process in developing this design that starts by creating the simplest working design for some small part of the game, and iteratively evolves toward the full specification above.

(Hat tip to Daniel Kluver for this exercise.)

P.S. Mastermind is an [actual board game](https://en.wikipedia.org/wiki/Mastermind_(board_game)) from the 1970s:

![](images/image1.png)


