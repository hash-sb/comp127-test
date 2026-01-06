---
name: "Graphing Calculator, Part 2"
repo: https://github.com/mac-comp127/graphing-calculator
title_image:
  url: images/image1.png
  alt_text: "Jagged, overlapping mountain-like mathematical curves"
original_url: https://docs.google.com/document/d/1JwW6sBtLPmJKukz_aG6Nvts7R2sjxtUBMEcOlqUuyLQ/edit?usp=sharing
---

# Graphing Calculator: The Sequel


{:standard_toc}


<callout>
  This activity uses the **same GitHub Classroom assignment as [part 1](/activities/graphing_calculator_1)**. Continue working with the same partner(s). No need to accept a new assignment, or to clone anything new. Please work from the same repository as before.
</callout>

## Learning goals
The goal of this activity is to learn about how programs allow users to interact with them by handling **input events**.

You will continue working with the same graphing calculator as in the previous activity, add a user interface to let the user control the view and animation.

Here is the **[Kilt Graphics API documentation](https://mac-comp127.github.io/kilt-graphics/)**; you’ll need it! (You can always find it in the sidebar of this course website.)

## Get set up

You need the graphing calculator showing some animated functions. If you didn’t get that far in the previous activity, or if your functions are messy, or if you just want to see something cool, copy the following code into `GraphingCalculator`’s main method (and run it to make sure it works):

    for (int n = 1; n < 12; n++) {
       double base = n * 0.1 + 1.5;
       calc.show((x, t) -> {
           double result = 0;
           for (int i = 1; i < 20; i++) {
               result += Math.sin(x * Math.pow(base, i) - t * i * 3)
                         / Math.pow(base, i);
           }
           return result;
       });
    }

## Buttons

Let’s add two buttons to the graphing calculator that will make it zoom in and out. In `GraphingCalculator`’s constructor (not the main method, the constructor!), create two `Button` objects with the titles “Zoom In” and “Zoom Out.” Add them to the canvas and position them in a corner, one next to the other.

<callout>
  Make sure you import the correct class, `edu.macalester.graphics.ui.Button`, and not some other class named “Button.” There are several `Button`s floating around in Java’s universe, and the activity won’t work if you import the wrong one.

  You can double check which classes you’ve imported by looking at the `import` statements at the top of the file, or by command-clicking (macOS) or ctrl-clicking (Windows) on the word `Button` in the code.
</callout>

Run your code. You should see your two buttons floating above the animated graph — and see them respond when you click them! But they don’t do anything yet. You need to say what should happen when the button gets clicked.

## Event handlers

Add the following code:

    zoomIn.onClick(() -> setScale(getScale() * 1.5));

Run your code again, and you should be able to enlarge the plot by clicking the Zoom In button.

(Note that we have to use the setter method instead of setting the `scale` instance variable directly, so that the plot will update.)

Let’s take a moment to analyze that code. You create a zero-argument closure that calls `setScale()`. That closure does _not_ run immediately. Instead, you pass the closure to the `onClick` method of the `zoomIn` button. The button holds on to the closure, and runs it whenever the button is clicked.

This is a classic example of what a closure is good for. It lets you say _what_ should happen, but lets other code decide _when_ it happens. Note that the closure depends on local context: it calls methods of this `GraphingCalculator` object, which the closure can do because it captures the implicit `this` variable. The closure thus serves to tie an <def>event</def> from the outside world — in this case, a button click — back to this code’s local world. A closure or function that runs whenever an event occurs is called an <def>event handler</def> or <def entry="event handler">event callback</def>.

Add code to make the `zoomOut` button work too. Test it and make sure it works.

(Should you add the button event handlers _before_ or _after_ the call to `canvas.animate()`? Does it make any difference? <hidden>Nope!</hidden> Why? Discuss with your partner. If you’re not sure, try <hidden>adding some print statements before, inside, and after the event handler closures, and study the order in which the print statements actually happen.</hidden>)

## Mouse events

Temporarily comment out the whole call to `canvas.animate()`. We will enable animation again later, but for now we want it off.

Add the following code to the constructor:

    canvas.onDrag(event ->
       setAnimationParameter(
           event.getPosition().getX() / width));

This tells the canvas what should happen when the user drags the mouse over the canvas. (“Drag” means moving the mouse while the button is held down.) The closure you pass to `onDrag` gets called repeatedly as the mouse moves. In this case, we take the x position of the mouse and use it to change the animation parameter.

Note that this closure takes one parameter. It is a `MouseMotionEvent`. (You can look at that class’s API in your IDE, or in the [kilt-graphics javadoc](https://mac-comp127.github.io/kilt-graphics/edu/macalester/graphics/events/MouseMotionEvent.html).) Event handlers often receive <def term="event object">event objects</def> that describe the event that caused the handler to be called.

Run the code and try it! You should be able to manually animate the plot by dragging the mouse back and forth.

## Dealing with coordinates

Note that we are using the _absolute_ x position, which means that the animation parameter jumps suddenly to wherever you start dragging. Try dragging the plot all the way to the right, releasing the mouse button, moving the mouse all the way to the left, and then dragging again. Note the sudden jump when you start dragging again.

It would be nice if instead our event handler started the animation parameter from its current value, then added the _relative motion_ of the mouse to it. The `event` object has a `getDelta()` method that can help you. It returns the difference between the mouse’s current position and its position at the previous mouse event. You can use it like this:

- Use `getDelta()` instead of `getPosition()` in the event handler, so you are computing how much the animation parameter should _change_ instead of computing its _new value_.
- Now add that change to the animation parameter’s current value.

Here is the formula for computing the new value of the animation parameter: <hidden>event.getDelta().getX() / width + getAnimationParameter()</hidden> You will still need to use that to update the animation parameter.

Once you’ve done this, when you do the drag-lift-move-drag sequence described at the beginning of this section, the plot should move smoothly.

## Mouse button events

Try uncommenting the animation code again. Can it animate and drag at the same time? Let’s find out! Run the program and see.

Note how janky the motion is when you drag in the opposite direction of the animation. What is happening is that Java is rapidly alternating calls to the closure passed to `animate()` and calls to closure passed to onDrag(). If the two closures move the animation parameter in opposite directions, you see it twitching back and forth rapidly:

| `animate`   | ➕ increase `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
| `mouseDrag` | ➖ decrease `animationParameter`  ← dragging starts here |
| `animate`   | ➕ increase `animationParameter`                         |
| `mouseDrag` | ➖ decrease `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
| `mouseDrag` | ➖ decrease `animationParameter`                         |
| `animate`   | ➕ increase `animationParameter`                         |
{:.compact}

To fix this, we’d like to stop the animation when the user starts dragging, and resume it when they finish.

First, let’s make a boolean flag that can start and stop animation. Declare a new boolean instance variable named `animating`, and change the animation closure so that it only does something when `animating` is true. (Hint: to put an if statement in the closure, you will need to convert it to a multi-statement closure.)

Test your change. Try initializing `animating` to false and run the program, and you should see no animation. Now try initializing it to true, and you should see animation.

That working? There are two event handling methods in `CanvasWindow` called `onMouseDown()` and `onMouseUp()` that can notify you when the mouse button goes down and up, respectively. Add a mouse down handler that sets `animating` to false, and a mouse up handler that sets it to true. Now the sequence of events looks something like this:

| `animate`   | `animating` is true, so change `animationParameter`     |
| `animate`   | `animating` is true, so change `animationParameter`     |
| `mouseDown` | **set `animating` to false**                            |
| `animate`   | <ghost>`animating` is false, so do nothing</ghost>      |
| `mouseDrag` | change `animationParameter` using mouse event’s delta x |
| `animate`   | <ghost>`animating` is false, so do nothing</ghost>      |
| `mouseDrag` | change `animationParameter` using mouse event’s delta x |
| `animate`   | <ghost>`animating` is false, so do nothing</ghost>      |
| `animate`   | <ghost>`animating` is false, so do nothing</ghost>      |
| `mouseDrag` | change `animationParameter` using mouse event’s delta x |
| `animate`   | <ghost>`animating` is false, so do nothing</ghost>      |
| `mouseUp`   | **set `animating` to true**                             |
| `animate`   | `animating` is true, so change `animationParameter`     |
| `animate`   | `animating` is true, so change `animationParameter`     |
{:.compact}

Run your program and try it! The animation should freeze when you press the mouse button, allowing you to drag smoothly, then resume when you lift the mouse button.

---

## Super amazing bonus challenge tasks

(That heading may be cheeky, but these _are_ actually pretty cool if you get them working!)

### Implement smartphone-style “coasting”

Instead of having the animation always move in the same direction at the same speed, we could use the mouse movement to make the plot continue coasting in the speed and direction the user was dragging. Here is a sketch of how to do it:

- Instead of making the animation increment by a constant as the current code does, create an instance variable named `animationSpeed`, initialize it to 0.01, and use that in the animation closure. Test that; animation should look the same.
- In the `onDrag` handler, in addition to calling `setAnimationParameter()`, take the same quantity that you just added to the animation parameter, take the average of that quantity with the current `animationSpeed`, and make _that_ the new value of `animationSpeed`. Test that; you should now be able to make the animation coast in either direction.
- Did you end up with a big repeated subexpression in two places? If so, try extracting it to a local variable inside the closure. Test again after to make sure it still works.

(The actual algorithms used by smartphones, tablets, and so on is more sophisticated than this, but this simple one feels reasonably nice.)

### Implement panning

It would be nice to be able to move the view around, wouldn’t it?

Alter your `onDrag` handler so that it does two different things:

- If the shift key is pressed, add the mouse event’s delta to the `origin` instance variable. (Note that they are both `Point`s and `Point` has an `add()` method, so you do not need to add the x and y components separately.)
- If the shift key is not pressed, do what you are currently doing.

Hint: use the `getModifiers()` method of `event` to see what modifier keys are pressed. This will take some API exploring! See if you can figure it out. Here are some hints:

<hidden>You can use the constant <code>ModifierKey.SHIFT</code> to refer to the shift key.</hidden>

<hidden><code>getModifiers()</code> returns a <code>Set</code>. Find the Javadoc for that class. How do you test whether a particular value is in a <code>Set</code>?</hidden>

### Make zoom preserve the visible center

The current zoom code zooms the graph centered on the origin. If you have panned far away from the origin using the shift key, pressing the zoom button now makes the graph jump quite a bit! Try altering the zoom code to preserve the current center of the screen, whatever it is.

This is tricky! Here are some hints:

<hidden>Use the methods of <code>Point</code> instead of doing math directly with x and y coordinates.</hidden>

<hidden>You want to move <code>origin</code> closer to or farther from the current center of the canvas by the same factor you multiplied the scale by.</hidden>

<hidden>You can use <code>canvas.getCenter()</code> to get the current center of the canvas.</hidden>

<hidden>Subtract the canvas center from <code>origin</code>, then scale that, then add the center back.</hidden>
