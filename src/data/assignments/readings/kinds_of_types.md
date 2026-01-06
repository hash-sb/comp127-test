# Java’s Two Kinds of Types

## Reading

- Read [this code about primitive types](https://github.com/mac-comp127/kinds-of-types/blob/main/src/kindsoftypes/PrimitiveTypes.java){:.assignment}.
- Then read [this code about object types](https://github.com/mac-comp127/kinds-of-types/blob/main/src/kindsoftypes/ObjectTypes.java){:.assignment}.
- Remember to **run the code** for yourself, see what it does, and **try experimenting with it**. Here are **[the instructions](/resources/code-as-reading-procedure)**. (Note that you only have to clone the code _once_ for this reading, even though it has two parts: both files come together, in the same repository.)

**Supplemental reading** (_not required_, but if you’re curious and hungry for more, or you’d like some more traditional reading to supplement the code):

- [Java tutorial: Primitives](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html "https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html")
- [Java tutorial: Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html "https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html")

## Important concepts

- <def>operator</def>
- limits of int types, <def>overflow</def>
- <def>boolean value</def>
- <def>primitive type</def> vs <def>object type</def>
- <def>method</def>, <def>method call</def>, <def>receiver</def>
- equality operator (`==`) vs. equality method (`.equals`) for strings
{:.concept-list}

## Things to consider <aside>(exercises for yourself, not to turn in)</aside>

1. Ponder the puzzles at the end of PrimitiveTypes.java. What questions do you have?
2. What `might` the AST for this code from the reading look like?   
  
    ```
    mascot.repeat(3)
    ```
       
    What about this?
 
    ```
    (mascot + "!").repeat(3)
    ```
       
    What about this?
 
    ```
    mascot.substring(0, 2).repeat(100)
    ```
       
   We haven’t covered this in class yet, or in the readings. Just take a guess! What do _you_ think might make sense? Think about it before class.
  
3. What other little experiments can you try on this code?

## Your response

What was particularly interesting, confusing, or exciting to you in this reading?
