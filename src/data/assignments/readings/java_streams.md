# Streams, Map and Filter

## Reading

- Read [Modern Java in Action, Chapter 4: “Introducing Streams”](https://drive.google.com/file/d/1I8XUMNF-tuEMxYpx7_Qzx_faUX43o0P7/view?usp=sharing){:.assignment} (from ["Modern Java in Action"](https://www.manning.com/books/modern-java-in-action) by Urma, Fusco, and Mycroft)

## Important concepts

- the <def term="map (functional operation)">map</def> and <def>filter</def> intermediate pipeline operations ← this is the main thing to learn here
- internal iteration
- pipelining
- <def>intermediate operations</def> versus <def>terminal operations</def> on a pipeline
{:.concept-list}

## Instructor note

This reading uses the term “declarative programming” to describe Java stream. I would quibble with that terminology. To be truly “declarative,” code must describe only _what_ results it should produce, and nothing about _how_ to get them. While Java streams do have _some_ of this flavor — one might be able to argue that they have a “more declarative style than loops” — Java code that uses streams still does describe a specific algorithm. A truly declarative language, like SQL or Prolog, says nothing about how the computer gets the result.

Like many terms in computer science, this seemingly precise term is in fact quite fuzzy. Others certainly would quibble with my quibble. My own opinion is that the book overreaches by using this term.

A better, more widely understood term for this style of programming is “functional pipeline” or “function pipeline.” (And I’m sure somebody would quibble with that!)

## Things to consider

- Consider the dishes example in CH 4.
  - How are the dishes in the menu represented?
  - What are some possible operations on this collection?

- What are some advantages of using Streams?
- What are some disadvantages?

## Your response

What was particularly interesting, confusing, or exciting to you in this reading?
