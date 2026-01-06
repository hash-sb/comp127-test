---
display_mode: article
original_url: "https://docs.google.com/document/d/1KB3T5can3aC5qygtdjKTUzl0P3c8BlN1QaWy4rIc2F0/edit"
---

# Comp 127 Java Style Guide

Software development organizations often have coding guidelines, which help ensure that code written by one person is readable by another. The guidelines we outline below are relatively standard across the Java development community. Your code in this class must follow these practices; this is part of how we grade the homeworks.

Your overarching goal is to make code **readable** and **clear**. When you write code, don’t just think of getting it working now. Think of your collaborators — both present and future. Think of _yourself_ in the future.

After quickly glancing at your source code, a reader should be able to understand how it is structured and get a general idea of its purpose. After careful study, a reader should be able to follow along with what you were thinking, and understand both _how_ and _why_ the code does what it does.


{:standard_toc}


## File Structure (starting with hw0)

- All classes must be in packages.
- Package names must start with a lowercase letter.
- Each Java file must have a header comment with the following:
  - Author name
  - Brief description of the class
  - Acknowledgements: _Always_ include names of people and sources from the web that helped you complete your work.


## Naming Conventions (starting with hw1)

### General naming advice for all programming languages:

- Classes, methods, and variables must have **descriptive names** that make their purpose clear.

  | ✅ Good: | `amountRemaining` |
  | ❌ Bad:  | `amtrem             // huh?` |
  {:.compact}

  | ✅ Good: | `backgroundColor` |
  | ❌ Bad:  | `c                  // What is c?` |
  {:.compact}

  | ✅ Good: | `totalPrice` |
  | ❌ Bad:  | `updatedIntResult3  // So many words, so little info!` |
  {:.compact}

- _Tip:_ If you find that a variable _name_ merely restates the variable’s _type_, ask yourself whether you can choose a better name for the variable that describes _what it means,_ _what it is for_, or _what role it plays_ instead of only stating _what its type is._ Sometimes there is no better name — but often there is!

  | ✅ Good: | `Point point;           // A location? a size? of what?`|
  | ❌ Bad:  | `Point playerPosition;  // Ah, so THAT’S what it means` |
  {:.compact}

- _Tip:_ If you are unsure about whether it’s OK to abbreviate a word in a name, don’t.
- _Tip:_ **The larger the scope of a name, the clearer it needs to be.** If you have a counter variable named `n` that is only used in 3 lines of code, that’s probably fine. But if a variable that is visible throughout a whole large program, then `n` is a bad name; consider naming it something more descriptive.

### Java-specific conventions:

- Method and variable names must be in `lowerCamelCase`
  - _Except:_ Static final variables must be in `ALL_CAPS_WITH_UNDERSCORES`
- Class names must be in `UpperCamelCase`.
- Class names must be singular nouns (or noun phrases).
  - _Except:_ A class that contains only static helper methods may have a plural name, e.g. `Collections` or `DoodadUtils`
- Interface names may be either adjectives (or adjectival phrases) if they describe a capability (`Comparable`), or nouns if they describe a general category of thing (`List`).
- In most situations, method names should be an imperative verb or verb phrase.

  | ❌ No:  | `resultPrint()` |
  | ✅ Yes: | `printResults()` |
  {:.compact}

  - You may break this rule if and only if it makes the code that _uses_ the method clearer in context. (This is called _clarity at the point of use._) We will see some examples of this with streams and event handling later in the semester.


## Braces and Indentation (starting with hw1)

- Opening curly braces `{` must be at the end of the line. Closing curly braces `}` must be on their own line. The indentation of a closing brace must be the same as the indentation of the opening statement it matches. There should always be a space before the opening brace.

  ✅ Yes:

  ```
  while (veggieDrawer.isOpen()) {
      guineaPig.wheek();
  }
  ```

  ❌ No:

  ```
  while (veggieDrawer.isOpen())
  {
      guineaPig.wheek();
  }
  ```

  ❌ No:

  ```
  while (veggieDrawer.isOpen()) {
      guineaPig.wheek();
      }
  ```

  ❌ No:

  ```
  while (veggieDrawer.isOpen()) {
      guineaPig.wheek(); }
  ```

  ❌ No:

  ```
  while (veggieDrawer.isOpen()){
      guineaPig.wheek();
  }
  ```

  - _Commentary:_ This way of placing braces is called the “K&R style” or “one true brace” styles. There are many different styles of brace placement, and they can all work. What _doesn’t_ work is mixing and matching styles! The important thing is for the people on a software project to pick one style and stick with it. K&R indentation is by far the most common in Java, so it’s what we use in this class.

- Lines must be indented in accordance with the structure of the code.
  - The start of every statement within the same block must be indented the same amount.
  - If a statement spans multiple lines, then the indentation must reflect the statement’s structure.
  - _Hint:_ In IntelliJ, use Code → Auto-Indent Lines or Code → Reformat Code to clean up improperly indented code.

  ✅ Yes:

  ```
  if (mouth.isEmpty()) {
      Food nut = nutContainer.take();
      if (nut != null) {
          mouth.eat(nut);
      }
  }
  ```

  ❌ No:

  ```
  if (mouth.isEmpty()) {
  Food nut = nutContainer.take();
      if (nut != null) {
          mouth.eat(nut);
          }
      }
  ```

  ❌ No:

  ```
  if (mouth.isEmpty()) {
      Food nut = nutContainer.take();
      if (nut != null) {
      mouth.eat(
      nut);
  }
  }
  ```


## Cleaning up (starting with hw1)

- Are there TODOs that you completed? **Delete them!**
- Is there code that you commented out in case you needed it later? If you’re handing in your assignment, you don’t need it anymore! **Delete it!**
- Are there variables, methods, classes, or even entire files you aren’t using anymore because you changed the code? **Delete them!**
- Are there lots of extra blank links in the middle of a file? **Delete them!**
- Are there print statements you put in the code purely for debugging purposes? **Delete them!**


## Decomposition (starting with hw2)

- Each method must have a single, clear purpose.
- A single method, including `main()`, should rarely be too long too fit on the screen all at once. Most methods should be short. Excellent methods are often only one or two lines long!
- Each class should provide a coherent collection of behaviors that represent a single coherent concept. Ideally, _a class should work as a meaningful abstraction_, not just a bag full of code.
- _Tip:_ If a class has a large number of instance variables, that is a strong sign that it wants to be decomposed.
- Prefer composition (has-a) over inheritance (is-a). Prefer interfaces over superclasses. Use inheritance (`extends`) sparingly.


## Testing (starting with hw2)

- All non-trivial methods not requiring user input or drawing should be covered by a unit test. “Covered by” means that the method being missing or broken could cause some test to fail.
- Tests should use _adversarial thinking_: they should not just demonstrate that the code can work, but should actively _try to make it break_ by testing corner cases and special conditions.
- As much as possible, methods that draw graphics or require user input should be independent of methods that perform abstract rules or logic (see Decomposition). This will help make your code more testable, and also easier to read. If it seems difficult to test your methods, consider whether you should refactor your code.


## Comments and API documentation (starting with hw3)

- Comments should _complement_ code, not _restate_ it. Do not write a comment that does nothing more than say exactly what the code already says. ([Don’t do this](images/door.jpeg)!) Instead, think about whether you can **provide additional useful information** :
  - Is there _context and intent_ that is important for understanding this code?
  - Would it help to _summarize_ a complex section of code that would otherwise be difficult to read?
  - Are there _unresolved concerns or questions_ about this code that future developers should be aware of?
  - When writing API documentation, are there _details_ that are not obvious from the declaration itself?

    ❌ Bad:

    ```
    /**
     * Sets the position to the given point.
     */
    public void setPosition(Point position) {  // comment repeats this almost verbatim
    ```

    ✅ Good:

    ```
    /**
     * Moves this rectangle's upper left corner to the given
     * position, preserving its size.
     */
    public void setPosition(Point position) {
    ```

- All public classes must have Javadoc (API documentation) describing what they do.
- All non-trivial public methods should have Javadoc describing what they do.

  - Sometimes the name of the method says it all. In this case, it is OK to leave out the Javadoc. But this is rarely truly the case!
  - In code out in the wild, people don’t always use Javadoc for classes that are _internal to a single project_; Javadoc is especially important for code that is _providing a published API_ across teams, time, or code boundaries. But in this class, we practice writing thorough documentation for everything.

- API documentation must start with "`/**`", not "`/*`". Otherwise it isn’t Javadoc!

  - Regular comments using  `//` or `/* */` include remarks for people who want to understand the _internals_ of a class. These comments are for developers reading the code, not just using the code. They do not show up in generated API documentation.
  - Javadoc comments `/** */` are the special ones that come before classes and methods and describe what they do. These comments are intended for the people _using_ a class. They show up in generated Javadoc [like this](https://www.google.com/url?q=https://mac-comp127.github.io/kilt-graphics/&sa=D&source=editors&ust=1749578735157707&usg=AOvVaw03-XlhBMfUWfL2Two52aH5).

- Javadoc must appear immediately before the class or method it describes — not above the imports, not inside the method. Immediately before. See examples below.
- If the Javadoc for a method is long and complex, use the `@param` and `@return` tags to help organize it:

  ✅ Good:

  ```
  /**
   * Opens a new window for drawing.
   *
   * @param title        The user-visible title of the window
   * @param windowWidth  The width of the window's content area
   * @param windowHeight The height of the window's content area
   */
  public CanvasWindow(String title, int windowWidth, int windowHeight) {
  ```

- Conversely, if you can describe the meaning of the method, its parameters, and its return value all in one breath, and `@param` and `@return` tags would thus add only redundant information, then omit them:

    ✅ Good:

    ```
    /**
     * Returns the width of the window's visible content area,
     * not including any title bar and border.
     */
    public int getWidth() {
    ```

- You do not need to write Javadoc for a method if:

  - it implements or overrides a method **from an interface or superclass**, and
  - that interface / superclass method **already has javadoc**, and
  - there is nothing useful to say about how this particular class overrides or implements the method beyond what the interface / superclass Javadoc already says.


## Class Definitions (starting with hw3)

- There should be **no** public instance variables. A class should expose _behaviors_, not its internal state. If you want to expose a property directly, use setters and getters. Also think about what more meaningful behaviors you might expose instead of setters, such as `pedalHarder()` in the `Bicycle` class instead of `setSpeed()`.

  - Out there in the wild, there are certain circumstances where public instance variables without getters and setters are in fact the best choice — but in Java, using getters and setters is _usually_ best, and it’s good to build that habit from the beginning.

- All classes should have an appropriate `toString()` method if you have reason to print the class for debugging purposes.

  - _Hint:_ Your IDE can generate a reasonable starting point for you. Take a look at what it generates, and think about what would be useful for debugging. Adjust it if it says too much or too little.

    - **VS Code:** Right-click inside the body of course class, and choose Source Action… → Generate toString….
    - **IntelliJ:** Choose Code → Generate…, then choose toString from the popup.


