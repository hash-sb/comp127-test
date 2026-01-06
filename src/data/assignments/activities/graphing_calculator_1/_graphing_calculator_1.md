---
name: "Graphing Calculator, Part 1"
repo: https://github.com/mac-comp127/graphing-calculator
original_url: https://docs.google.com/document/d/1wLaS0PX1LKwEgiwMAFYVoITLA4Y4VIm2Qe_A9Rolqjo/edit?usp=sharing
title_image:
  url: images/image1.png
  alt_text: "Many overlapping curves showing the shape of a variety of mathematical functions"
---

# Graphing Calculator


{:standard_toc}


## Learning goals

The goal of this activity is to learn about closures (also known as “lambdas”) and their many syntactic variations in Java.

You will use lambdas to plug functions into a “graphing calculator” class that we give you.


## Get set up

- Open the `GraphingCalculator` class.
- Find the main method at the bottom of the class.
- Run the class. You should see two thin grey lines in an otherwise blank window.

## Simple lambdas: single expression, one parameter

This class plots functions, like a graphing calculator. Try adding this line to the main method, and run the program again:

<highlight><code>calc.show(x -&gt; x * x);</code></highlight>

You can plot multiple functions in the window, and it will automatically give them different colors. Try adding more calls to `show()` to add a few simple one-line functions of your own.

The parameter to `show()` is a <def entry="closure">lambda</def>, also known as a <def>closure</def>. A closure is a piece of code that can be passed around as a value and run in the future — possibly multiple times, possibly not at all! Usually if you said `x * x`, then Java would immediately look for a variable called `x` and try to do the multiplication. But here, you are passing that calculation to the graphic calculator and letting _it_ decide whether and when to run it, and what value to provide for `x`.

Note that the _whole closure_ is just _one value_. This is a tricky detail. In this code, we are passing _one_ argument to the `show` method:

    calc.show(x -> x * x);

…and that _one_ argument is `x -> x * x`. All of that, the whole closure, is a _single_ value, in the same way that `17` and `"fish"` and `new CanvasWindow()` are each a single value:

| This expression…     | …evaluates to…     |
|----------------------|--------------------|
| `17`                 | one `int`          |
| `"fish"`             | one `String`       |
| `new CanvasWindow()` | one `CanvasWindow` |
| `x -> x * x`         | one closure        |
{:.compact}

Closures are **functions passed as values.**

Approximately how many times does the calculator run that closure? In other words, about how many times does it do the `x * x` computation to produce a plot? Once? Twice? A lot? That’s not up to you; it’s up to the `GraphingCalculator` class! A closure creates a **separation of concerns**  between **_what_ a piece of code does and _when_ that piece of code runs**. In this case, you decide what the code does (it squares `x`) and the calculator decides when it runs (over and over with different values of x, to create the plot). `GraphingCalculator` doesn’t control what the code in your lambda does, and you don’t control when it runs.

What happens if you give the lambda’s parameter a name other than “`x`”? Try it and see:

    calc.show(zargle -> zargle * zargle);

Did it make a difference? Why or why not? **Discuss with your partner.** Partial answer: <hidden>Changing the formal name of a parameter in a <em>method</em> doesn’t affect the caller. The same principle applies to a closure.</hidden>

## Method reference lambdas

You can plot a cosine wave like this:

    calc.show(x -> Math.cos(x));

Try it! Note that your lambda takes one double as a parameter and returns a double — and `Math.cos()` _also_ takes one double and returns a double. That lambda and the `cos()` method happen to have the same signature! Java provides a nice shortcut for this:

    calc.show(Math::cos);

This syntax essentially means “pass this whole method as a lambda.” Try it.

Try some other single-parameter static methods in `Math` that would work with this syntax.

## Two-parameter lambdas

The graphing calculator can animate equations. If you pass `show()` a lambda with two parameters instead of one, the first parameter is still x, and the second parameter is one that will vary over time to create an animation. Here is a fun one to animate:

    calc.show((x, n) -> Math.atan(x / Math.sin(n)));

Note that to pass two parameters to a lambda, you have to put parentheses around them.

**Invent another animated function with your partner**. Have a little fun with it!

## Method reference lambdas again

Does the method reference syntax work with more than one parameter? Yes! `Math.pow()` takes two doubles and returns a double, and that is exactly what the animated variant of `show()` expects, so instead of this…

    calc.show((a, b) -> Math.pow(a, b));

…you can do this:

    calc.show(Math::pow);

## Zero-parameter lambdas

As with two-parameter lambdas, you also need to provide parentheses for a _zero_-parameter lambda. (You can even put parentheses around a single lambda parameter, but people usually don’t.)

In the `GraphingCalculator` constructor, find the call to `canvas.animate()`. The `animate()` method will call the given lambda over and over while your program is running, which allows you to produce animations.

Try to understand the syntax of that call. Where does the lambda begin, and where does it end? **Discuss with your partner.**

## Multi-statement lambdas

So far, all the lambdas we have seen consist of a single expression. You do not have to stick with single expressions, however; you can put as many statements as you like inside a lambda. To allow multiple statements, two things change:

- You need to use curly braces.
- You need to explicitly say “return.”

In other words, a multi-expression lambda looks a lot like the body of a method.

For example, the original function call you added:

    calc.show(x -> x * x);

…is shorthand (also called “syntactic sugar”) for this:

    calc.show(x -> {
        return x * x;
    });

Note that the whole closure is _still just one value._ In the example above, we are still calling the `show` method with just one argument. The closure now spans three lines of code, but it is still a single closure!

This syntax lets the graphing calculator plot a function that requires a complex multi-statement computation — even a loop! Try this:

    calc.show((x, n) -> {
       double result = 0;
       for (int i = 1; i < 20; i++) {
           result += Math.sin(x * Math.pow(3, i) + n * i)
                      / Math.pow(2.5, i);
       }
       return result;
    });

Run that code and see what happens. (You can comment out some of your other functions if the plot is getting too messy.)

How many arguments are you passing to the `show` method _now?_ <hidden>Still just one.</hidden> Where does the closure begin and where does it end? **Discuss with your partner.** Answer: <hidden>It begins with <code>(x, n) -&gt;</code> and ends with the last curly brace, the one on the final line just before the final parenthesis.</hidden>

Try creating your own plot with a function that requires a multi-statement computation.

## Variable capture

One of the very interesting features of lambdas is that in addition to their own parameters, they can also see any variables that are in their enclosing scope. Here, for example, the code inside the lambda not only sees `x` and `t`, but also `n`:

    int n = 3;
    calc.show((x, t) -> Math.sin(n * x - t * 10) / n);

This is called <def entry="capture">variable capture</def>. We say that the lambda has “<def term="capture">captured</def> `n`”. A lambda can use this to tie its own logic to its surrounding context. This turns out to be quite powerful, as we will see later this week.

There is, however, a catch: in Java, a captured variable must be “effectively final,” which means that it is only assigned to once. Suppose, for example, that we wanted to take the previous function and add it to the calculator many times for different values of `n`. We might try this:

    for (int n = 0; n < 12; n++) {
       calc.show((x, t) -> Math.sin(n * x - t * 10) / n);
    }

…but sadly, Java does not allow it, because the `++` changes the value of `n` over and over. Paste the code in and see the error message for yourself.

How can you fix this? You need a variable that:

- takes on each value of `n` as the loop iterates, but
- is only assigned to once after being declared.

Discuss with your partner. (It’s tricky because the solution feels like cheating!)

<callout>
  Please note that **the code above should plot 12 _different_ functions**  all at once. If it doesn’t, then you haven’t figured it out yet!
</callout>

If you’re not sure what to do, here is a sketch of the answer: <hidden>Declare a <code>final</code> local variable <em>inside</em> the loop that is initialized to <code>n</code>.</hidden>

## Invent your own

You have all the tools you need. Come up with some nifty functions of your own. Be creative and go wild!
