---
name: "Concurrency, Part 1"
repo: https://github.com/mac-comp127/two-kinds-of-concurrency
original_url: https://docs.google.com/document/d/1At7nbmvANlJHo6dzu0fcSwhRb4KGRGWKhCNP-rec6Nc/edit?usp=sharing
---

# Two Kinds of Concurrency, Part 1: The Fast and the Dangerous™


{:standard_toc}


In software, <def>concurrency</def> is when **multiple tasks have overlapping timespans**.

<def lowercase>Concurrent execution</def> is the opposite of <def>sequential execution</def>. When code runs **sequentially**, one task finishes completely before the next task starts. You are already used to sequential execution: when you write code, each statement finishes its work before the next statement starts. If you put a loop in your code, the program waits for the loop to finish before moving on to the next thing. If you call a function, the program waits for the function to finish before moving on to the next thing. Code in Java (and Python, and most other common programming languages) is fundamentally sequential.

When code runs **concurrently**, one task or operation starts _before others are finished._ Why would you do that? And how does it work?! There are multiple answers — very, very different answers! — to both of those questions. In this two-part activity, you will look at two alternatives.


## Set up a performance test

Take a look in the `paralleldanger` package. The `SisyphusStream` class has a main method with this ridiculous stream:

    LongStream.range(0, 100L)
        .map(n -> 1)

What does this stream do? <hidden>It gives you the number 1 repeated 100 times.</hidden> Why would we want that?! Because we’re going to use it for performance testing. (The `L` in `100L` means “long.” It is there so we can make that number get much, much bigger later on.)

Java streams have a `forEach` method that calls a closure with each element of the stream. Note that this is different from `map`. The `map` closure _must_ return a value, and then `map` gives you a new stream that contains the transformed values. However, `forEach` is a _terminal_ operation: its closure does not return anything, and it does not give you a new stream. It passes the closure you provide each element in turn, then does nothing else with the values.

Let’s use `forEach` to print each element in the stream, and see the number 1 repeated 100 times. Exciting! (Not really, but testing our code _is_ always exciting.) Update the main method:

    LongStream.range(0, 100L)
        .map(n -> 1)
        .forEach(System.out::println);

Run that and make sure it works.

Our goal is to compute the sum of the numbers in the stream. It should sum to 100, of course! (This is a nice task for performance testing: the correct answer is obvious, but it will still take some amount of time to compute.)

Take a _quick_ look at the `SumCalculator` class. Then update the main method to use it to compute a sum:

    SumCalculator calc = new SumCalculator();
    LongStream.range(0, 100L)
        .map(n -> 1)
        .forEach(calc::add);  // Add each steam element to the total
    System.out.println(calc.getTotal());  // Print the total

Note the new `forEach` call. What is it doing? What method does it call? What instance variable does that method modify? <hidden>It calls <code>calc</code>’s <code>add</code> method, which modifies the <code>total</code> instance variable of <code>calc</code>.</hidden>

OK, let’s start timing! The project also contains a `Timer` class for doing performance testing. You do **not**  need to read and understand it now, although you are welcome to study it after class. Here is how to use it:

    Timer.measure(20, () -> {  // Repeat the following test 20 times:
        SumCalculator calc = new SumCalculator();
        LongStream.range(0, 100L)  // Process this many numbers
            .map(n -> 1)
            .forEach(calc::add);
    });

Copy that code into your main method, and run it. Study the output. Everything make sense? Grab an instructor or preceptor if you have questions.

Now the fun begins! **Add one zero at a time to 100L until the average rep is longer than 0.5 seconds.**  How many zeros did you have to add? How fast is a computer, anyway?! Yikes!

<callout>
  **Aside:**  Why are the subsequent repetitions so much faster than the first ones? **Read on if you’re curious; skip ahead if time is running short.**

  Java runs your code in a sort of “quick start, slow speed” mode when your program first begins. Java then observes where your code is spending most of its time, and plans out faster ways of running the particular code that is causing performance problems

  This strategy is a part of Java’s **JIT**  — “**J**ust **I**n **T**ime” — compilation strategy. To “compile” code essentially means “prepare the code in advance for running.” Here, the “just in time” means that Java waits until your code is just about to start running before fully compiling it. And then — here’s the key part! — Java may compile it _again_ in a different way, after it’s already started, based on what is actually happening.

  For this reason, Java code is often slow at first, then much faster. Keep this in mind when you do performance testing! This is one reason why `Timer.measure` does many repetitions and takes the average.
</callout>

**Write down the average time from your test.** Now that we have something slow, we’re ready to make it faster.

## Let’s make it fast! What could possibly go wrong?

Did you know that most modern computers, probably including _your_ computer, can do _multiple things simultaneously?_ This is called <def entry="parallelism">parallel execution</def>, or just <def>parallelism</def>. “Parallel” doesn’t just mean different tasks taking turns while they work; it means different tasks happening _at the same time_. (When you hear computer advertisements talking about a “4 core processor! 8 core processor! etc,” this is the thing they’re referring to: how many things the computer can do truly simultaneously.)

Java streams have an amazing feature: you can tell them to run in parallel! And it only takes only one line of code:

    Timer.measure(20, () -> {
        SumCalculator calc = new SumCalculator();
        LongStream.range(0, 1000000000L)
           .parallel()  // Magical fairy dust here
           .map(n -> 1)
           .forEach(calc::add);
    });

Run your code again. If you have a multi-core computer, it will be faster! (How much faster? Compare to your average time from before.)

Amazing! Just like magic!

But…hmmmmm, we’re not actually printing the total anymore. We really ought to check that we’re still getting the right answer. Add a line to print the total for each repetition:

    Timer.measure(20, () -> {
        SumCalculator calc = new SumCalculator();
        LongStream.range(0, 1000000000L)
           .parallel()
           .map(n -> 1)
           .forEach(calc::add);
        System.out.println("total=" + calc.getTotal());  // Just to be sure
    });

![](images/image1.jpg)

If your computer has a single core, you will see the correct answer. But if you have a multi-core computer — which most computers these days are — you will see some very puzzling results.

Wait, was it ever correct? Comment out the `.parallel()` line and watch your code succeed slowly. Uncomment the `.parallel()` line and watch your code fail quickly.

## What went wrong?

The problem lives in this innocent-looking line in SumCalculator:

    total += x;

That line of code does _three_ different things:

1. **Read**  the current value of `total`.
2. **Compute**  a new result.
3. **Write**  the new result back to `total`.

When we made the stream run in parallel, we made it so that multiple calls to the `sum` method are doing those three steps _simultaneously_. Suppose that `total` is currently 999. If we’re lucky, very lucky, then this happens:

| Parallel worker 1   | Parallel worker 2     |
|---------------------|-----------------------|
| Read total → 999    |                       |
| Add 1 to 999 → 1000 |                       |
| Write 1000 → total  |                       |
|                     |  Read total → 1000    |
|                     |  Add 1 to 1000 → 1001 |
|                     |  Write 1001 → total   |

But sometimes we’ll get unlucky, and something like this happens:

| Parallel worker 1   | Parallel worker 2    |
|---------------------|----------------------|
| Read total → 999    |                      |
|                     |  Read total → 999    |
| Add 1 to 999 → 1000 |                      |
| Write 1000 → total  |                      |
|                     |  Add 1 to 999 → 1000 |
|                     |  Write 1000 → total  |

…and then we get the wrong answer.

This is called a <def>race condition</def>: the correctness of a computation depends on the exact timing of concurrent tasks. The two workers here are in a <def>data race</def>, each one racing to write its result back to `total` before the other can read it.

The fundamental source of trouble here is that even though there are multiple parallel workers, they are all sharing the _same_ instance variable of the _same_ `SumCalculator` object:

    private long total;

The term for this is <def>shared mutable state</def>: “shared” because multiple things all use it, “mutable” because it changes. Shared mutable state is a common source of bugs in many situations. When parallelism is present, it can be deadly.

<details>
<summary>
**On effectively final variables, lambdas, closures, and race conditions**
</summary>

Were you confused by the "effectively final" concept when you learned about lambdas / closures in Graphic Calculator? One of the reasons the Java language designers created that concept is because of race conditions.

Imagine code like this:

```
int sum = 0;
SumCalculator calc = new SumCalculator();
LongStream.range(0, 1000000000L)
    .parallel()  // Magical fairy dust here
    .map(n -> 1)
    .forEach(i -> sum += 1); // looks innocent enough, but will not compile
```

The idea there is that the closure will capture `sum`, and every time the lambda is called, it will add 1 to `sum`. If the stream has 1000 elements, the lambda gets called 1000 times, adds 1 to sum 1000 times, and you get 1000. Right?

The exact same kind of race condition as above would happen here:

1. Parallel worker 1 reads `sum`.
2. Parallel worker 2 reads `sum` -- getting the same value!
3. Worker 1 writes back the incremented value of `sum`.
4. Worker 2 writes *its* incremented value of `sum`, which is the same as what worker 1 just wrote! We processed two values, so `sum` should increase by 2, but it only increases by 1.

This race condition is one of the things that motivated the language designers to forbid the above code and come up with the concept of "effectively final". In fact, the language designers [wrote about this while working on adding lambdas to Java](https://cr.openjdk.org/%7Ebriangoetz/lambda/lambda-state-4.html) -- see section 7, variable capture.

</details>




## How do we fix it?!

Our simple, innocent-looking `SumCalculator` class is the source of our trouble. It isn’t built for parallelism. (The term people often use for this is that it “isn’t <def>thread-safe</def>.” In this context, “thread” is a term for the abstraction the computer uses to manage parallelism, and even simulate it on single-core computers.)

Fortunately, we don’t need `SumCalculator` at all: Java _already provides a `sum` method for streams_. And unlike the `SumCalculator` class, it knows how to handle parallelism:

    Timer.measure(20, () -> {
       long total = LongStream.range(0, 1000000000L)
          .parallel()
          .map(n -> 1)
          .sum();  // Instead of using SumCalculator
       System.out.println("total=" + total);
    });

Run it. Look at those timings! Look at those correct results!

How precisely does Java avoid the pitfalls of shared mutable state and make that `sum` method work? How do parallel algorithms work in general? It is a complicated and _fascinating_ topic, so complicated and fascinating that we devote an entire course to it (COMP 445: Parallel and Distributed Processing). These are not questions we answer in COMP 127. However, there are still lessons we can learn right now.

## Lessons from this adventure

- Parallel computing can make things much **faster**. It is amazing!

- Parallel computing can also make code break. It is **dangerous**.

- The problems that parallelism causes can be **surprising**  and **hard to spot**. Do not assume your code works just because it looks right and you’ve thought about it very hard.

- **Measure** performance. Don’t introduce danger or complexity to make things faster until you know you actually have a problem.

  <callout>
    “Programmers have spent far too much time worrying about efficiency in the wrong places and at the wrong times; premature optimization is the root of all evil (or at least most of it) in programming.”

    — [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth), 1974
  </callout>

- If you don’t need parallelism, **don’t use it**.

- If you _do_ need the performance benefits of parallelism, **let a library handle it for you**  whenever possible. Let somebody else’s well-engineered code do the tricky stuff!

  Don’t get clever when something simple and obvious will do the job.

- If your own code _does_ need to exist in a context where there is parallelism, remember that you are walking on lava, and **beware of shared mutable state**.

  Beware the following words that warn you that you might be in the danger realm:

  - **parallel**
  - **thread**
  - **multitasking**
  - **distributed**

## Onward

Get up! Stretch! Then proceed to [part 2](/activities/concurrency_2).
