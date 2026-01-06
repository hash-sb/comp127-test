---
repo: https://github.com/mac-comp127/critters
title_image:
  url: images/critters-transparent.png
  alt_text: "A cartoonish boxy robot and round bug standing next to each other and looking at each other"
---

# Critters

<callout>
  When you accept this assignment, you will be asked to join a team. Please choose the one existing “All students” team. The whole class shares code for this one!
</callout>


{:standard_toc}


## Learning goals

Today's activity is a bit of silly fun in the name of practicing graphics programming and using inheritance. This project provides a framework for building little critters that wander around the screen. You will add your own critters.

There are two deeper pedagogical purposes for today’s activity:

- **Using inherited methods.** You’ve read about the drawbacks of inheritance, but:
  1. Inheritance still in widespread use, and you should know how to with with it. In particular, existing code often asks you to use features that it provides in a superclass and your code inherits. You’ll practice this pattern today.
  2. Sometimes inheritance works out fine, and you can hold the drawbacks at bay! This activity is one of those times.

- **Learning by playing.** Your work in this class so far has come with a lot of scaffolding: carefully designed starting code, precise directions, lots of hints, and very specific goals. This a great way to learn in class and a great way to get a good start on something unfamiliar, but it’s not what most learning will look like throughout your life. Instead, you’ll be figuring out unfamiliar things on the fly — things for which nobody exactly prepared you, and for which there are no clear directions. _That_ is what a liberal arts education is really preparing you for: open-ended learning beyond school. All these courses you’re taking? They’re just a runway to get your plane in the air.

  Think of how little children learn to talk and walk. They don’t take classes for it. They don’t have assignments and deadlines. They don’t get graded on it. They _mess around_ and _explore without any clear goal_ and _make lots of mistakes_ and _just keep going_. They **play**. And it _works_.

  Think of how little kids will happily just _scribble_ on a piece of paper, without any sense that their scribbling needs to be good enough to get an A, or impressive enough to put on their resumé. Bring some of that “scribble attitude” to your computer science learning. Sometimes, you can just _scribble with code_. Believe it or not, this kind of playing around — not courses, not trainings, but **open-ended experimentation** — is the way expert developers learn the majority of what they know. It is _powerful_. Do it.

Now, ready to play?


## Create your critter

Begin by studying one of the existing critters. Take a look at `BoxBot`, `RoundBug`, and `Mario` as examples. Run `CritterTester` to study the individual critters. Then run `CritterParty` to see all of them wandering around together.

Now create some new critters of your own! Create a new class that extends `Critter`, called `<<YourTeamNames>>Critter` (but with your actual names or nicknames, with the first letters capitalized). This is to make sure that your critter’s name is unique. Add a comment to the top of your `Critter` file with your full names.

There are only three requirements for your first critter:

- Add animated legs or eyes (or both), as the example critters do.
- Don’t make it so big that it hogs the screen. Play nice with the other critters.
- Have fun! (This rule is the most important one.)

Test your new critter using `CritterTester`. Change it to create one of your critter rather than one of the examples.

Design your critter so that it approximately fits within the grey guide box that `CritterTester` draws.

### Important notes

- Look at the examples to see how they add the animated legs and eyes. Study how they use `addLeg(new Leg(...))`.
- One great way to “scribble” in code is to take **existing code that already works and modify it**. You can do that with the critters! You might start by copying some or all of an existing critter and changing it — first small changes, then large — until you have something new.
- If you want to use an image, as Mario does:
    - Make sure that your image is small enough that it fits within CritterTester’s guide box.
    - Place your image in the directory called `res`, not in `src`. This is the resources directory.
    - Prefix the image’s filename with your GitHub username, e.g. `pcantrell-octopus.png`. PNG and JPEG image formats should work.
    - Load your image like this: `new Image(0, 0, "pcantrell-octopus.png")`. This will give an image you can position, resize, and add to a graphics group just like any other graphics object.
    - You should still add animated features so it looks good in the `CritterParty`. Don’t just drop in a fixed image and call it done. Remember: play!!
- Make sure you **add your name(s)** to your critter class name **and** image names to avoid Git conflicts.

The code in `CritterParty` is written to automatically detect new critters, but does so randomly, so it is best to use this for text _after_ you have your new critter drawing properly in the `CritterTester`.

We hope that you will be able to finish at least one new critter of your own invention during class time! Work quickly, and don’t worry about perfection. Remember: scribble! Have fun!


## Share your critter

Once you have a new Critter working and **thoroughly tested**, then:

- Open your project in GitHub Desktop, and…
- …<highlight>**before you commit anything**</highlight>, uncheck the checkbox next to `CritterTester` so that you aren’t committing your changes to it. You can even right-click it and choose “Discard Changes” to make sure you don’t commit them. Make sure you **only discard changes to `CritterTester`**. Don’t lose your work!

   <callout>
     If you commit any changes to `CritterTester`, you will get merge conflicts with the other students. If you accidentally committed changes to that class, **ask the instructor for help before you push!**
   </callout>

- Commit your code.
- Now pull other people's critters by choosing Repository → Pull. 
- Did any changes come in? Run `CritterParty` again to make sure everything still works.
- Finally, push your changes by choosing Repository → Push.

## Repeat for extra fun!

You only need to make one critter to complete the activity, but more critters make for a merrier critter party!

As the critters come in, we’ll have a class critter party on the projector.

## Acknowledgements

Paul developed this activity, Shilad tweaked the Eye code, Bret tweaked the default critters, Libby added x, y offsets for Critters and a test program.
