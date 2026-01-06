---
display_mode: article
---

# About Software Testing

<definition-callout>
  <def lowercase>Testing</def> is the process of ensuring that something functions as people want it to function.
</definition-callout>

## The Testing Mindset

Testing is a _crucial_ part of software development.

Beginning developers (and bad project managers) often view testing as an afterthought — “first build it, then make sure it works” — but for the expert, **testing is a central part of the _entire_ software development process**, right from the very beginning.

When people are first learning to code, just getting the code to work _at all_ is a huge victory. The beginning developer’s mindset: “Can I get this to work??” That is a good and natural place to start. And getting code to work at all is not easy!

As you mature as a software developer, however, your mindset will shift. Think: when you use somebody else’s app or web site, are you happy if it just works _sometimes_? How often does it have to do the wrong thing before it becomes frustrating, and you look for an alternative? To make great software, it’s not enough to make sure that code works _once_; we aspire to make sure that it _never doesn’t work_. (This is close to impossible, but we aspire!)

The expert developer’s mindset thus shifts away from “**Can I get it to work?**” and toward “**How can I break it?**” You might be surprised by the idea that intentionally _trying to break your own software_ can be desirable, and even fun — but it is! Every piece of code you write is a little puzzle, asking a question: “Does it _always_ work? Can I find some input, some situation, that my code doesn’t handle correctly? What did I miss? How can I trick my own code into doing the wrong thing? How can I break it?” Good developers are thinking this way from the very beginning of a project.

This “**How can I break it?**” mindset is at the heart of **software testing**. It is a theme you will encounter throughout this course.


## Kinds of testing

There are _many_ different approaches to testing, and many different goals testing might have. Because there are so many kinds of tests, developers have many ways of describing and categorizing them, and have invented many different terms to describe them. This reading will introduce you to just a few of those terms.

One important distinction is manual vs. automated testing:

- <def lowercase>Manual test</def> / <def>interactive test</def>: A human interacts with the code in an ad-hoc way by inputting data and observing the results. They might make up tests as they go along, or they might run through a written checklist.

  - _Advantages:_ easy to set up, encourages exploratory testing, can catch problems that the tester wasn’t even looking for but happened to notice
  - _Disadvantages:_ time-consuming, not easy to repeat, may not be perfectly consistent or thorough every time, easy to miss things

- <def lowercase>Automated test</def>: a developer writes _test code_ that runs through a predetermined list of test cases, and checks that the code being tested produces the correct result for each one. In other words, the programmer writes _code to test other code_.

  - _Advantages:_ consistent for different people, consistent over time, tests run fast so it makes sense to test often, excellent for catching <def>regressions</def> (things that used to work, but don’t work anymore)
  - _Disadvantages:_ only tests what the test code is looking for, coding tests is time-consuming, test code has to be maintained just like other code and may become a burden, some kinds of tests are hard to automate

Another important distinction is unit vs. integration testing:

- <def lowercase>Unit test</def>: A test to verify that one individual part of the software works in isolation.

  For example: “Does the doorknob turn? Does the door hinge swivel?”

- <def lowercase>Integration test</def>: A test to verify that many pieces work when they are all put together.

  For example: “If I turn the knob and pull, does the door open?”

To appreciate the distinction between these two, watch this video entitled [Two Unit Tests, Zero Integration Tests](https://www.google.com/url?q=https://drive.google.com/file/d/1xvr8ej9wFDg-kFmlLW-6brXr74ETMXMG/view?usp%3Ddrive_link&sa=D&source=editors&ust=1758991071647132&usg=AOvVaw011VFnseV69LkfzJBJWj1f).

You have already done some **manual testing**. In our next in-class activity, you will be working with **automated unit tests** for the first time in this class.


