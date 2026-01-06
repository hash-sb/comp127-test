# Defensive Copying

## Reading

- Read [Effective Java – Item 50: Make defensive copies when needed](https://drive.google.com/file/d/1NDvUNedlNfrKQdBSNZaOeJ9aoXtRwRhK/view?usp=drive_link). This reading mentions some concepts we have not covered in class, but you should be able to understand the core points.

## Important concepts

- <def>defensive programming</def>
- <def>invariant</def>
- <def>malicious client</def>
- <def>internal state</def>
- <def>defensive copy</def>
{:.concept-list}

## Things to consider <aside>(exercises for yourself, not to turn in)</aside>

1. Think of an example of each of the following:
   - class invariant
   - immutable class
   - non-final class
   - window of vulnerability
2. How do you determine whether an object is immutable? Is an immutable object always “safe?”
3. What if you are writing code that is never called by or used by someone who is not on your team? Would you ever still want to practice “defensive programming” to defend against _yourself?_

## Your response

What was particularly interesting, confusing, or exciting to you in this reading?

---

## Additional reading <aside>(optional)</aside>

This comes from [_Effective Java_](https://macalester.on.worldcat.org/search?queryString=effective+java). The whole book is an excellent one to read in its entirety. Is it essential if you plan to do serious work in Java after this class, and its excellent discussion of general programming principles will make you a better programmer no matter what language you are working in.

If you are curious to read more of the book, we have posted an **optional** additional excerpt for you:

- [Effective Java – Item 49: Check parameters for validity](https://drive.google.com/file/d/1TVeul6CSJtK_h7O9mZHyYVaq38-oHvsn/view?usp=drive_link)
