Class activity: use a toaster to illustrate object modeling concepts from the reading.

1. In groups of 2 or 3: enumerate properties & behaviors of a toaster
  - Just list, don’t describe
  - No code!! Just enumerate properties & behavior, as the reading did for a door.
  - No looking stuff up. No computers, phones, tablets -- just analog, organic humans!
  - There is no single correct answer!
  - **Note:** Toasters are rare in many countries, and some students may never have even seen one! That’s great: software dev often involves learning about unfamiliar things. Pair students who’ve never used a toaster with ones who have. The students who do _not_ have experience with a toaster should be the ones writing out the list of properties and behaviors. Tell them to **ask clarifying questions**.
2. Share & compare with neighboring group. What differences do you notice, small or large?
3. Summarize interesting differences with whole class
4. Free-ranging whole-class discussion. Likely topics:
  - terminology differences
  - what is state? what is behavior? (in the larger sense, or for the toaster)
    - does behavior have to change the state of the object?
  - different people might have been picturing different things (e.g. pop-up toaster vs. toaster oven)
  - different level of abstraction (heating coils vs “push it down”)
  - different features included / not included (e.g.)
  - what is NOT in your model? What did you omit? What did you abstract away?
    - omitting something entirely is different from hiding something in an abstraction; compare omitting the state about how many burnt crumbs are inside versus omitting details of what it means for the lever to be down, because you abstracted that away into a bit of boolean state
  - design is subjective!
  - different people may be imagining different things!
  - hidden assumptions
  - central role of communication
  - sketching and prototyping
  - when is it better _not_ to include everything?
  - when we write code, how do we make all these modeling decisions?
  - "all models are wrong, but some are useful". Map is not the territory, menu is not the meal, and so on
  - there's a well-known analogy between Platonic forms and OOP's classes and objects. Fun to think about but likely a bit more than what the students can take on board right now
  - etc etc etc

Follow your nose!

## Other articles, resources, ideas

Here are some other things that seem relevant to this discussion activity. Some are likely a bit too much for students right now, but can be nice to be thinking about, and maybe to discuss later.

- [Building Objects out of Plato: Applying Philosophy, Symbolism, and Analogy to Software Design](https://doi.org/10.1145/1164394.1164396): Platonic forms and OOP -- and program design. Interpreting literature, like Poe's Raven, or Hemingway's *The Old Man and the Sea* -- the act of analysis, of crafting an analogy at the right level of abstraction, and generating ever more general type hierarchies. Not only good things to consider for OOP design, but helps make the case that computer science really can be thought of as one of the liberal arts.
- [Rayside and Campbell, “An Aristotelian Understanding of Object-Oriented Programming.”](https://doi.org/10.1145/353171.353194). Gets into the weeds of Aristotelian, Platonic philosophy, and Darwin and evolution...one interesting angle here is the relation between *design* and *execution/code* and epistemology and ontology:
  - the design process is inductive or inferential: you start with what you know about, say, toasters -- or what your client says they want in your program -- and produce the more abstract thing: a design document or code for a class definition. That seems to mirror our everyday experience of making observations and inferring what the Platonic realm has. We see a bunch of metal boxes on kitchen counters, and infer that there is some Platonic form of a toaster. This, the article says, is Aristotelian, because Aristotle refers to the singular (those actual literal metal boxes) are "primary substance" and the universal (the Platonic form) is secondary substance.
  - Your design or code goes the other way, though: the abstract thing precedes or has some causality upon the particular thing. To get an actual object in your Java program, you need a class definition to exist.
  -
