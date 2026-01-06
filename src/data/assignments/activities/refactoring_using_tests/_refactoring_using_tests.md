---
repo: https://github.com/mac-comp127/refactoring-lists-loops
original_url: https://docs.google.com/document/d/1Xvw0a9pqOTVh3skuyJpvBdtKN7mEywWtVxzx4hUIrBw/edit
cleanup_pending: false
---

# Refactoring with Lists and Loops


{:standard_toc}


## Learning goals

In this activity, you will:

- Learn about **refactoring**
- Practice working with lists
- Practice using different Java loop patterns
- Use unit tests to verify code correctness during refactoring
- Practice thinking about and dealing with **edge cases** and **special cases**

## Refactoring

<definition-callout>
  In software development, <def>refactoring</def> means changing the _structure_ of code without changing its _functionality_.
</definition-callout>

On long-running projects, developers often **refactor** code to keep it readable, maintainable, and ready for the future. But why? Why change code that already works, so that it _does exactly the same thing_?! There are at least three good reasons for this:

1. to **reflect on and improve our own work** after completing a first working draft,
2. to **make the code clearer** to people reading it in the future, and
3. to **prepare the code** for some upcoming change.

Louis Brandeis said, “There is no great writing, only great rewriting.” This might be true of code as well: the first draft is rarely the best draft. It is possible to take rewriting too far — eventually, you need to just say “enough” and move on! — but it is crucial to _have the_ _capacity to rewrite_.

Refactoring can restructure a whole program, or just one little piece. In this activity, you will practice refactoring several individual methods involving lists and loops. Sometimes one version is clearly better. Sometimes there are tradeoffs. Sometimes it is a matter of taste.

The List Basics reading says:

> There are _always_ many approaches! The important thing is not to learn the single best one and always use it, but rather to see the many alternatives and possibilities, and get comfortable with them.

This activity is about learning to _see alternatives_ and _see possibilities._ Good refactoring starts with realizing that you have choices.

## Refactor `numberEachItem`

Look in `ListFormatting`. Read the documentation for the `numberEachItem` method. Do you understand what this method is supposed to do?

Look at the test for `numberEachItem` in `ListFormattingTest`. Study the test cases. Do you understand what they’re testing for? Run the tests and make sure they pass.

**Your challenge:** Refactor this method to use a **C-style for loop** instead of a for-each loop.

Hint: <hidden>You want to loop over the <strong>indices</strong> of the items in the list</hidden>. There is an example of how to do that in <hidden>the Loop Basics reading!</hidden>

Use the tests to help you as you go. If you think you’re done, make sure the tests pass! Even if you aren’t done, however, the tests can still help: you can run the tests when it’s only _partly_ working, and make sure that you are getting the (partial, incorrect) results you expect. “Fails as expected” is useful information. Test early and often! A common beginning programmer mistake is to write lots and lots of code before running _any_ of it.

Once you have your new version and **all these tests pass again**, make sure that you have **deleted the old code**. (Be bold! Don’t just comment out defunct code; _delete_ it!)

( **Notice** how having working tests allows you to refactor like this with much greater confidence.)

Now open up GitHub Desktop, and open up `ListFormatting`. The code you deleted will appear in red; the code you added will appear in green. This is called a “diff” (short for “difference”). Inspect the diff. Compare the old version to the new version.

**Discuss** with your partner: Is one clearly better? What are the tradeoffs?

Commit your work.

## Refactor `formatWithCommas`

New method, same procedure:

- Read the documentation for this method. Does it make sense?
- Inspect the test for this method. Does it make sense?
- Run the tests. Make sure they pass.

**Your challenge:** Refactor this method to use **no loops at all**, and instead use a helpful `String` method that Java provides for you.

You can find that documentation for `String` by putting “ **java 21 string api** ” into a web search engine. (21 is the version of Java we are using. The term “API” here refers to the parts of the `String` abstraction you can see from the outside. We’ll talk more about that later in this course!)

Scroll through the `String` docs. Read the descriptions of the methods. Is there one that would help you put commas in between items from a list? It’s _very_ hard to spot it! (That is unless you already know about this method, in which case it is fairly easy. Experience sure does help.)

Answer: <hidden>The method you want is <code>join</code>.</hidden>

(How are you supposed to find something like that?!? The answer: (1) reading other people’s code, (2) seeking help from people, documentation, or other resources, and (3) experience.)

Q: “OK, but…I don’t see how that method applies here! It doesn’t mention `String` or `List`.”

A: Yes, it is really tricky! It works because <hidden><code>String</code> is a kind of <code>CharSequence</code></hidden>, and <hidden><code>List</code> is a kind of <code>Iterable</code></hidden>. You are _not_ supposed to already know this. You simply cannot know absolutely everything about a programming language or tool at first (or, in most cases, _ever_). Again: reading code, seeking help, experience. Those things are the only way, and they simply take time. Today, you’re getting a nice bite of all three!

Now that you have the method, how do you call it?! Hint: Look at the declaration. <hidden>It is a static method</hidden>. Answer: In the [Unit testing activity](/activities/unit_testing), you learned that <hidden>you can call a static method of another class by saying <code>NameOfOtherClass.methodName(…)</code></hidden>. So the way to call this method is <hidden><code>String.join(…)</code></hidden>.

Once you have figured out how to call the method, your code will <hidden>be <em>very</em> short: just a single return statement</hidden>! The moral here is that sometimes it takes a lot of work to produce something simple. More code ≠ more accomplished.

Go to GitHub Desktop and compare the old versus the new version in the diff.

Commit your work.

## Refactor `formatGrammatically`

New method, same procedure:

- Read the documentation for `formatGrammatically`.
- Inspect the test for it.
- Run the tests.

Notice that this method is a _lot_ like the previous one. Exactly the same, in fact, except that the thing joining the last two items needs to be “`⎵and⎵`” instead of “`,⎵`”. (Here the ⎵ symbols represent spaces.)

There _is_ a way to do this using <hidden><code>String.join</code></hidden> again. Can you see how to do it?

Hint: <hidden>Join all the elements <em>except the last one</em> with commas, then append the “<code>⎵and⎵</code>” and the final element.</hidden>

OK, but how do you do _that_? <hidden><code>String.join</code></hidden> doesn’t have an option to only join _some_ of the list!

Hint: Look at the documentation for `List`. <hidden>There is a method that returns a sublist, i.e. just some of the list but not all of it</hidden>. Having trouble spotting it? [Here it is](https://docs.oracle.com/en/java/javase/21/docs//api/java.base/java/util/List.html%23subList(int,int)).

Spelling it out a little more: <hidden>Make a sublist that is everything except<em> </em>the last element, and pass <em>that</em> to String.join, then append the “<code>⎵and⎵</code>” and the final element.</hidden>

Ask for help. Do the tests all pass? Yay! But wait…

## Oops, fix `formatGrammatically`

Look at the tests for `numberEachItem` and `formatWithCommas`. Both of them test (1) an empty list, and (2) a single-element list. (Find those test cases!) For some reason, however, those test cases seem to be missing for `formatGrammatically`.

**Add those test cases** to the test for `formatGrammatically`. **Run the tests.**

Depending on how you implemented `formatGrammatically`, it is very likely that those cases fail. (If they don’t fail, check with your instructor or preceptor. It’s possible your test might be calling the wrong method, or otherwise not testing what it’s supposed to test. Or maybe you already spotted the problem and handled it!)

When there is some limit or boundary to data — an empty list, an empty string, a minimum value, a maximum value — we refer to the situations where we are working near that boundary as <def term="edge case">edge cases</def>. When writing tests, it is always important to think about these edge cases. Why? Because bugs tend to live there.

For this problem, we can write simple code that works just great when there are 2 elements in the list, or 10, or 100, or 1000000…but doesn’t work for 1 or 0. It’s the edge cases that get us!

Now, **fix the edge cases** of 0 and 1 elements. To do this, you will probably need to treat them as <def term="special case">special cases</def>: instead of handling them with the normal logic, you add an if statement that checks for 0 elements and does something special, and another that checks for 1. We don’t generally like special cases in code; it’s better to limit how many paths the code can take. Sometimes, however, a special case is the best solution.

When the new edge case tests pass, **inspect the diff** and **commit your work**.

## Study the solutions

Either now, or after class, or after you do the bonus challenges below:

Look in the `solutions` directory in the project. There you will find many ways of implementing these methods, with some opinionated commentary on the different approaches. Compare these to your own solutions. Think about each one. What do you notice? What do you wonder?

## Bonus Challenge: `formatGrammatically`, version 3

Try implementing `formatGrammatically` using a C-style for loop. Compare to the previous approaches.

Any other alternative approaches you can see for the other methods?

## Bonus Challenge: implement `formatGrammaticallyWithOxfordComma`

The test for this method is turned off; JUnit is currently skipping it. Enable the test by deleting `@Disabled` from the test file. Run the tests, and make sure that they now fail.

This one is tricky to implement: there’s just not a good way to make it really tidy. Consider all the different approaches you’ve seen for the methods above, and try one. See how it pans out.

Then consider refactoring.

## Bonus Challenge: One method to rule them all

If you are looking for something extra:

Create the following method that can either use the Oxford comma or not, depending on its second parameter:

      public static String formatGrammatically(List<String> items, boolean oxfordComma) {
          ...
      }

Don't implement it by calling the methods already in the code. Instead, do it the other way around! Change the existing “format grammatically” methods so they both use your new one, like this:

      public static String formatGrammatically(List<String> items) {
          formatGrammatically(items, false);
      }

      public static String formatGrammaticallyWithOxfordComma(List<String> items) {
          formatGrammatically(items, true);
      }

...and see if all the tests still pass!


