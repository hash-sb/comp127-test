---
repo: https://github.com/mac-comp127/bubble-blitz
---

# Bubble Blitz

{:standard_toc}

## Learning goals

This assignment focuses on graphics, object state and behavior, and testing.

In this homework, you will practice:

- Decomposing a problem into classes and methods
- Understanding the relationship between classes, instance variables, methods, and constructors
- Doing coordinate arithmetic and physics simulation
- Creating animations using loops
- Using different testing strategies

Hat tip to Bret Jackson, the original author of this assignment.

## Background Knowledge: Using the `GraphicsGroup` class

The `edu.macalester.graphics` package contains a class called `GraphicsGroup` that is a `GraphicsObject`, and can therefore be placed on a canvas using the `add` method — but is also a container for graphics objects, like a canvas, with its _own_ `add` method. This enables several graphical objects that could be placed together or all moved in the same way to be kept together in one object and all moved at once.

One thing to keep in mind with `GraphicsGroup`s is that they define their own interior coordinate system. For example, if you add a `Rectangle` object to the group at position (10, 10) and then add the group to the `CanvasWindow` at position (50, 50), then your rectangle will actually appear at position (60,60) relative to the upper left corner of the window.

## Your Tasks

Your task in this homework is to create an Angry Birds style game where the user can adjust the angle and velocity of a cannon (the black line) to shoot purple bubbles on the screen:

![screenshot of the bubble blitz game](images/bubbleblitz.png)

Explore the code we have given you. We have already decomposed the problem into the following classes:

- `BubbleBlitz`: contains the main method, creates the game’s `CanvasWindow`, and is responsible for running the program and gathering together all the other objects that make up the game.
- `Cannon`: a `Line` representing the direction the cannon is pointing.
- `CannonBall`: tracks the motion of the cannonball, and displays it on the screen as an `Ellipse`.
- `Bubble`: a `GraphicsGroup` representing a single bubble as a collection of concentric circles. Reponsible for determining if it has been hit by a cannonball.
- `BubbleManager`: keeps track of bubbles and is able to query each bubble to see if it should be popped.
- `VisualCannonTester`: can be used to test that your CannonBall code works correctly.

### Part 0: Cannonballs

You should use an iterative development process to avoid writing a lot of untested code that then breaks when you finally run it. As you work, think: “How can I test what I am writing? How can I organize my work so that I can test it _sooner_?”

Start by working on the `CannonBall` class. Complete the class as follows:

#### State

Each `CannonBall` object must keep track of the following state:

- the ball’s current center position (`x` and `y`),
- the ball’s current velocity (`dx` and `dy`), and
- the maximum x and `y` values (`maxX` and `maxY`) for the boundaries outside of which the ball should stop moving.

<details>
<summary>
Aside: What are the units for dx and dy?
</summary>

As with most video games, they are fairly arbitrary units. We are not doing a real physics simulation here, so we can just play with different numbers until the animation looks good. In game development, most things don’t need to be _accurate_; they need to be _convincing_.
</details>

#### Constructor

`CannonBall` provides an empty constructor. You will need to fill it in.

Use the parameters to initialize the starting state of the class.

<details>
<summary>Hint about the constructor
</summary>

Be mindful of the different between **constructor parameters** and **instance variables**. (Which is which? How is the syntax different for the two? Refer to the [bicycle activity](https://comp127.innig.net/activities/bicycle/) and the [class structure reading](https://comp127.innig.net/readings/class_anatomy/).)

The **constructor parameters** are the bits of information someone must specify to create a `CannonBall`.

The **instance variable** are the state a `CannonBall` always has for as long as it exists.

In this case, there is _not_ a perfect one-to-one correspondence between parameters and instance variables. There is a comment in the code with a hint about computing the initial velocity. (In this context, “initial” means “at the start.”) That comment will help you.
</details>

Initialize `ballShape` by creating an `Ellipse` of the appropriate size.

#### Property accessors

Add getter methods for the center `x` and center `y`.

#### `updatePosition`

This method moves the ball if it is still in bounds, then applies gravity by adjusting the ball’s velocity.

Your method should do this:

- Calculate new `x` and `y` coordinates by adding the `x` and `y` velocities times `dt` to the current coordinates.
- If the new positions are greater than zero and less than the maximum:
  - Move the `CannonBall` to the new position. (Hint: A `CannonBall` has an `Ellipse` named `ballShape`, which will need to move to the new `x` and `y` position so that you can see the cannonball moving on the screen. Look at the methods of `Ellipse` for ideas.)
  - Subtract `GRAVITY` times `dt` from the `y` velocity.
  - Return true, meaning “in bounds.”
- Otherwise, if the ball is out of bounds:
  - Return false, meaning “out of bounds.”

#### Visual testing

Now visually test your results. Open `VisualCannonTester` in the test folder. Uncomment the commented-out code, and edit line 27 to initialize a `CannonBall` object using the value for the variables defined on the previous few lines. Run the code. You should get an animated result that looks like the following when it is complete:

![visual image of the cannonball trajectories](images/cannontrajectories.png)

#### Unit testing

You should also test your code with unit tests. In the test folder, run `CannonBallTest`, uncomment the commented-out code, and make sure that the test passes. (The numbers in the test are already correct.) Add new test methods to check that `updatePosition` returns the expected boolean value, and that it does not change the position if the result would put it outside of the specified bounds.

### Part 1: Bubbles

Look at the `Bubble` class. We have completed the constructor and drawing code for you already. Make sure you understand how the positioning of the Ellipses inside the `GraphicsGroup` works. We will be using `GraphicsGroup` objects in future assignments.

We need to change the `intersects` method. Remember our test-driven development workflow for making changes to code:

1. Run the tests. If they don't pass, then...
2. Change the code.
3. Goto (1) and repeat until the tests pass!

So: run the tests in `BubbleTest`. They will fail.

Complete the `intersects` method. This should return true if the cannonball parameter is within the bubble's radius. Otherwise it should return false.

Run the tests again and change your code as necessary until the tests pass.

### Part 2: Popping bubbles

Open `BubbleBlitz`. This class contains the main method to run your game to allow a player to shoot cannonballs at the bubbles to pop them. Start by implementing a single round of the game that prompts a user for input and then fires the cannon. A single round progresses as follows:

- Ask the user for an angle between zero and 180 degrees
  - Hint: The class already has a `Scanner` instance variable that you can use to get input. (What is the name of that variable?) Look at your solution for the Units and Coins if you need a reminder of how to read numbers from a `Scanner`.

- Animate the cannon moving to the specified angle
  - Hint: You **do not** need to modify the `Cannon` class. You also do not need to implement this animation yourself. What are the methods of `Cannon`? Which methods might be relevant? Which are _public_ methods meant to be used from other classes? How do you call those methods from your `BubbleBlitz` class?

- Ask the user for an initial velocity

- Create a cannonball positioned at the end of the cannon, with the specified velocity, angle, and boundaries based on the width/height of the window
  - Hint: Again, pay attention to the methods of `Cannon` that you can use.
  - <details>
    <summary>
    Hint about getting the ball coordinates
    </summary>

    The cannonball’s constructor requires separate `x` and `y` coordinates. But the relevant `Cannon` method returns the `x` and `y` together in one value. What kind of value does it return? How can you get an `x` and a `y` from it?
    </details>

- Add the cannonball’s graphics to the canvas

- While the ball does not hit a bubble and is in bounds:
  - Update the ball’s position
    - `0.1` is a good value to use for the `dt` parameter, but you can play with slightly larger or smaller numbers until you like the feel of the game
  - Call the CanvasWindow’s draw() method to refresh the screen
  - Pause for 10ms
    - Hint: How do you do this? Look for code already in the project that does the same thing: click the magnifying glass in the left pane and search for "pause".
- Remove the cannonball from the canvas
- Call the CanvasWindow’s draw() method to refresh the screen

Requirements and hints:

- You must use method decomposition! Make sure the pseudocode above is **not all in one giant method**. Give each method one small, clear purpose.
- Look at the documentation for `CanvasWindow` to figure out how to pause and remove objects.
- How do you know when the cannonball has hit a bubble? Look at `BubbleManager`. How can an instance of that class tell you when a cannonball has intersected a bubble?

Run the `BubbleBlitz` program and interactively test that you can shoot a bubble.

### Part 2b: Adding some juice

Let’s make it just a little more satisfying when the cannonball hits a bubble. The `Bubble` class has an `animatePop` method. Add one line of code to call `animatePop` _just before_ removing the bubble from the canvas.

The trick here is figuring out _where_ in the code to add that one line. You will have to read and understand some of what’s there to figure this out.

<details>
<summary>Hint on removing bubbles
</summary>

Look in the `BubbleManager` class. Where is the code that removes a bubble from the canvas when it is popped?
</details>

### Part 3: Playing a whole game

Now modify your code so that as long as bubbles still exist on the canvas you can repeatedly keep asking the user for new angles/velocities to shoot again. (Hint: the `BubbleManager` can tell you if there are still bubbles left on the screen.)

Once you can do that, then modify your code again so that when all the bubbles have popped, the game resets and allows you to play again.

If you have practiced good method decomposition, these last two steps should not be too difficult.

### Optional bonus challenges

Occasionally, our homeworks will list optional extra credit tasks that allow you to go beyond the basic requirements of the assignment. You won’t receive a _lot_ of extra credit points for the extra work, so focus on making your code clean and well-decomposed first. However, if your code is in code shape, then take advantage of these fun opportunities to challenge yourself and learn more!

There are many things you could do to go beyond the basics of this assignment:

- Implement a maximum number of shots the user can take before telling them that they lose. Display the current shot count on the screen.
- Add more interesting graphics drawings (e.g. animations of bubbles popping, a more detailed cannon, etc.)
- Implement a class to represent obstacles randomly distributed throughout the sky. The obstacles should block the cannonballs, so that the user has to make use of the projectile curves to hit blocked bubbles.
- Make the bubbles slowly float across the screen while the cannonball is moving.
