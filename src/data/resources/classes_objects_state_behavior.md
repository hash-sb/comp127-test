---
original_url: https://docs.google.com/document/d/1yrRr1G7pEp6thgwD41jv9EdtWpC0Sv24B-24zSg-FZk/edit
display_mode: article
---

# Classes and Objects, State and Behavior


{:standard_toc}


The title of this class is “Object-Oriented Programming and Abstraction.” We talked about abstraction on the first day of class, and you’ve seen the word “object” floating around in readings. Today is the day when we finally start putting the pieces together and studying what “object-oriented programming” means.

**This reading involves no code.** The goal here is to get a picture of the big ideas first. Every idea in this reading translates directly into code in Java, and you’ll be learning how over the coming weeks. However, when you are neck deep in the code, it is easy to get overwhelmed by syntax, by bugs, by all the details a programmer has to think about while coding. In such a state, it is easy to get lost. This reading is your map.

Absorb the big ideas in this reading, **learn the vocabulary** — terms that are highlighted <def ignore>like this</def> are terms we use when talking about actual code — and **hang on to these ideas** to help you navigate all the overwhelming details to come.

## State and Behavior

At the heart of Object-Oriented Programming, or “OOP,” are two key ideas: **state** and **behavior**. The best way to understand them is by example.

Consider the door example from the first day of class. Here are examples of things that would be part of the <def>state</def> of a particular door:

* The door is open at a 37° angle.
* The handle is in the rest position.
* The door is unlocked.
* The door is green.
* The door is not latched.
* There is a key inserted in the lock.
* The [tiny little pins inside the lock mechanism](https://en.wikipedia.org/wiki/Pin_tumbler_lock) are raised by 3mm, 1mm, 4mm, etc.

You can think of state as “the current characteristics of a thing at a moment in time,” or “what is currently true about a thing,” or (anthropomorphizing a bit) “what a thing knows about itself.”

Here are examples of <def term="behavior">behaviors</def> of a door:

* Open
* Shut
* Lock
* Unlock
* Turn handle
* Push

Behaviors are actions a thing can do, or have done to it.

The first central insight of OOP is that <highlight>**state and behavior are interconnected**</highlight>.

How? **Behaviors can *change* state**: If you *push the door* (behavior), then *the angle at which it is open* (state) can change. If you *lock the door* (behavior), then *the door becomes locked* (state). If you paint the door, it can become a different color.

However, **behaviors also *depend on* state**: If you push the door but *it is closed* (state) and *latched* (state), then the angle at which it is open might not change. If you try to open the door but *it is locked*, then it doesn’t open.

Note that not *all* states and *all* behaviors of everything in the world are interconnected. There are many doors in the world; if *one* door is locked, that doesn’t affect whether a *different* door can open. And if you open a door, that doesn’t change the color of your shirt (unless something *very* strange is happening!). The states of two different doors are independent; the behavior of the door doesn’t affect the state of your shirt.

The second central insight of OOP is that because not *all* state and behavior are interconnected, <highlight>it makes sense to **group together the behaviors and the state that *are* connected**.</highlight>

## Objects and Classes

### Object

An <def>object</def> is **one thing whose state and behavior are tightly interconnected**. In the example above, the door that connects our classroom to the hallway is one door object. The doors that leads directly into the neighboring classrooms are two *different* door object. Each one of the doors at the top of the front steps of OLRI is a different door object.

An object can have <def term="property">properties</def> (also known as <def entry="property" term="attribute">attributes</def>) that describe its state. The properties of a door, for example, might be “hinge angle“ and “color”, and the **values of those properties** might be “37°” and “green.”

An object can have <def>internal state</def> that is not a visible property, such as the position of all the little pins in the lock mechanism of a door. Note that although the lock pins are not visible, they *do* affect visible properties (“Can the key turn?”) and behaviors (“Try turning the key!”).

### Class

A <def>class</def> is a **category of objects with a shared structure**, objects that **have the same potential states and behaviors**. “Door” might be a class. “Table” might be a different class. “Car” might be a different class.

(Note that we say “might be.” Deciding how to divide the world of an object-oriented program up into objects and classes is a **subjective design decision** that never has a single correct answer. Much of the work of programming is making subjective decisions like this: not writing code, but deciding what *approach* we are going to use to organize the code.)

There can be many objects of the same class. There are many doors in the world, and they are all doors. We say that an object is an <def>instance</def> of a class. For example, our classroom’s door is an instance of the “door” class. When we make a new object of a certain class, we call it <def term="instantiate">instantiating</def> the class. To add a new door to a room, you’d need to “instantiate the door class.”

### Class Definition

We said above that all the objects of a certain class have a shared structure. What does this mean? It means that all the various objects of a given class all have the **same behaviors**, the **same potential states**, and the **same properties**. For example: some doors are open and some doors are shut (*different states*), but they all *could* be open or shut (*same potential states*), and you can try to open or shut any door (*same behaviors*). Some doors are orange and some doors are pink (*a property has different values*), but all doors have a color (*same properties)*.

All the objects of a given class have some shared structure. A description of that shared structure is called a <def entry="class declaration">class definition</def>. Based on the discussion above, we might define the “door” class like this:

**Door class**

* Properties:
  * hinge angle (number, in degrees)
  * is locked? (boolean)
  * color
* Behaviors:
  * open
  * shut
  * lock
  * unlock (with a key)
  * repaint (with a new color)

This class definition is like a blueprint for all door objects. If we have a door, we know that we can try to shut it or unlock it. The door might not allow it, but we can try! However, we can’t try to *eat a door* or *quack a door*, because that’s not part of the class definition of doors. “Quack a door?!” That doesn’t make sense. It’s not in the class definition.

We can ask what the color of the door is, or whether it is locked, because those are properties of a door. They are properties of *all* doors, because they are in the class definition. We can’t, however, ask for the *family name* or the *genre* of a door, because those properties aren’t part of our class definition for doors.

### Summary of Terminology by Example

|  | Class | Object |
| ---- | ---- | ---- |
|  | Door | Your home’s front door |
| **State** | Location, hinge angle, color, position of internal and external parts, etc. | At the front of the house, blue, half open, knob turned slightly, etc. |
| **Property** | Hinge angle (number) | 37° |
| **Behavior** | Doors can open | Open this door! |
| **Internal state** | Position of the lock pins | \[3mm, 1mm, 4mm, 2mm\] |

## Objects and Classes Create Abstractions

Abstraction is the pillar of object-oriented programming. Remember from the first day of class that **abstraction is something that hides complex details** and **presents them in a simpler form**. This is what classes and objects help us accomplish.

Consider a car as an abstraction:

> If you drive a vehicle, you may be able to turn it on and drive. The ability to do this is different from knowing the mechanical underpinnings of the car. What’s under the hood? How does the engine work?! The magic about abstraction is that we do not need to know the details in order to drive the vehicle!

A car presents a driver with properties (Is it turned on? How far down is the pedal pushed?) and behaviors (Turn the wheel! Push the pedal! Honk the horn!). Those things that the *driver* can see and use and do are the <def>interface</def> of the abstraction.

A car’s interface has some complexity to it! Driving well is not easy (as many people on the road prove every day). However, the internals of the car are *far more complex* than its interface. You don’t have to know how a transmission or a motor works to drive a car. These <def>implementation details</def> of a car are hidden from the driver. What is the internal state of the car? A mechanic might have to figure it out, but the driver doesn’t.

In fact, the implementation details and internal state of a car are not merely *hidden from* the driver; they are *protected from* the driver. You shouldn’t be able to rearrange the gears in the transmission or release the toxic chemicals in the battery by operating the interface of the car. If you could, we’d consider this a serious failure!

This *hiding* and *protecting* of implementation details and internal state is called <def>encapsulation</def>. The third big insight of object-oriented programming is that <highlight>when we make good decisions about what we encapsulate and what interface we present, it increases our ability to **keep software manageable as its complexity grows**</highlight> — even when it grows beyond the point where any one person can understand all of it at once!

## Software Too!

Throughout this reading, we’ve looked at physical objects (doors, cars) as analogies for what software does. Let’s consider an actual piece of software acting as an abstraction:

> If you type in a URL address on your browser window to navigate to a certain web page, the browser is able to interpret what you type, request the necessary data from the Internet, and render the page as a bunch of pixels on your computer’s screen. How? You do not need to know the inner workings of the Internet or how the browser processes these requests, yet we use web browsers every day!

Is a web browser an object? In the actual code for the web browser, is there a “browser” class? Perhaps! Most likely the browser is an object that is made of objects that are made of objects that are…. For example, a programmer implementing a web browser might choose to make a “navigation bar” **class**. It would have a current location and browsing history as part of its **state**, and “go to new location” and “go back” as **behaviors**. Then each different browser window would have its own different navigation bar **object**, one **instance** of that class.

## Software in Context

Software development is not just about writing code. It is about making **modeling decisions** like the ones in the previous section. (“How should we organize the code for the browser? Should there be a “navigation bar” class? What do we want the browser to *do*? Are we really building what we should be building?”) Writing software means:

1. Understanding a problem
2. Building a mental model of that problem
3. Understanding the building blocks programming languages / development tools provide
4. Deciding how to express our mental model using the tools available
5. Writing the code
6. Testing and debugging the code
7. Testing, maintaining, and refining this mental model over time
8. Making sure the code continues to reflect that mental model over time
9. Observing, reflecting on, and improving the real-world impact of what we built

Number 5 is not easy — but in a full-scale software project, it is by *far* the easiest item in that list. Yes, really: <highlight>generating code is the easiest part of software development.</highlight>

This is one reason why we say that COMP 127 is **not** a “Java course;” it is a course about the practice of software development that happens to use Java. This is also why it is important for you to get more than just coding as part of your education *even if writing software is the only thing you ever want to do in your life* (and hopefully it isn’t!). To write great software, study communication, human thought, human relationships, creativity, social context, *all* the things you find at a liberal arts college. Then synthesize them with what you know about software. With some tenacity and good luck, you just might build something wonderful.
