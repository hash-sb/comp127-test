---
repo: https://github.com/mac-comp127/units-and-coins
---

# Units and Coins

This assignment teaches some big lessons about testing, numerical error, repetitive code, and refactoring.

## Learning Goals

- Get more practice using and writing unit tests.
- Learn about the `%` operator if you haven’t already encountered it.
- Learn about the perils of floating point arithmetic.
- Learn about the perils of excessively repetitive code.
- Practice two excellent software development patterns:
  - refactoring _before_ making a change in order to make that change easier and less error-prone, and
  - using unit tests to refactor with confidence.
- Practice using Java’s method declaration syntax, and thinking about parameter and return types.
- Experience the deep, soul-renewing pleasure of deleting code.

## Setup

1. Make sure you have completed the Installfest instructions (on our course web site) successfully.
1. Follow the [Homework / Take-Home Exercise Procedure](https://comp127.innig.net/resources/take-home-procedure/).

## Task 1: Sheppeys

This problem is a warm-up to give you more practice working with unit tests.

A “Sheppey” is a fanciful measure of distance equal to 7/8 of a mile. It is defined as the closest distance at which sheep remain picturesque! More information on the definition can be found at Wikipedia's [List of Humorous Units of Measure](https://en.wikipedia.org/wiki/List_of_humorous_units_of_measurement#Sheppey).

Good news: software can helpfully convert this unit to something more familiar for you. Hooray! In the `src/` directory of this project, in `UnitConversions`, there is a `sheppeysToMiles` method. Unfortunately, the method is not implemented correctly: it always returns zero.

In the `test/` directory, in `UnitConversionsTest`, there is a unit test for this method. The test is correct, and would pass if the method were implemented correctly!

1. Implement `sheppeysToMiles` correctly, so that the test in `UnitConversionsTest` passes.
   (**The test is already correct.** You only need to change the code in `sheppeysToMiles`, to make it return the correct result instead of zero.)
2. Create a new method named `milesToSheppeys` to do the conversion in the other direction. Thing to check:
   - Make sure that you _add_ this new method and new test instead of _replacing_ the existing ones.
   - Be sure to add a documentation comment for your new method, like the one that appears just above `sheppeysToMiles`, formatted exactly the same way, but with the text updated so it correctly describes your new method.
   - Make sure you have a test for your new method, like the existing `testSheppeysToMiles`, with the name and the numbers changed as appropriate.
   - Make sure your new test passes!
   - Make sure _all_ methods and variables in your new code have accurate names.


## Task 2: Cash Chaos

> **Note:** It it best to do this portion of the assignment **after** the Unit Testing in-class activity. If you started early on the homework, (1) nice!! and (2) wait for that activity before getting too far into this part of the homework.

`MoneyCalculator` contains code that breaks a given amount of money into US cash currency. The best way to see what it does is to run it: open up `MoneyCalculator.java`, which has a `main` method, and run it. Type in some amount of money. (You might have to click in the Terminal pane to be able to type.) Watch what the program does with the input you give it. Play with it until you think you understand what this code is supposed to do.

Unfortunately, `MoneyCalculator` is full of bugs.

**Play with it for a while**, and see if you can find some dollar amounts for which it gives the wrong answer. **Don’t fix anything yet!** Just do some manual testing, and see if you can find any of the bugs. If you notice a particular amount that gives the wrong answer, write it down for later.

Once you understand it, open `MoneyCalculatorTest`. Run all the tests. They all pass! (How can the tests all pass if there are bugs? Why don’t passing tests guarantee bug-free software?) Study the structure of the test code. Notice how it uses Java’s flexible formatting rules to spread the code out and make it easier to read.

### Task 2a: Understand the code

Take a look at the code for `MoneyCalculator`. Scroll through it. Yikes.

You are in a situation that is very, _very_ common for software developers:

- There is a lot of code.
- You didn’t write it. (Or maybe you did, but you forgot how it works.)
- You want to fix it, change it, or add to it.
- Before you can do that, you have to _understand_ it.

Throughout this class, we will often give you existing code that you have to understand and change. This is not just training wheels for beginners; this is _what software development is like_.

Find the `convertToCash` method. Do you notice the repeating pattern? Somebody did a _lot_ of copying and pasting. (That’s going to be a problem.) Look at the first instance of the repeating pattern in `convertToCash`. Good news: it has a few helpful comments. Think through it _line by line_, and try to understand what it’s doing and _why it works_.

Here are some hints (click to expand):

<details>
<summary>
Hint about `(int) (amountRemaining / 100)`
</summary>

The `(int)` is a type cast. It forces the result to be an int, rounding down if necessary.

What is the type of `amountRemaining`? What therefore is the type of `amountRemaining / 100`?

Consider a specific example. Suppose `amountRemaining` is 768.0. What is `amountRemaining / 100`? But what is `(int) (amountRemaining / 100)`?
</details>

<details>
<summary>
Hint about `hundreds == 1` and `hundreds != 0`
</summary>

There is a comment that says “Singular, plural, or none?” in the code. It is describing three different situations the code needs to handle. What are those situations?

Think: Which path through the if-else chain does the code take in each one of those situations?
</details>

<details>
<summary>
Hint about `amountRemaining %= 100`
</summary>

The `%` operator is the modulus operator. It means “remainder after
division” (with some special rules about negative numbers). For example,
`17 % 3 == 2` because if you divide 17 by 3, you get 5 with a remainder
of 2.

If `%` is an operator, then what is `%=`? What is `amountRemaining %= 100` a shortcut for? Think, then click to reveal: <hidden>`amountRemaining = amountRemaining % 100`</hidden>

What is `768.0 % 100`? Think, then click to reveal: <hidden>68.0</hidden>

Think: Why is that useful? Why do we assign it back to `amountRemaining` for the next step?
</details>

Make sure you understand the existing code. If it is confusing, **ask for help** from a preceptor or your instructor before continuing.

### Part 2b: Fix the first bug

Run the main method in `MoneyConverter`, and input 11 dollars. It _should_ print this:

    1 ten
    1 one

But what _does_ it print?

See if you can find the bug, but **don’t fix it yet**. Just find it.

<details>
<summary>Hint about where the bug is
</summary>

Look at the section of the code that deals with fives. There is a mistake in the repeating pattern. Looks like the person who was copying and pasting that pattern missed a spot.
</details>

Notice that **repetitive and verbose code can hide bugs**. This is one of many reasons why copying and pasting big chunks of code is usually a bad thing.

Before you fix this bug, add a new `assertEquals` somewhere in `MoneyConverterTest` that tests the 11 dollar case. (You might add that to an existing `@Test`, or you might create a new one. Either is OK. Pick one that makes sense to you.) Make your test expect the correct answer. Run the tests again, and **make sure that your new test fails**.

Now fix the bug. **Make sure the tests all pass now.**

Commit your progress with Git.

### Part 2c: Finding the second bug

What should the answer be for $0.29? Think, then manually test it: run the main method in `MoneyConverter`, and input 0.29.

Huh?!

If you’d like, take a minute to make a guess what’s going on. You can add a print statement, or even find the source debugger in VS Code and step through the code. (We’ll look at the debugger in class at some point. Don’t worry if you don’t know about it yet.)

Once you have a guess, or you’ve explored a bit, or you just can’t even imagine where to start, here is the answer:

<details>
<summary>
An explanation of what the bug is
</summary>

It’s a floating point thing. The type of `amountRemaining` is `double`. Floating point numbers are approximations, and subject to surprising rounding errors. Because of this, you should **never use floating point types to perform monetary calculations that need to be precise** — including calculations about money!

With `0.29`, in the quarters step, when Java computes `0.29 % 0.25`, the result is not `0.04`; it is `0.03999999999999998`. Because of that, it gets the wrong answer in the pennies step.

The exact reason _why_ this happens are beyond the scope of COMP 127. For now, it is sufficient just to say that **we shouldn’t be using floating point math to do currency calculations**.
</details>

OK, given that, how could we fix this bug? Think for a moment then read about…

<details>
<summary>The solution we’ll use in this homework
</summary>

Instead of using a floating point type for doing currency computation, we'll use an integer type.

How? Instead of computing with dollars, we’ll compute with cents. For example, a dollar would be represented as 100, and $3.50 would be represented as 350.
</details>

That can work! But…ouch. Because `convertToCash` has such repetitive code, we are going to have to make that change over and over and over. And if we mess up even once, it could create an insidious bug that is hard to find — just like in part 2b.

A better approach: don’t do that! Instead, here is a plan for what we’re going to do. (Not yet! This is a plan for **all the steps to come**, not what you should do right away!)

- First you will **refactor** the existing code to remove the repetition. It will still use `double`, still do exactly what it does, but it will become much more concise, and much easier to make this change.
- You will then add a test for $0.29, and make sure it fails as expected.
- You will change the code to use an integer type.
- Finally, you will then make sure the new test passes with the change to integers.

That’s a lot. Let’s take it step by step.

### Part 2d: Create the helper function

We are going to extract the repeating pattern in `convertToCash` into a new helper function, `computeDenomination`. Then, instead of an almost-repeating chunk of code, `convertToCash` will become a series of calls to the same function. (If you took COMP 123 at Mac, you might have done the “Row, Row, Row Your Code” activity. This is very similar.)

Look at the repeating pattern in `convertToCash`. Think about what parameters the helper function is going to need. What changes between each repetition? What stays the same? What changes are just changes to variable names, versus changes to data / constants? What variables can be _local_ variables for the function? What variables need to live on between calls to the function?

Here are two hints:

- You can return the updated value of `amountRemaining`.
- You can pass in `result` as a parameter every time you call the function, so that the function can add to it.

Make a list of parameters you think you will need, and the method’s return value.

These are all tricky questions! Think about it, come up with a list, _then_ check your answer:

<details>
<summary>
Parameters and return value for `computeDenomination`
</summary>

Parameters:

- The amount of the currency denomination (100, 50, etc.)
- The denomination’s singular name ("hundred", "penny", etc.)
- The denomination’s plural name ("hundreds", "pennies", etc.). Note that because some of the plurals are irregular, you cannot just append "s" to the singular name.
- An amount of money
- The list of result strings (to which the function may add a new one)

Return value:

- The new amount remaining
</details>


What are the **types** of all those parameters? (Remember, we are sticking with `double` for now: first refactor, _then_ fix the bug.)

Add a method declaration for your new `computeDenomination` helper function. It should go _after_ the closing brace of `convertToCash`. You can use `convertToCash` as a model for method declaration syntax, with one change: make `computeDenomination` a `private` method (instead of `public`), because it is only for use inside of `MoneyCalculator`.

Try moving the code for _only_ the hundreds into your new helper. Not everything! Just the hundreds. Use the parameters instead of constants like `100` and `"hundred"`. Make sure that there is _nothing_ left in your function that specifically refers to hundreds, _including variable names_.

Once you’ve done that, run the tests. They should still all pass.

Commit your work.

### Part 2e: Goodbye, repetitive code!

Now…

**🌈🦋🦄 ¡¡¡THIS IS THE BEST PART OF THE ASSIGNMENT!!! 🦄🦋🌈**

…replace _all of the repeated code_ with calls to `computeDenomination`. You get to _delete a whole lot of code_. Enjoy!! Deleting code is one of the best parts of programming.

When you are done, all the tests should still pass.

Commit your work.

### Part 2f: Fix the bug

First: add a test for $0.29. **Make sure this new test fails.** Our tests now check for the bug.

Now, at last, we can fix the floating point bug by switching to an integer type. Remember that primitive types have maximum values. If we use `int`, then the code will break for any amount over $21,474,836.47, so let’s use `long` instead. (It’s good up to $92,233,720,368,547,758.07. Seems like that should suffice.)

Find every instance of `double` in `MoneyCalculator`, and replace it with `long`. Pay attention as you go! Don’t miss anything!

- You are working with cents now, so there are some numeric constants that need to change.
- There is one message to the user that will need to change, to prompt them to enter cents instead of dollars with fractions.
- There are comments that need to change too.

You will also need to update the tests, which now need to provide cents instead of dollars as inputs. Once you change that — and _only_ that — in the tests, they should all pass again.

Commit _and push_ your work.

Congratulations! You just made some software better.


### Acknowledgements:


## Submitting your assignment

Please follow the [homework instructions](https://comp127.innig.net/resources/take-home-procedure/) on the course web site.


## Acknowledgements


This is based on problem PP2.9 on page 71 in _Java Foundations_ (Lewis / DePasquale / Chase, 4th edition), reworked by Paul Cantrell to focus on refactoring and floating point math.

Bret Jackson added the bit on Sheppeys. :stuck_out_tongue_winking_eye:
