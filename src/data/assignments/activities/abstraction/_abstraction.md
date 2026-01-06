---
original_url: https://docs.google.com/document/d/1aLsLyVsJOcpQtD9OdmHcz_-1XQ2V2GZS7k8ufMXi7O4/edit
hide_general_instructions: true
---

# Abstraction

Welcome to COMP 127! To get class started, here is an activity that has no code, but that conveys some of the most important lessons of the whole semester.


{:standard_toc}


## Goals of this activity

- Learn about **abstraction**.
- Meet some of your classmates.
- Start learning and practicing the social norms and communication habits of in-class activities in this course.

## The setup

- Form groups of **2–3** students.
- Introduce yourselves within your group with your names. It can also be helpful to share your pronouns, your year at Macalester, and your major(s)/minor(s).
- Take a moment to share one thing that you are excited about, either in general or just today, so that your compatriots for the day have a little something to remember you by.
- **Remember:** Be a good partner! Make sure everyone has a chance to speak and be heard. Your job is to support each other.

## One task described three ways

Here are some instructions for a familiar everyday task, which you should open in a new tab:

> **[Instructions, Version 1](version1)**

Read the instructions together (either out loud together or silently side by side, as your group prefers). What are these instructions describing? Can you figure it out?

Here is a different set of instructions:

> **[Instructions, Version 2](version2)**

And yet another:

> **[Instructions, Version 3](version3)**

These instructions all describe the same task. However, they are not the same! What is the difference between them? The answer to that question sits at the heart of this class.

## Clarity

Which version of the instructions is _clearest?_ Well, what does “clearest” mean? Consider these three different ways we might interpret “clearest,” and take just a minute or two to **discuss with your group**:

1. Which version is most _precise?_
2. Which version is most _unambiguous?_
3. Which version was _easiest for you to understand_ as you read it?

When we talk about **code** being “clear,” about it having “clarity,” we are usually talking about #3 in that list: code that is easy for humans to understand. _All_ computer code is _precise_ and _unambiguous_, by definition, because a programming language must be readable by a machine. However, as you no doubt have seen in your own experience writing and reading code, some pieces of code are much easier for humans to understand than others!

Because **software is created by humans for humans**, because each line of code in a typical software project is _read_ by a human dozens or hundreds of times for every time it is _modified_, clarity matters. **In most circumstances, good code is not clever or complex; it is _obvious_.**

## Abstraction

What is the difference between the three versions of the instructions above? You might say that Version 1 has more detail than Version 2, which has more detail than Version 3. Yes, the level of detail is part of it, but there’s an even more fundamental difference: the three versions work with different vocabulary, different conceptual building blocks.

Version 1 of the instructions only refers to body parts (arm, elbow, fingers), materials (metal), and basic geometric shapes and directions (rectangle, rod, axis, horizontal, rotate, clockwise).

Version 2 introduces the words “handle,” “grasp,” and “pull.” Note how those words subsume most of the details in the first version.

Version 3 introduces the concepts “door” and “open,” which together cover the entire task.

In software, we have a name for these conceptual building blocks that gather up many details into a tidy package. They are **abstractions**.

<definition-callout>
  An <def>abstraction</def> takes something complex and, by hiding its details, presents it in a form that is simpler.
</definition-callout>

We say that Version 1 is at a **lower level of abstraction** (more detail, simpler building blocks), and Version 3 is at a **higher level of abstraction** (less detail, more hidden complexity). Those details that an abstraction hides are called <def>implementation details</def>.

Much of the work of programming is about **choosing** abstractions, **understanding** abstractions, **creating** abstractions, **altering** abstractions, and **moving between levels** of abstraction.

How do we take **code** at a lower level of abstraction, code that looks like Version 1 of the instructions, and transform it to use higher levels of abstraction like Version 2 and Version 3? What programming languge tools and technologies do we use to do that? What human thought processes and social practices do we use to do that? These questions are at the heart of COMP 127. It’s a long journey, and a journey that does not end with this course. It’s not a journey that ends at all; the software world is constantly rethinking this.

This class is about how we say in code, “There are things called _doors_, and they can _open._”

## Generality and resilience

For convenience, here are those three versions of the door-opening instructions again:

- **[Version 1](version1)**
- **[Version 2](version2)**
- **[Version 3](version3)**

The instructions above were written specifically for the door that leads from OLRI 256 to the hall. Discuss with your group: which of those three versions of the instructions would have to change…

- …if the door were made of wood?
- …if the handle were on the left instead of the right?
- …if the door had a round knob instead of a handle?
- …if the door opened [inward instead of outward](https://www.reddit.com/r/TheFarSide/comments/1c4xudt/midvale_school_for_the_gifted/)?
- …if the door didn’t latch, had no knob, and simply opened by pushing?
- …if the door were being opened by a person who, either due to a disability or because they are using both arms to carry something, does not have use of their hands?
- …if the door is already propped open?
- …if the door is locked?

<callout>
  Note: **Do not** actually edit the different versions of the instructions in response to these questions! Just have a quick discussion to identify which versions would need to change.
</callout>

Improving clarity is not the only motivation for abstraction. If done right, abstraction can also create **generality** and **resilience**.

<definition-callout>
  <def lowercase>Generality</def> is the ability for the same code to handle many situations.
</definition-callout>

If “open” means different things for different doors — turning a knob versus pushing a bar, for example — Version 3 has enough _generality_ to handle that variety.

<definition-callout>
  <def lowercase>Resilience</def> is the ability for existing code to continue working despite changes in its context.
</definition-callout>

If we create a new kind of door that opens in some new way — maybe you have to turn a crank while whistling and patting your head, for example — Version 3 is _resilient_ to that change.

## Play

**Get the instructor’s attention**, and let them know you’ve reached this point in the instructions. They will hand you an everyday task at a high level of abstraction, like Version 3. As a group, try rewriting it at a Version 2 or even a Version 1 level of abstraction. (_Watch the clock! Class is ending soon._)

Once you have your version, show it to a neighboring group (and introduce yourselves!), and see if they can figure out what everyday task it describes.

Have fun!

## What’s the catch?

Doesn’t abstraction sound great? **One last discussion question for your group** if you have time, or something to take home and ponder if the clock is running out:

> Isn’t a higher level of abstraction _always_ better? Why would we _ever_ want instructions like Version 1 or Version 2? Do lower levels of abstraction serve any purpose?

Think, discuss, then scroll down for some answers.

<scroll-for-answer/>

Here are some reasons software developers may want to work at a lower level of abstraction:

- _Somebody_ has to implement the abstractions in the first place! (If there are no doors, then somebody needs to design and build the first one. If you’ve never used a door before, you have to learn the details of what “open” means in order to open it. These same principles apply in code.)

- Even if you aren’t creating an abstraction yourself, learning about its implementation details can…
  - …help you use it more effectively.
  - …help you diagnose and fix problems.
  - …help you suggest improvements.
  - …help you _avoid_ suggesting improvements that seem like good ideas but don’t actually make practical sense.
  - …help you use your tools with a bit of appreciation instead of only being angry at their shortcomings.

- Sometimes the available abstractions don’t fit the problem you have, and you need to work with lower-level details to solve it well. Sometimes (often!) nobody has figured out an abstraction that fits the problem, one that really works well — at least not yet. As software developers, we live our lives in a world of details, always on the lookout for abstractions waiting to emerge.

- Last but definitely not least: it’s **fun** to learn how things work!

One of the best things you can do as a software developer is to get comfortable **moving between levels of abstraction**, both higher and lower. That is a practice you’ll be cultivating both in this class and long after.


