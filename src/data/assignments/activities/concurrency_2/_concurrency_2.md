---
name: "Concurrency, Part 2"
repo: https://github.com/mac-comp127/two-kinds-of-concurrency
original_url: https://docs.google.com/document/d/11T12QttzSNMVwWX2wR0jZ_cwEJEQigaPCQGxwNcP0qI/edit?usp=sharing
---

# Two Kinds of Concurrency, Part 2: Playing Nice, Taking Turns


{:standard_toc}


<callout>
  This activity uses the **same GitHub Classroom assignment as [part 1](/activities/concurrency_1)**. Continue working with the same partner(s). No need to accept a new assignment, or to clone anything new. Please work from the same repository as before.
</callout>

Recall that <def>concurrency</def> is when **multiple tasks happen during overlapping timespans**. When code runs **concurrently**, one task or operation starts _before others are finished._

How does that work? And why would you do it? In Part 1, we looked at introducing **parallel execution**  in order to **make code faster**. In Part 2, we will look at a different kind of concurrency that solves a different problem.

(This activity uses the same repository as [part 1](/activities/concurrency_1).)

## Cell Absorption Redux

Run the main method of `CellUI` in the `cellabsorption` package. This code is a slightly refactored version of the solution to the Cell Absorption lab.

Take a look at the main method. What makes the animation happen? <hidden>The while loop.</hidden>

Our goal in part 2: make it so that clicking on the window adds new cells to the simulation.

## Let’s try something that doesn’t work!

Try adding the following code **after**  the while loop:

    canvas.onMouseDown(e ->
        simulation.createCell(e.getPosition(), 5));

This code will create a new cell at the mouse location every time there is a click on the canvas. And this code is correct!

However, this code will cause a compile error, and Java will refuse to run your program at all.

**Read the error message.**  What does the error mean? Why are you getting this error? <hidden>The while loop never ends, you added code <em>after</em> that loop, and therefore the code you added will never run.</hidden> Why would Java choose to make this an error? <hidden>Because you wrote code in a place where it cannot <em>possibly</em> ever run, so there must be some serious misunderstanding.</hidden>

Putting this code after the while loop won’t work.

## Let’s try something else that also doesn’t work!

The obvious solution is to move those two new lines of code _before_ the while loop. **Try that**.

Your code will now run. Try clicking on the canvas to create new cells. What happens? Why? The problem in this case is far less obvious. Make some guesses. Discuss with your partner.

When you are ready for the answer, scroll down.

<scroll-for-answer/>

The call to `canvas.onMouseDown` does not mean “create a new cell right now!” It means “create a new cell _in the future_, whenever the user clicks on the canvas.”

That code works, just as you have it: the canvas is all ready to respond to mouse clicks, and it remembers that whenever there is a click, it should call your closure. So far so good.

Then the infinite loop starts.

Now we have a tricky question: if the user clicks the mouse, _when_ should that closure that creates the new cell actually run?

Kilt Graphics _could_ choose to handle the `onMouseDown` event in **parallel**: while the infinite loop is running, _at the same time_, it runs the closure that creates a new cell. Would that work? Well, remember some of the lessons of Part 1:

- Parallel computing can also make code break. It is **dangerous**.
- If your own code does need to exist in a context where there is parallelism, remember that you are walking on lava, and **beware of shared mutable state**.

Is there any **mutable state**  that is **shared**  between the animation loop and the mouse down handler? <hidden><strong>YES</strong>: the <code>CellSimulation</code>’s list of cells, the <code>GraphicsGroup</code>’s list of children, the canvas itself, and even the <code>Random</code> object in <code>CellSimulation</code>! All of these have state that changes. And all are shared between the animation and the mouse down handler.</hidden>

Remember from Part 1 how badly even a simple line of code like `total += x` could fail in the presence of parallelism. Is there any hope of us making all the complexity of the whole cell simulation and canvas thread-safe? It’s not impossible, but it would be a formidable and error-prone task!

## What do we do, then?

The trouble here is that we don’t want _parallelism_ if we can help it, but we really want _concurrency_.

Why concurrency? Because we want the “handle mouse down” task to happen _concurrently_ with the “animate the canvas” task.

Why don’t we want parallelism? Because it’s going to be hard to make it work without bugs, it would require tools and knowledge we don’t yet have, and it would probably always be prone to new bugs even if we did get it working. And for all that trouble, _performance is not the problem we have!_ It’s not that “animate” or “add a cell” runs too _slowly_; we just don’t want “add a cell” to have to _wait_ for “animate” to finish, because that would mean “wait until after forever.”

User interface libraries like Kilt Graphics — and web browsers, and iOS and Android and Windows and Mac and just about anything you can think of — all solve this problem as follows:

1. **Make everyone take turns.**  Wait for each function / method to return before letting the next one start. Don’t even _draw_ anything new on the screen until each method has finished its work.
2. **Keep track of everyone who is waiting for their turn.**  If new events like “mouse moved” or “key down” happen while some other task is already working, queue them up (i.e. have them wait in line) until other tasks are done.
3. **Break long-running tasks like animation into small pieces.**  That way all the small pieces can take turns with each other. Each task’s job is to do a little work _quickly_ and then get out of the way.

Instead of having an infinite loop handle our animation, we will give Kilt Graphics a closure that handles just **one frame of animation**, and we’ll ask Kilt Graphics to let other events take turns with that closure. The result looks like this:

| **call animation closure** |
| (wait 1/60 sec) |
| **call animation closure** |
| **oh, mouse pressed! call the onMouseDown closure** |
| (wait 1/60 sec) |
| **call animation closure** |
{:.compact}

There is **no parallelism here**: each closure finishes before the next one starts. However, there is still **concurrency**, because other tasks happen during the (endless) timespan of the “animation” task. This approach is called <def>cooperative concurrency</def>, or <def entry="cooperative concurrency">cooperative multitasking</def>.

The particular cooperative concurrency approach that user interface libraries like Kilt Graphics use is called an <def>event loop</def>. There is in fact an infinite loop buried deep in the guts of some library, but you never see it! That loop basically looks like this:

```text
while (program is running) {
    wait until there are events to handle
    get next event
    call whatever code handles that event, and wait for it to finish
    draw new graphics on the screen if necessary
}
```

The details are more complicated, but the basic idea is as simple as that.

<callout>
  Aside: This pattern extends far beyond Kilt Graphics and Java. Almost every GUI platform in the world has an event loop of some kind: HTML+JS in a web browser, iOS / macOS UIKit and SwiftUI, Android and Android Compose, Windows WinUI and .NET and MFC, the Unity and Godot game engines…you name it. Even some server frameworks (like Node/Express) and entire programming languages (like Erlang and Elixir) use an event loop pattern or something like it.
</callout>

## Let’s fix it

Here is how Kilt Graphics handles animation:

    canvas.animate(() -> {
        ...do one frame of animation...
    });

There is also a second variant where the closure accepts a parameter, which tells you exactly how much time has elapsed since the previous animation. You can use this in some situations to make the animation a little smoother:

    canvas.animate(dt -> {
        ...do one frame of animation...
    });

…but for the purposes of this activity, we’ll use the first version.

There are two important things to know about `CanvasWindow`’s `animate` method:

- You do not have to pause. The UI library handles the timing of the animation for you.
- You do not have to call `draw()`. The UI library always updates the screen if necessary after any event handling closure is done.

<callout>
  **Take note:** <highlight>you do not need `canvas.draw()` or `canvas.pause()` anymore</highlight>. Those methods were just training wheels, so that you could do loop-based animation in earlier assignments before you had closures. Now the training wheels come off. You do not need to call those methods in Breakout, or in **any of the subsequent assignments in this class**.
</callout>

You now have what you need. Let’s make this cell simulation work! Your job:

- Replace the `while(true) { … }` loop with `canvas.animate(() -> { … })`. Format it nicely! Check your indentation!
- Remove the calls to `draw` and `pause`.
- Test it.
- Anything you can simplify? Anything you need to clean up?
- Test it again, and have fun clicking and creating lots of cells!

## Bonus challenge

Make it so that dragging the mouse resizes the cell you just created. This is tricky!
