---
display_mode: article
original_url: https://docs.google.com/document/d/1FdwfzT0ChZT1ETADzkDRLdENQQOc_35mXxLOZy2ZgXU/edit
cleanup_needed: true
---

# Contracts and Breaking Changes


{:standard_toc}


Note that all the examples in the [reading about APIs](/resources/apis) work if the client & provider of the API agree on their assumptions about the API. We call these shared assumptions the <def>contract</def> of the API.

It is possible for an API to make <def term="breaking change">breaking changes</def>, i.e. changes that alter the contract so existing clients won’t work anymore. (Have you ever upgraded your OS or your browser, only to find that some app or web site no longer works? This is why.) Identifying the contract is about identifying which changes could be breaking changes.

You can think of a contract as an agreement between the provider and the client of an API: “If the client promises A, B, and C, then the provider promises X, Y, and Z.” This agreement on assumptions both allows and constrains what decoupling may occur.

Contracts are not necessarily forever. To allow future evolution, an API provider will typically have some system for warning clients that breaking changes are coming, along with some mechanism for letting clients control when they move to the new version that has those breaking changes. (Such systems include [semantic versioning](https://www.google.com/url?q=https://semver.org&sa=D&source=editors&ust=1757905937609736&usg=AOvVaw1omTzhooBDiDAhqASda9NR) for libraries and [REST API versioning](https://www.google.com/url?q=https://restfulapi.net/versioning/&sa=D&source=editors&ust=1757905937609823&usg=AOvVaw08HVDnlkfSrniwK0DdNZab) for web APIs.)

Preventing and managing breaking changes depends on identifying things that change the API contract. What things, then, are part of an API contract?


## Types and method signatures as contracts

For a Java class, the following things are all part of the contract, because changing any of them is a breaking change:

- The name of the class
- The superclass, if any
- The interfaces the class implements, if any
- For each public or protected method of the class:
  - The method name
  - The access level
  - The number and types of parameters
  - The return type
- For each public or protected instance variable of the class:
  - The name of the variable
  - The type of the variable

These things are all (1) part of the declaration of classes and methods and (2) can cause a breaking change. We call them the <def>signature</def> of the class or method. (There are a few others we don’t discuss in COMP 127 that are also part of the class signature, such as checked exceptions and the `final` modifier. You can learn about these things if you ever need them, but for now, don’t worry about these details. The important thing here is the big idea of the “signature.”)

For example, consider this class:

    public class Nessie {
        private String name;

        public Nessie(String name) {
            this.name = name;
        }

        public String getName() {
            return name;
        }

        public String swim(int distance, Loch bodyOfWater) {
            bodyOfWater.enter(this);
            travel(distance);
        }
    }

Here is the **signature** of this class, with the non-signature parts grayed out:

<pre>
public class Nessie {
    <ghost>private String name;</ghost>

    public Nessie(String <ghost>name</ghost>)<ghost> {
        this.name = name;
    }</ghost>

    public String getName()<ghost> {
        return name;
    }</ghost>

    public String swim(int <ghost>distance</ghost>, Loch <ghost>bodyOfWater</ghost>) <ghost>{
        bodyOfWater.enter(this);
        travel(distance);
    }</ghost>
}
</pre>

Why is the signature part of the contract? Consider this code that uses Java’s `List` interface:

    void printOddItemsReversed(List<String> items) {
        for (int i = items.size(); i >= 0; i -= 2) {
            System.out.println(items.get(i));
        }
    }

Consider what would happen if somebody changed the signature of `List`’s `size()` method in any of the following ways:

- Rename `size()` to `length()`.
- Change `size()` so it returns `long`.
- Make `size()` require a second parameter, a boolean saying whether to count null elements in the size. (Normally `size()` would count them.)

Every one of these changes would make the code above no longer compile. Every one of these would thus be a breaking change. The signature of a class or a method is part of its contract, a set of promises to API clients. This signature:

    public int size()

…makes a set of promises:

- There is a method named `size`.
- It is public, so you can call it.
- It takes no parameters.
- It returns an int.

Code that uses the method relies on these promises. We could _add_ things to the signature of a class, but _changes_ to things that are already part of the signature have the potential to break our previous promises.

The signature alone, however, is not the whole contract.


## Behaviors as contracts

Consider how each of the following changes to `List`’s `size()` method would affect the code above:

- Make `size()` always return zero, no matter how many elements are in the list.
- Make `size()` count only the elements that are not null.
- Make `size()` return the most elements the list has _ever_ held, not how many it has now.
- Make `size()` remove an element from the list every time you call it.
- Make `size()` enter an infinite loop if the list is empty.

Note that **none of these changes would affect the method signature**: there would still be a zero-parameter method named `size()` that returns an `int`. Code that uses the `size()` method _would still compile_. Every one of these changes, however, would be a breaking change.

In addition to signatures, contracts also include **behaviors**. It doesn’t matter how a method looks; it matters what it _does_. People often categorize behavioral contracts into three types:

- A <def>precondition</def>  is something that the API **client** must guarantee is true **before** it can call a method (or otherwise perform some action). For example, `List`’s `get(i)` has the precondition that `0 ≤ i < size()`. In `GraphicsGroup`, the `remove(gobj)` method has the precondition that the group must contain `gobj`.

- A <def>postcondition</def>  is something that the API **provider** guarantees will be true **after** a method (or other action) completes. For example, `List` promises that `size()` will return the number of elements in the list. It also promises that it will not modify the list. `GraphicsGroup` promises that after a call to `remove(gobj)`, the group will no longer contain `gobj`.

- An <def>invariant</def>  is something that an API **provider** guarantees that any client will **always** observe to be true about an object, before and after every action. For example, a `List`’s size is always nonnegative. A `GraphicsObject` never belongs to two different `CanvasWindow`s.

  A subtle point: It is worth noting that an invariant can _temporarily_ break while a method of the object is _in the middle of doing its work_. However, the invariant must be true again by the time the method is done. The important thing here is that no client of the API can ever _observe_ the invariant not being true.

It is useful to categorize behavioral contracts this way because each category says something different about what is a breaking change:

- A **precondition**  can get **weaker**  without breaking clients, but not stronger. In other words, a provider can _allow things it used to prohibit_, but not the reverse. (That is what the word “weaken” means in this context: the condition becomes _less strict_.) For example, `GraphicsGroup` currently does not allow you to `remove()` a graphics object that is not already in the group. It could change to allow this — relax that precondition — without breaking any existing code.

- A **postcondition**  can get **stronger**  without breaking clients, but not weaker. In other words, a provider can _make additional promises to clients about the result of a method call_, as long as it keeps its existing promises. For example, a new version of `GraphicsGroup` could add a postcondition that `remove()` records the removed object in a “previously removed objects” history that did not exist before.

- An **invariant cannot get stronger _or_ weaker**  in a new version without breaking existing code. For example, here is an invariant of `List`: an integer `n` is a valid index for this list object if and only if 0 ≤ `n` \< `size()`. Because of this invariant, the `size` method can never change what it counts — choosing not to count nulls, for example — without causing a breaking change.

### Examples of contract violations in our class

You've recently seen an example of buggy code and incorrect, unexpected behavior because of a contract violation: the ["Extra fun chaos" in the Introduction to Maps exercise](/activities/maps_intro/#map-keys-chaos). One piece of the contract for a `Map` is *you must not change the keys*. If you violate that contract, the behavior is (at least) not what you would expect.

Another example is from the Pixel Arrays exercise, when you [tried to copy a non-square image using `x` and `y` coordinates](/activities/pixel_arrays/#non-square-reflection), and your code failed because it tried to access an index beyond the bounds of the array. In other words, one piece of the contract for arrays is *you must not try to access an index less than 0, or greater than or equal to the array's size*. One difference here is that Java can detect when you do this, and it halts the program with an "[ArrayIndexOutOfBoundsException](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ArrayIndexOutOfBoundsException.html)". (We haven't discussed exceptions in this class; a Java program "throws an exception" when it detects it has gotten into an invalid state and cannot continue running.)

## Look who just entered the chat

Does all that language about preconditions and postconditions and invarients sound suspiciously like the language of mathematical proof? That’s because it is!

Macalester students often ask, <highlight>“Why does the Computer Science major require Discrete Math?”</highlight> Partly it’s because many of the specific topics in that course — number theory and graph theory, for example — are directly applicable to certain software problems. The biggest and best reason for computer science students to take that class, however, is the course’s general _way of seeing_, the particular _mode of thought_ it teaches: proof-based mathematics.

<callout>
  Proof-shaped thinking is the bedrock of writing good, reliable software.
</callout>

Don’t get the wrong picture here. This does _not_ mean that people who create software all spend their days writing out formal proofs in fancy mathematical notation. (Occasionally, for certain specific kinds of problems, certain software developers do! However, that is fairly unusual.) It does not mean that software development always requires knowing lots of math — at least not in the sense of using trigonometric identities and doing calculus and linear algebra. (Some kinds of software development _do_ in fact require that kind of math. Other kinds require none at all.)

When we say that proof-shaped thinking is the bedrock of writing reliable software, what that means is that good software developers are always asking questions such as:

- Does this work?
- Does it _always_ work?
- How can I _know_ it always works?
- _Can_ I know that it always works?
- What **hidden assumptions** am I making?
- What **edge cases** might break those assumptions?
- What **examples** should I consider?
- Can I find a **counterexample**?

_These_ sorts of questions are what “proof-shaped thinking” means. These sorts of questions are how developers discover and prevent bugs. These sorts of questions are how security researchers and hackers find, exploit, and prevent security flaws. These sorts of questions are how developers use the process of writing code to question the underlying design assumptions of a piece of software, and investigate the customer need or business case behind it.

These sorts of questions are always, _always_ lurking just beneath the surface of software. This reading about API contracts is one place where you see them poking their head up above the water. Learning to see and embrace this way of thinking is an essential part of becoming a good software developer.

And that is exactly the kind of learning you will do in proof-based mathematics courses.


## What should you do with this information?

If you are publishing a library for other projects to use, you’ll need to think very carefully about the shape of your API and its contract.

Even if your code is only for your own team’s project, however, paying attention to the APIs you are setting up within your own code will help you:

- design for decoupling,
- write good tests,
- catch bugs,
- prevent bugs,
- explain your thinking better to your teammates and yourself,
- write better documentation, and even
- catch potential security flaws (as we will soon see!).
