---
repo: https://github.com/mac-comp127/inkblot
title_image:
  url: images/inkblots.png
  alt_text: "Four abstract multicolored blobs with horizontal mirror symmetry"
original_url: https://docs.google.com/document/d/1Ge2JjD38MGAbhyLNX5w5EhDYGwSrGlbdn1nT1u15C8A/edit?usp=sharing
---

# Inkblots


{:standard_toc}


## Learning goals

What you are practicing today:

- Creating a class that represents an abstract process (a random walk) instead of a physical thing
- Using instance variables to track state that changes over time
- Enforcing encapsulation
- Use code that you have created to delve into your subconscious

Credits: This activity was created by Daniel Kluver.


## Modeling intangible things

Inkblots have long been used in the field of psychology to study the subconscious. While the science behind them might be a bit dubious, the art of inkblots is pretty fun! In this activity we will be making a computer program that creates inkblot art without any human intervention.

The core programming task in this activity will be building a class named `RandomWalk`. A **random walk** is the name for **something that moves or changes via a series of random steps**, sometimes getting bigger, sometimes smaller. You will implement a `RandomWalk` class that models this behavior, which you will then use to generate inkblots!

Note that “random walk” is a very abstract idea. _What_ is taking the random walk? It could be a particle of pollen, or a temperature, or a price, or…anything! There is not actually a _physical object_ we can pick up and say, “Here, I am holding a random walk in my hand.” That doesn’t make sense. A random walk is an _idea_, a _process_, a _pattern_.

Despite the fact that it is not a _physical object_, a random walk can still be a _Java object!_ In object-oriented programming, **“objects” can model intangible things**: ideas, patterns, processes. All the principles of OOP still apply: grouping state and behavior, encapsulation, abstraction, etc.

In this assignment, you will create a wandering path using two random walk objects for the x and y coordinates. You will then give the paths colors by using three more random walk objects for the red, green, and blue color components. You will thus **create a new abstraction** that you can use for both positions and colors.

In programming, we are constantly on the lookout for new abstractions that will help us. Sometimes we spot those abstractions in the world outside the code  — like the bicycle from the previous activity. But **sometimes, abstractions emerge from the code itself**. “Gosh, we seem to be doing similar thing X in many different places. Can we create a new abstraction for X that might make our work easier to do, or easier to understand, or more reliable?” In this activity, “random walk” is one such abstraction.

## Step 1: Implement RandomWalk

1. Open `RandomWalk`. This has the very basic outline of a class: A single instance variable (`Random rand`), a constructor, and some methods which are currently not implemented.
2. Add a new `int` instance variable to represent the current random walk value. _Think:_ How do you make it an _instance_ variable, and not a _local_ variable?
3. Modify the constructor so that you can pass an initial value for this variable to the constructor.
4. Fill in the `getValue` method to return the current value of the variable.
5. Fill in the `advanceValue` method. This should randomly either add 1 or subtract 1 from the value. (HINT: use `rand.nextBoolean()`.)

After implementing these methods, you have now created a class that unites state (the current value) and behavior (taking a random step) to create a new abstraction.

## Step 2: Making basic inkblots

1. Open the InkBlot Class. This file has a similar structure to some of the emoji-related classes from earlier activities: it has a single variable (the `canvas`) which is created in the constructor. It has a main method that creates an instance of `InkBlot` and then calls the `generateInkBlot` method. Before implementing this method, you will want to look over the helper methods provided in this file.

2. Fill out the `generateInkBlot` method. To make an inkblot we will be using two `RandomWalk` objects to randomly move around the screen drawing shapes. To begin with, create two `RandomWalk` objects to represent the X and Y coordinates we will draw. Make the `RandomWalk` objects start in the middle of the canvas, at (150, 150).

3. To draw an inkblot, you will repeat the following process 15,000 times: advance the X and Y position, then draw a line from the previous position to the new position.

   To make this easier, we have provided you with the `advance` method, which takes two `RandomWalk` objects, advances them, and returns a Line from their previous value to the current one. Take a moment to read this code and ensure you understand how it works.

   As `InkBlots` are traditionally symmetric, we have also provided a `mirror` method which you can use to get a second line object that has been mirrored around the middle of the screen. Using this properly will take a little thought.

4. At this point you should be able to generate inkblots! Note that Kilt Graphics waits for your code to finish before drawing anything on the screen. If you want to be able to see the random walk while it grows, you can add a call to `canvas.draw()` to your loop to show you the drawing in progress.

## Step 3: Adding color

Inkblots are cool, but it would be nice if they were colorful. To make the inkblot have color we are going to use more `RandomWalk` objects: one each for the red, green, and blue values of the color. (Red, green, and blue light can mix to form any human-perceptible color. The computer screen on which you are reading this text is made of tiny glowing dots of nothing but red, green, and blue!)

Add three new `RandomWalk` objects to your method for red, green, and blue. Your code now creates five different `RandomWalk` objects, so that the x, y, red, green, and blue values can all change independently. But wait! There is only _one_ current value instance variable in the code! How can there be _five_ independently changing values? <hidden>Because the current value is an **instance variable** of `RandomWalk`. That means each `RandomWalk` object has its own current value, just like every bicycle in the previous activity had its own speed and direction.</hidden>

To help with this task, we have created a `getColor` method which takes three `RandomWalk` objects. It will advance the random walks and return the color indicated (if legal) or dark gray if the color is invalid (red, green, and blue must be \>= 0 and \<256).

Discuss: Why is the drawing mostly dark grey? This will take some investigation of the code.

## Step 4: Expanding RandomWalk

The `RandomWalk` class is nice, but based on our usage it has a major flaw. `RandomWalk` objects can “walk” into invalid color values, or “walk” right off the side of the screen! It would be better if we could place limits on the `RandomWalk`.

Modify `RandomWalk` to have two new `final` instance variables, `min` and `max`, representing the minimum and maximum value the `RandomWalk` should take. (Why can they be `final`, but the current value can’t be? <hidden>Because <code>min</code> and <code>max</code> never change, but the current value changes with every step.</hidden>) Add constructor parameters to allow setting these values when creating the object. Then modify the `advanceValue` method so that it will not set value to be too big or too small.

Once you have done that you will need to update the `RandomWalk` objects in `InkBlot`. Make the x / y walks stay on the screen, and the red / green / blue `RandomWalk` objects stay within valid color ranges (which are 0 through 255).

## Extra ideas for extra fun!

Now that you have working inkblots there's a lot of fun to be had here are a few ideas:

- What happens if you use more or fewer steps to make the inkblot?
- Modify the `RandomWalk` to be able to take different sized steps. How does this affect your drawing?
- Try variations on the line coordinates. An interesting one: make all the lines go back to the center of the screen instead of the previous position.
- Try creating a new class, `Epicycle`, that has the same `getValue()` and `advanceValue()` methods as RandomWalk, but returns this value:

      sin(sin(t)) * (max - min) / 2 + initialValue

   …for some t that increments by some rate for each step, where the rate is provided in the constructor and different for each `Epicycle`. Try to write it so that you can substitute `Epicycle` for `RandomWalk` in the `InkBlot` class, perhaps changing the constructor parameters, but not making any other changes.

- Replace `mirror()` with n-fold rotational symmetry.
- Add a text/scanner based interface to allow you to generate a new inkblot as many times as you want. (You can keep creating new `CanvasWindow` objects.)
- Use the `GraphicsText` object to allow adding labels to your inkblots.
- Add a virtual psychologist that asks you what you think of inkblots and gives a stunningly accurate psychological diagnosis.
