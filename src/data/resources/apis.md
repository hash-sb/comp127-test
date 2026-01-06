---
display_mode: article
original_url: https://docs.google.com/document/d/1TP8kAjTfMeqN34U5SIo8Zh5H0hOJVSbFOgtaSXH4CJk/edit
cleanup_needed: true
---

# APIs


{:standard_toc}


## What is an API?

Suppose you have a piece of code that uses a `GraphicsGroup` to make a face:

    GraphicsGroup face = new GraphicsGroup();

    face.add(leftEye, 10, 30);
    face.add(rightEye, 60, 30);
    face.add(mouth);
    canvas.add(face);

Now suppose the authors of the Kilt Graphics library make changes to the code for `GraphicsGroup`. If you use the new, updated version of Kilt Graphics, will your face-making code continue to work? The answer to that question depends on what they changed about `GraphicsGroup`.

If they added a new `getTotalArea()` method without changing any of the existing methods, then your code will work fine: that method is new, so old code never used it! Perhaps they improved the performance of `add()` so that it works 30% faster, but does exactly the same thing. That too will not break existing code (unless the existing code somehow _relied_ on `add()` being slow, which is possible!).

However, if they removed the `add()` method, or renamed the class to `GObjGrouping`, or deleted the class altogether, then your existing code would break. Each of these is an example of what we call a <def>breaking change</def>.

There are some changes that are _internal_ to `GraphicsGroup`, and other changes that are _externally visible_ because they could affect other code that uses `GraphicsGroup`. This distinction matters. In this class, we are concerned with creating abstractions so that different components of software can change somewhat independently. However, abstractions are only useful if we have an idea of which changes are breaking changes.

It is the responsibility of the <def>provider</def> (or “vendor”) of a library like Kilt Graphics to think about what kinds of changes they are making, and whether those changes are externally visible or purely internal. It is the responsibility of <def term="client">clients</def> of Kilt Graphics to learn what it provides, what promises it makes, and what assumptions are safe for client code to make. All this in turn requires some mutual agreement between the provider and the clients of Kilt Graphics about what everyone should expect.

That agreement is called an **API**. (It stands for “application programming interface,” but hardly anybody spells that out, even in formal writing. It’s just “API.”) This is an _extremely_ broad term. Here is a definition:

<definition-callout>
  An <def>API</def> is a set of expectations that one piece of code (the “API provider”) fulfills so that other code (“API clients”) can interact with it.
</definition-callout>

Those “expectations” might include:

- What kind of communication mechanism to use
- What operations or actions are available, and what their names are
- What types or data formats those operations will use for input and output
- What assumptions a client may and may not make about those operations

In other words, <highlight>**an API is what you can see from the outside of an abstraction.**</highlight>

In the example above, the [Kilt Graphics API](https://mac-comp127.github.io/kilt-graphics/edu/macalester/graphics/GraphicsGroup.html) says something like this: “There is a class called `GraphicsGroup`. It is a `GraphicsObject`. It has a zero-parameter constructor. It has an `add()` method that takes one parameter, which can be any `GraphicsObject`. When you draw a `GraphicsGroup` on the screen, it shows all the objects that have previously been added to it. The most-recently added object appears in the foreground”…and so forth.

Note that the API does **not** say how `GraphicsGroup` works. Does it have a `List` instance variable to track all things that were added? Who knows! Does it use a loop to draw its children? Who knows! Does it have private methods? Who knows! Does it secretly use other classes that are private to Kilt Graphics? Who knows! These things are not part of the API. You, a user of Kilt Graphics, don’t have to care about them. In fact, your code _shouldn’t_ care about those details; if it relies on them, it might break with the next Kilt Graphics update.

## Types of API

There are many different kinds of API. Some APIs are for communication between separate computers:

- A <def>remote API</def> is a protocol for pieces of software to communicate over a network or other physical separation. Remote APIs almost always boil down to rules about reducing all interaction to a sequence of bytes.
- A <def>web API</def> is a particular kind of remote API that uses the technology of the World Wide Web (things like HTTP, TCP/IP, and DNS). When businesspeople who are not programmers talk about “APIs,” this is usually the specific thing they mean. However, in technical circles, the term “API” is much broader.

  (We don’t deal with remote APIs in this class. The Weather Display exercise does use one! However, the assignment provides the code for interacting with that API, so you don’t need to learn how it works. You may learn about web APIs in COMP 225 and COMP 446, and some of the data science courses as well.)

Some APIs are for communication between different pieces of code running on the same computer:

- An <def>inter-process API</def> allows separate programs on the same computer to share data. The file system is the most familiar example of this: many programs can read and write the same file, because the computer’s operating system provides a common API for all of them to interact with those files. There are other inter-process communication mechanisms too, such as pipes and sockets. You might get a taste of this in COMP 240.

- A <def>library</def> is independent code meant for other software projects to use directly, not communicating with another computer or another program, but within the same program. For example, Kilt Graphics is a library. The Java Collections Framework that provides `List` and `ArrayList` is also a library. **Every library provides an API**, either explicitly or implicitly.

  Unlike remote and inter-process APIs, which tend to be language-independent, this kind of API is language-specific: it exposes its capabilities to clients using the features of one particular programming language. For example, a Java library provides…

- an <def>object API</def> that specifies a set of public classes and their public and protected methods. This includes not only the signatures (names, parameter types, and return types) of the methods, but also expectations: what a client needs to do to use the classes and their methods, and what the library classes promise (and don’t promise) to do.

  In fact, a piece of code doesn’t even need to be a library to provide an API. In a large project, we might even choose to imagine that one part of our own program provides an API to other parts, acting as our own unofficial library provider. This relationship can even exist between individual objects.

In this course, we will specifically be working with **object APIs**. However, most of the underlying principles in this document apply to the other kinds of API as well.

## What’s the point of APIs?

APIs help pieces of code change and evolve independently — and thus helps humans work independently but effectively.

We say that two components of a system are <def>tightly coupled</def> when changes to one are likely to require changes to the other, and are <def>loosely coupled</def> or (better yet) <def>decoupled</def> when it is easy to change one independently of the other. (These terms come from physical engineering disciplines: Two parts of a physical object can be coupled — joined, attached — tightly together with, for example, welding or glue, so that it is hard to replace or modify one without replacing the other. Conversely, parts can be “loosely coupled” or “decoupled” so that one can be replaced without necessarily replacing the other, e.g. two gears that mesh but are not attached.)

There are three important kinds of decoupling an API can provide:

- **Clients can change without changing the provider.** For example, you can add a custom brush in your Painter app without having to change the Kilt Graphics library.

- **New clients can appear without changing the provider.** For example, you can create a new Java project that uses Kilt Graphics without having to change Kilt Graphics itself. (This may seem self-evident, but it was in fact a [tremendous breakthrough in the early 1950s](https://www.google.com/url?q=https://www.computinghistory.org.uk/det/5487/Grace-Hopper-completes-the-A-0-Compiler/&sa=D&source=editors&ust=1757905915375468&usg=AOvVaw2VgyBpWV86jVwsA5Ug6VHR) when it became possible for new code to use existing libraries without someone having to wire all the code together through manual effort. This was one of several things that made Grace Hopper famous.)

- **The provider’s implementation can change without changing clients.** For example, Kilt Graphics could change how `GraphicsGroup` stores the objects previously added to it, but as long as the class _behaves_ the same way — as long as the change is not a **breaking change** — you wouldn’t have to change your critter code.

Here are some examples of decoupling paying off:

- You can use Java’s existing `String` class to write a new program, despite the fact that your project didn’t even _exist_ when they made the `String` class.
- Java’s maintainers can change how `String` is implemented (as they have, many times before), but your code still works.
- A search engine can update their search ranking algorithm (as they often do), but you can still use the search engine exactly as you used it before.
- You can update your web browser, but the same web sites that used to work for you still work. Probably. (It is in fact sometimes in the interest of businesses to create tight couplings between different pieces of software their customers / users rely upon, creating **lock-in** that prevents people from migrating to competing alternatives. This is one of the reasons Google makes its own web browser, Chrome, and gives it away for free: it is to Google’s advantage for there to be sites that only work properly in Chrome. It is why Apple has been stubbornly reluctant to create an API that would let Android phones handle the reaction emojis from the iPhone Messages app.)


