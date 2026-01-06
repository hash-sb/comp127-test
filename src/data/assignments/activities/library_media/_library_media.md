---
repo: https://github.com/mac-comp127/library
title_image:
  url: images/image1.png
  alt_text: "A lavish library with books lining the walls, with elegant woodwork and domed ceilings"
original_url: https://docs.google.com/document/d/15e-bx95dAQw03Ej47CIXNlzL-4e715xu4asnIKMmnfY/edit?usp=sharing
---

# Library / Media

([image credit](https://en.wikipedia.org/wiki/Library%23/media/File:Biblioth%25C3%25A8que_de_l'Assembl%25C3%25A9e_Nationale_(Lunon).jpg))


{:standard_toc}


## Learning goals

In this activity, you will learn how you can use **Java interfaces** to create **polymorphism**, allowing objects within the same `List` to have different behaviors.  After completing this activity you should be able to:

- Create a new interface
- Write classes that implement your interface
- Make different classes under the same interface display different behavior
- Start to recognize polymorphism and the problems it solves


## What problem are you solving?

Run the `LibraryMain` class. Observe how the program behaves. You will preserve that functionality as you work on this activity, adding new features without altering existing ones!

Here’s our problem: we want our library to be able to hold and check out things other than books. For now we are adding videos; in the future, we might add even more kinds of items. This goal collides with our existing code, posing some <highlight>vexing questions</highlight>:

- Books and videos do not have the same metadata. For example, books have _authors_ while videos have _directors_. <highlight>How do we have different fields for different items??</highlight>
- We are going to have different rules — and thus different code — for checking out books versus videos. <highlight>How do we keep the `checkIn` / `checkout` code from getting too complicated to read and maintain??</highlight>
- The code in `LibraryMain` specifically mentions `Book` in many places, most crucially in the various `library` variables whose type is `List<Book>`. <highlight>Can we have something like a `List<Book or Video or maybe other stuff too>`??</highlight>

### A terrible solution

Here’s a really bad idea: we could expand the `Book` class to turn it into `BookOrVideo`, and stuff everything we need for both books _and_ videos into one class. We are not going to do this, but think about it for a moment:

- Books and videos do not have the same metadata, so our terrible `BookOrVideo` class would need instance variables for any information we might want about _either_ a book _or_ a video. We would have to leave some of those fields `null` for cetain item types. That means always carefully avoiding using the wrong fields depending on the type of item, so that for example we never have an object with an author _and_ a director.
- Our `checkOut` and `checkIn` methods would grow long and complicated, since they have to handle rules for books _and_ videos in the same place — and those methods would only grow longer if we add yet more types of media.
- How do we know whether a `BookOrVideo` object truly represents a book or a video?

It gets messy fast. Fortunately, there is a better way.


## Create an interface

We are going to create our first interface, called `Media`.

- Create a new interface the same way you would create a new class: right click on the package folder, create a new file named `Media.java`, then use the word `interface` instead of `class` in the source code.

Add three methods to the interface:

- `getTitle`, which has no parameters and has the return type `String`
- `checkOut`, which has no parameters and has the return type `boolean`
- `checkIn`, which has no parameters and has the return type `boolean`

Remember from the reading: when creating an interface, you do not provide an actual implementation of the method. Put a semicolon after the method signature.

Congratulations! You have an interface.

An <def>interface</def> in Java is a description of something that multiple different classes have in common. In this case, what we are saying is, “There is something called `Media` in our world. Many different classes could all be kinds of `Media`. What they all have in common are that they have titles, and you can check them out and check them in.”

||============================================================||

[syntax]      Java syntax
[terminology] What it’s called
[semantics]   What it means

||============================================================||

[syntax]

  interface Media {
    <ghost>...</ghost>
  }

[terminology]

  - <def>interface declaration</def> <meta>(n)</meta>
  - declare an interface <meta>(v)</meta>

[semantics]

  “In our world, `Media` is one kind of thing that can exist. Many different classes could be a kind of `Media`.”

||------------------------------------------------------------||

[syntax]

  <ghost>interface Media {
    …</ghost>
    boolean checkOut();
    <ghost>…
  }</ghost>

[terminology]

  - <def>abstract method</def> declaration <meta>(n)</meta>
  - declare an abstract method <meta>(v)</meta>

[semantics]

  “Every class that is a kind of `Media` must have its out `checkOut` method that takes no parameters and returns a boolean. We aren’t specifying _how_ that method words; there is no implementation here. We are just saying that this method must exist on any class that is a kind of `Media`.”

||------------------------------------------------------------||


This interface directly answers each of the <highlight>vexing questions</highlight> questions above:

<dl>
<dt>
  _How do we have different fields for different items??_
</dt>
<dd>
  Many different classes can all be a kind of `Media`, and each one can have different instance variables!
</dd>
</dl>

<dl>
<dt>
  _How do we keep the `checkIn` / `checkout` code from getting too complicated to read and maintain??_
</dt>
<dd>
  Each different class can have its own separate implementation of those methods.
</dd>
</dl>

<dl>
<dt>
  _Can we have something like a `List<Book or Video or maybe other stuff too>`??_
</dt>
<dd>
  Yes! That is what we are going to achieve in this activity.
</dd>
</dl>


## Implement your interface

For your next task, you will both make the `Book` class implement the `Media` interface and create a new class, `Video`, which implements this interface.

1. First, open up `Book.java`. In the class declaration line, after the name of the class, add:

       implements Media

   Rerun your program and make sure it still works. If you’ve completed Task 1 and added the implements keyword correctly, you should have no errors. The existing `Book` class already matches the `Media` interface you created.

1. Create a new Java class `Video`. Your `Video` class represents video media that the library would like to start stocking—hurray! However, library policy for `Video` is going to be different than policy for Books: the library plans to buy multiple copies of each Video item it will stock. Implement the following behavior:

   - The `Video` class must implement the Media interface.

   - `Video` items are tracked by the following:
     - Title
     - Director
     - Number of copies the library owns

     The constructor for `Video` must set all of these values.

   - To check out a video from the library, you should check how many copies of the video are still available. Checking out should only be successful (return true) if there is at least one copy of the Video still available. A successful checkout removes one copy from being available.
   
   - To check in a video from the library, you should confirm that the library is missing at least one of the copies that they own. If the library already has all of its copies of that Video item, the check in should fail (returns false). A successful check-in adds one copy to the total available.
   
   - The Video class must implement a `toString` method that returns the title, director, and number of copies available for loan.
   
   - The `Video` class must implement a `getTitle()` method that returns the title of the video.

1. Once you have an error-free `Video` class, open `LibraryMain` and make it compatible with all `Media` objects. There are five <highlight>TODO</highlight>s commented throughout the class. Change the code underneath each of these so that it works with `Media` objects in general, not just `Book` objects.

    - Don’t forget to add new `Video` objects to your `library` `List`! You can base your additions off of the sample code that adds new `Book` objects.

1. Rerun `LibraryMain`. You’ve implemented an interface and two classes that use it! Do you see the differences you expect in how `Books` and `Video` are handled?


||============================================================||

[syntax]      Java syntax
[terminology] What it’s called
[semantics]   What it means

||============================================================||

[syntax]

  <ghost>class Book</ghost> implements Media <ghost>{
    ...
  }</ghost>

[terminology]

  - Media is a <def>supertype</def> of Book
  - Book is a <def>subtype</def> of Media

[semantics]

  “`Book` **is a** kind of `Media`. Every `Book` object **is a** `Media` object.”

||------------------------------------------------------------||

[syntax]

  @Override
  <ghost>public boolean checkOut() {
    ...
  }</ghost>

[terminology]

  - <def entry="override">method override</def> annotation <meta>(n)</meta>
  - (In Java, this `@` syntax is called an <def>annotation</def>. `@Test` is another example.)

[semantics]

  “I think that this `checkOut` method matches a method in a supertype. Please give me an error if it doesn’t!”

[commentary]

  We didn’t ask you to type this, but VS Code _might_ have inserted it for you. It’s a safety check, and a good idea. If, for example, you misspell the method as `chekOut`, you will get a helpful error message.

||------------------------------------------------------------||


## Did you notice the magic? You might have missed the magic!

Your `LibraryMain` should now have a little code that looks something like this:

    for(Media mediaItem : library) {
        if(mediaItem.getTitle().equalsIgnoreCase(title)) {
            return mediaItem.checkOut();
        }
    }

Don’t worry if you used a different variable name for `mediaItem`. That’s fine. Focus on this part:

<pre><ghost>
for(Media mediaItem : library) {
    if(mediaItem.getTitle().equalsIgnoreCase(title)) {
        return </ghost>mediaItem.checkOut()<ghost>;
    }
}
</ghost></pre>

Here is the question: **what code runs when that method is called?** The answer is that <hidden>you can’t know. You don’t have enough information to answer that question.</hidden> Why? <hidden>Because you have to know which _specific subtype_ of `Media` this _particular_ `mediaItem` is. It could be a `Book` _or_ a `Video` — the library contains both! — and depending on which one it is, Java will run a different implementation of `checkOut`.</hidden> This magic is called **polymorphism**.

<definition-callout>
  <def lowercase>Polymorphism</def> is when one variable in a program can refer to many **different types** of values, and the same function / method call in the same spot in the code can thus cause **different code** to run at different times. (Computer scientists quibble about the _exact_ definition of polymorphism and which specific things count, but that is the gist of it.)

  The word comes from Greek: _poly_ many + _morphē_ form.
</definition-callout>

In our code, polymorphism is what allows the same library code to manage many different kinds of items.

---

## Bonus challenges

These are some optional additional challenges that you can use to practice using interfaces.

- Add a new media type of your choice to the library: create a class for that media that implements the `Media` interface, and set some rules and unique behavior for how check in or check out works. Some ideas:

  - Rare manuscripts: cannot be checked in or out, but should always be shown as available (for supervised research!)
  - Digital articles: can be checked out limitless times, because they’re available online, never need to be checked in, always available

- Add a new command to `LibraryMain` and corresponding code in your interface and classes that allows users to do something other than check in and check out books.  Some ideas:

  - Preview: This would allow users to see a short description of a title before checking it out.
  - Hold: This would set an item on hold so that it cannot be checked out.


