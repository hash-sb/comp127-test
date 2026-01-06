---
repo: https://github.com/mac-comp127/employee-database
title_image:
  url: images/acme-logo.png
  alt_text: "ACME Corporation logo"
original_url: https://docs.google.com/document/d/1mTFpb0bkw3wyBiHdp8awRIKLRFjeoDE2vKcHq10L2Mk/edit?usp=sharing
---

# Employee Database


{:standard_toc}


## Learning goals

In this activity, you will practice refactoring an inheritance-based structure to use interfaces and has-a relationships instead. After this activity, you should be able to:

1. identify issues with code structure caused by customizing behavior via inheritance,
2. extract class behaviors into interfaces, and
3. see ways to favor composition over inheritance when you design object models.


## Task 0: Understand your starting point

You are the Head Scientist at ACME Corporation, a company manufacturing such illustrious products as jet-powered roller skates, explosive tennis balls, and invisible paint. As a hub of scientific innovation, all of your employees so far have been laboratory technicians. However, as a result of your business success, you have a very large number of lab techs — so many that operations devolved into chaos. The invisible paint kept spilling all over the anvils, leading to all of the techs tripping over invisible anvils. You really needed someone to keep track of lab space and work assignments!

You thus recently decided to add a few middle managers to your company, and to assign permission for specific rooms to specific employees. You modeled this in your code using inheritance because that seemed convenient at the time.

**Run the `DatabaseMain` class and observe how it behaves.** Try creating two different employees — one lab tech and one middle manager — and then list the employees. You will preserve that functionality as you work on this activity.

**Read the existing classes**, especially `Employee`, `LabTech`, and `MiddleManager`. Which one is the base class? Which methods do subclasses override? What problem is this code solving with inheritance?

<details>
  <summary>
  What do `\n` and `\t` mean?
  </summary>

  In Java and many other languages, a `\` (backslash) in a string indicates an **escape sequence**, which is a way of specifying special characters you can’t necessary type or put directly into a string literal.

   `\n` is “newline,” and it creates a line break in the middle of a string. `\t` is ”tab,” and it indents the text that follows.

  Look at the relationship between the `description` methods and the text they produce when you run `DatabaseMain`.
</details>

<details>
  <summary>
  What does `enum` mean in `RoomType`?
  </summary>

  An enum is a way of creating a fixes list of choices in code. That file essentially means, “These and nothing else are the available `RoomType`s.” There’s a lot more to Java enums, and unfortunately we don’t get to it all in 127! You can read more on your own if you like.
</details>

**Run the tests and make sure they pass.** (It’s a really good habit to do this every time you clone or pull new code! Make sure you’re starting from a place where things work!) Remember that you can do this by clicking the Testing icon in the left toolbar (looks like a chemistry beaker), then clicking the double-triangle “Run Tests” button in the panel that appears:

![Screenshot of the two buttons described in the paragraph above](images/testing.svg){:scale="1"}

**Read `EmployeeTest`** and make sure it makes sense to you.


## Task 1: Understand the problem at hand

It turns out that adding middle managers did not magically get rid of all of the safety hazards in the lab. The exploding tennis balls caused three injuries just this week alone (luckily none severe, but it’s only a matter of time). You need some safety inspectors! Safety inspectors will have access to all labs and supply closets, same as the lab techs.

Looking a little further down the road, there’s a second need coming your way: the lab techs are mad that the middle managers get offices and they don’t. They’re eventually going to want to have their own private office too — but each should only have access only to their _one_ specific office. You can hold them off for a while — safety inspectors are the most urgent thing! — but this is coming.

What does all of this mean?

- Right now, we want to add a new type of employee, `SafetyInspector`, that has the same access permissions as a `LabTech`.
- Soon — not yet, but soon! — we will want to further complicate the access permissions for lab techs, so their permissions are the same as safety inspectors _plus_ a new office permission.

### A bad solution

OK, so how does this play out with our current permissions model?

- We will need to override the `hasAccess` method for the new `SafetyInspector` class. However, that new `hasAccess` implementation will look _exactly_ like the implementation for `LabTech`. Having that duplicated code doesn’t seem ideal.

- We could save the duplicated code by moving it into the superclass: instead of making `hasAccess` an abstract method in `Employee`, we could make it so that all employees have access to labs and supply closets by _default_, and then only `MiddleManager` needs to override it! That’s less code!

   But…this also smells funny. Will it continue to be true that “all labs and supply closets” is a good default for all employees, even as we add new types of employees? What if there are multiple types of employees with duplicated — or partially duplicated permission?

- Oh, wait, we’re about to be in that situation: once we give the lab techs offices, then we _do_ have to override their `hasAccess` method. We could use `super.hasAccess(room)` to do the default “lab and supply closet” check and then also check for the office permission, but this feels like it’s about to get messy as we add more employee types.

Hmm, what’s that smell?

<definition-callout>
  A <def>code smell</def> is “any characteristic of source code that hints at a deeper problem” ([Wikipedia](https://en.wikipedia.org/wiki/Code_smell)). This is a subjective thing, and it is not a certainty; it is a _feeling_ rooted in experience.

  The analogy to “smell” is a good one: not all smells are bad, but most of the time, most things shouldn’t smell too much. If there’s a smell, you want to know where it’s coming from.
</definition-callout>

There are **two code smells** that showed up as we thought through implementation of these new features:

- We are talking about putting **code in the superclass** (`Employee`) that really **only applies to _some_ of its subclasses**. This is a sign of bad decisions about inheritance.

  <details>
    <summary>
    Aside: This problem is not hypothetical, and is not limited to class activities your  instructors cook up to demonstrate principles. Click to read more.
    </summary>

    This happened at Microsoft, for example, in the early 1990s, with their MFC framework for building Windows apps. In MFC, one gigantic superclass of all UI elements ended up with methods for scrolling (even though most subclasses don’t scroll at all) and fonts (even though most subclasses don’t have text), and…well, just take a moment to scroll through [its mind-boggling list of methods](https://github.com/MicrosoftDocs/cpp-docs/blob/main/docs/mfc/reference/cwnd-class.md). (Don’t try to understand; just behold the sheer number of methods in that class.) MFC overused inheritance, and paid a high price in complexity.
  </details>

- We are talking about **stuffing into a single method** things that **should be able to vary independently** (Has lab access? Has floor access? Has access to a specific office? etc.) This is a sign of a missing has-a relationship, a sign that a class needs to split into multiple classes.

### A better solution

Here’s what we really want:

1. We want different access permission rules that can vary independently of employee type. If two types have an identical permission rule, they can share the code.
2. We want one employee to be able to have _multiple_ independent permissions, so that we can add a new permission (such as “has an office”) in a tidy way that does not disrupt too much code.

How could we do this? Talk it over for a minute with your partner, then scroll for a good answer.

<scroll-for-answer/>

Here’s the plan (**just a plan**, we are thinking ahead, **don’t do all this yet**):

- Create a new `Permission` interface, so that there can be many kinds of permissions.
- Create classes that implement `Permission` for “access to supply closets,” “access to labs,” and “access to all rooms on a floor.”
- Create a new relationship so that `Employee` **has many** permissions.
- Move all permission-related logic out of `Employee` and its subclasses, and into this new structure.

This will work out much better! We can reuse the code for lab and supply closet access for both lab techs and safety inspectors. The `Employee` base class will not end up with bits of code that really only apply to some subclasses. We can add new permissions to certain employee types without making a mess of existing permissions.

Let’s do this.


## Task 2: Create a `Permission` interface

Much as in the [Encapsulated Behaviors](/readings/encapsulated_behaviors/) reading, what we need is an interface that handles different permission types. Then, our `Employee` class can contain a `List` of `Permission` objects.

Create a new interface called `Permission`. Add these methods:

- `allowsAccess(...)`, which take a `Room` as a parameter and returns a boolean indicating whether this permission grants access to that room.
- `description()`,  which returns a description of this specific permission (for example, `"Access to all labs"`).

Add a `List` of `Permission` objects called `permissions` as an instance variable in `Employee` class. The `Employee` class should initialize this to an empty `ArrayList`.

Add a method to your `Employee` class that takes a `Permission` object and appends it to the current permissions `List`. Subclasses will use this to add permissions.

Nothing uses that `Permission` interface yet, or the `addPermission` method. You are going to incrementally replace the inheritance-based code with this new structure.


## Task 3: Update `LabTech.hasAccess()` to use the new approach

We will need two new permissions classes, each of which should implement the `Permission` interface:

- `LabPermission`, which grants permission to any room that is a lab
- `SupplyClosetPermission`, which grants permission to any room that is a supply closet

Implement those two classes. Each of them will need implement the two methods that the `Permission` interface requires. What should each one return? You’ll be testing it shortly, so just make your best guess now. If it’s a little bit wrong, you’ll find out soon enough! Hint: <hidden>Each method’s body should contain only a single line of code.</hidden>

Now change the `LabTech` constructor to add the appropriate permissions. Hints:

- How do I get the permissions I need?
  - Hint: <hidden>You need to **instantiate** each of the appropriate permission objects.</hidden>
  - Bigger hint: <hidden>The Java syntax for instantiating an object is `new`.</hidden>
  - Even bigger hint: <hidden>Use code like `new LabPermission()`</hidden>.
- When I have a permission object, what do I do with it? How do I add that permission to this employee?
  - Hint: <hidden>You just wrote the method for that.</hidden>
  - Bigger hint: <hidden>You need to call the `addPermission` method.</hidden>
  - But how do you call it? Hint: <hidden>That method is inherited from `Employee`!</hidden>

Now you are going to make the `Employee` base class use its `permissions` list to implement `hasAccess` in a way that will work for _all_ employee types.

- **Delete** the `LabTech.hasAccess()` method. (Yes, delete! You can always use Git to look at your previous commits if you need to refer to the deleted code. Don’t leave commented-out dead code lying around.)
- Change `Employee.hasAccess()` so that it is no longer `abstract`.
- Write an implementation for `Employee.hasAccess()` that returns true if any one of the employee’s permissions grants access, and false otherwise. Here is a sketch: <hidden>Loop over each of the objects in `permissions`. If it allows access to the room, return true. If you get past the end of the loop, then none of the permissions must have granted access, so return false.</hidden>

**Run the tests again.** If you did it right, they should pass!

<callout>
  **Do not modify the existing tests.** If you think you need to change them, something went wrong. The code should still behave exactly as before.
</callout>

Note that `MiddleManager` still works only because it’s still overriding `hasAccess`. That’s OK. We will fix it next! However, it is a _really good idea_ to test frequently along the way! We don’t want to dig ourselves in a giant hole. When possible, <highlight>take **small steps** from things that work to other things that _also_ work so that you can **test each step and see each it succeed**.</highlight>


## Task 4: Update `LabTech.description()` to use the new approach

We now need to make `description()` use the `permissions` list.

Study the existing `LabTech.description()` method. How much code in that method was actually specific to lab techs? <hidden>Only the part about permissions.</hidden> And how much of it _will_ be specific to lab techs with our new permissions approach? <hidden>**None** of it!</hidden>

**Cut** the `description()` method out of `LabTech` and move it into `Employee`. Make it produce the correct description of the employee’s permissions using the `permissions` instance variable. Doing this takes some fiddling. Some hints:

- <hidden>You can’t return the whole string at once. You have to build it piece by piece.</hidden>
- To do that, create a <hidden>`String` local variable</hidden> to hold the result as you build it piece by piece, loop over <hidden>each object in `permissions`</hidden>, and <hidden>append each permission’s description to the result you are building</hidden>.
- You will have to think about how you handle the `\n` and `\t` characters. It’s better to have them in the `Employee` description method: make each `Permission` object return a string without any line breaks or tabs, and let `Employee`’s `description` method decide how it wants to format them.

This may get tricky, but when you’re done, the result shouldn’t take _too_ much code. When you think you have it, **run the tests!**

If tests fail, study the results carefully. You might be way off track. You might have only made a _tiny_ mistake. Note that the tests will fail even if you are off by a single space character! **Don’t just make “wild guess” fixes.** Study the error, understand exactly _why_ a test fails if it fails, then fix the problem with a clear head.

<callout>
  **Do not modify the existing tests.** If you think you need to change them, something went wrong. The code should still behave exactly as before.
</callout>

After all of this is done, you should be able to run the `DatabaseMain` program again without errors!


## Task 5: Update `MiddleManager` to use the new approach

Repeat everything you just for `LabTech` for the `MiddleManager` class.

One hint: you’ll need create another new class that implements `Permission` to represent <hidden>access to all offices on a given floor</hidden>. Because this new class applies to a specific floor, you’ll need to give it two things that the `LabPermission` and `SupplyClosetPermission` objects didn’t have:

- To remember the floor, <hidden>a `floor` instance variable</hidden>.
- To set the floor when the object is created, <hidden>a constructor that accepts a `floor` parameter</hidden>.
- And…not strictly necessary, but it would be nice if other classes can ask for the floor, so add a <hidden>`getFloor()` method</hidden>.

Implement the two methods of the `Permission` interface for your new class.

Modify the `MiddleManager` constructor to add the appropriate permissions.

Once you’ve done that, what should you do with the `hasAccess` and `description` methods of `MiddleManager`? <hidden>Delete them!</hidden> And what should you do with the code that was in those methods? <hidden>**Nothing!** Just delete it! The new, better code in `Employee` should already do everything those methods were doing.</hidden>

Now, just to be sure you’re not using inheritance for permissions any more, make one last change: **mark the `hasAccess` and `description` method in `Employee` as `final`.**

||============================================================||

[syntax]      Java syntax
[terminology] What it’s called
[semantics]   What it means

||============================================================||

[syntax]

    <ghost>public</ghost> final <ghost>boolean hasAccess(Room room)</ghost>

[terminology]

  - <def>final method</def> <meta>(n)</meta>

[semantics]

  “Subclasses may **not** override this method.”

[discussion]

  Note that you have also seen `final` _variables_. The meaning is completely different; Java just chose to use the same word for both. A `final` _variable_ cannot have its value changed after it is initialized. A `final` _method_ prohibits subclassing.

||------------------------------------------------------------||


Now that those methods are `final`, we know that we don’t have any inheritance sneaking in to make them work. If they work, they _must_ be using our new improved approach!

**Run the tests.** They should pass!

<callout>
  **Do not modify the existing tests.** If you think you need to change them, something went wrong. The code should still behave exactly as before.
</callout>


## Task 6 (if you have time): Add safety inspectors!

You’ve done the part of the activity that had the really important learning, but…remember that this whole exercise first started off because we want to add safety inspectors!

**Add that new employee type** and **give them the appropriate permissions**. (This should go fairly quickly!)

**Add tests** for the new employee type.

**Update DatabaseMain** to support creating the new employee type.

---

## Bonus activities

(These are extras, but they are great practice!)

### Offices for lab techs

Give lab techs the ability to have access a single office that is their office.

### No inheritance?

An interesting result of our refactoring is that the various subclasses of `Employee` serve almost no purpose. Try refactoring the code so that `Employee` has no subclasses, and what were subclass constructors become static methods on `Employee` with names like `createLabTech`.

### Cleaners

Now that you’ve made it easy to dynamically handle permissions for each type of employee at ACME Corporation, it’s time to challenge yourself! The lab techs keep exploding things, and this necessitates a new role at the company: **cleaners**. Add this new type of employee, and give them permission for lab access but not supply closet access. Add a new type of room called `CUSTODIAL_CLOSET` (guess the syntax), and give cleaners and lab techs access to it.

You will also need to modify the base `DatabaseMain` program so that you can add this new employee type to the company.

### Generalize permissions

We don’t really need separate `LabPermission` and `SupplyClosetPermission` classes. We could instead have a single class that grants permission to all rooms of an arbitrary given type, and pass that type in the constructor. Try making that change.
