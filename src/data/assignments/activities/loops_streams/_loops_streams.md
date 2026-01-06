---
repo: https://github.com/mac-comp127/streams-and-loops
original_url: https://docs.google.com/document/d/1Y88tOIph9zn-zMQGAXOlPclTPb91nIfJ2xk9_CsuptI/edit?usp=sharing
---

# Streams and Loops


{:standard_toc}


## Learning goals

In this activity, you will practice writing queries using streams (`map`, `filter`, and friends) and loops (for-each and friends). While working on this activity, observe how collections and streams manage iteration over data.

After completing this activity you should be able to:

1. Write queries using streams
2. Write queries using loops
3. Convert a given query that uses streams to loops and vice versa

## Task 0

This activity contains the classes `Book`, `Library` (sound familiar?), and `LibraryTest`.

It also includes a complete solution to the activity in `LibrarySolution` if you want to check your work (or if you are stumped, but ask for help first!). Note that there are many correct solutions to each part, so your solution might look different than the given one.

Go over the `Book.java` code. Why do you think the `genres` field of the `Book` class is of type `Set<String>` instead of `String`?

The `LibraryTest.java` file contains a number of book objects and two “libraries.” Briefly inspect the code to observe the books and their properties. Why do you think the tests include an empty library? <hidden>Because it is common for methods to fail on empty input, and we want to test for that.</hidden>

Run the `LibraryTest.java` file and **make sure that all the tests pass.**

## Task 1

Go back to the `Library.java` file. This is where you will write your code.

The first query in the` Library` class is `findTitlesByAuthor`. The existing code gives you a loop version of this query. First, study this version: What does it return? What are its parameters?

Rewrite this code which returns the titles of all the books by the given author **using streams**. For this task, you will need two intermediate operations and one terminal operation. Discuss which ones these are.

**Delete** the existing code — don’t just comment it out — and insert new code that uses streams instead.

<callout>
Afraid of deleting? Want to have the original code to refer to? Of course you do! Remember that you can always use Git to see the original code. Open up GitHub Desktop, and it’s right there in the diff, and in the history tab. **Delete code boldly!**  Don’t leave fragments of commented-out old code in your program.
</callout>

Once you write your code, run the tests to make sure that it works.

Hint: <hidden>Filter (to find the books where the author name is equal to the one given), map (to extract the names of the books), and collect (starts the pipeline of operations and converts the stream into a list).</hidden>

The point of this activity is to compare two approaches side by side. After you complete each task, we encourage you to **compare your solution with the original version**. (Open the repository in GitHub Desktop and look at the changes.) Remember you can also compare your solution to the code in `LibrarySolutions`.

## Task 2

Task 2 asks you to find the titles of the books with more than the given minimum number of pages. In this case, the stream version of the code is provided for you.

Rewrite this code **using a loop**.

## Task 3

How can you display the titles and authors of all the books in the given genre? The existing code includes a loop version of this task. Study this version of the code first. What are the parameters? What does it return?

Reimplement the same task **using streams**. Note that you need to display the title and author name of the books in the given genre.

## Task 4

How many books from a given genre does the library have? The existing code gives you a loop version of this task. Study the code.

Implement the same query using streams. Note that this one is different from the previous tasks in one aspect. What is it?

Hint: <hidden>It returns a number instead of a collection. This should give you a hint about the terminal operation to use.</hidden>

Hint: <hidden>Delete the .collect(…) call and see what other methods your IDE suggests at that point.</hidden>

## Task 5

For this task, suppose you would like to find a book that you can finish during a short trip. How can you find the shortest book in the library? Well, perhaps that will be a book that you’ve already read, so let’s display the _ **n** _ shortest books in the library.

The stream version of the code for this task is coded for you. Study this code. Observe how the Comparator.comparing and lambda expression as a method reference (i.e. `Book::getPageCount`) are used. How can you re-write the method reference in

    Comparator.comparing(Book::getPageCount)

as a regular lambda expression?

Rewrite this query using loops and mutable collections.

Hint: <hidden>Mutable lists have a `sort()` method, and it can take a `Comparator` much like `Stream`’s `sorted()`.</hidden>

## Task 6

Suppose you have found the shortest books in the library but you are actually looking for books of a specific genre. How can you find the books from a given genre, sorted from shortest to longest?

Study the existing loop-based implementation. What are the differences between this one and the one in Task 5?

Rewrite this query using streams.

## Task 7

What is the book with the shortest title? The existing code constraints a stream version of this query. Study this code. What does each part of this solution do? See if you can puzzle it out.

Hint: <hidden><code>min()</code> returns an <code>Optional</code>, which is a little like a List except that it can only have zero values or one value. Why does <code>min()</code> return that instead of a stream?</hidden>

Hint: <hidden>If the <code>Optional</code> has no value, <code>orElse()</code> lets you specify what to return instead. In this case, we want to return null if there is no minimum. But how could there ever be no minimum?</hidden>

Hint: <hidden>Look at the test.</hidden>

Implement this task using loops. The stream version won’t help you; the loop version will look completely different.

Compare the stream version with the loop version. Which one do you think is easier to code for you to create (i.e. _writable_)? Which one is more _readable_?

## Task 8

Let’s print all the items in the library! There is a very, very concise way to do this using the stream-like `forEach` method of `List`. See if you can find it.

Notice that it is not possible for a unit test to check what gets printed to `System.out`, and so **this test always passes even if your code doesn’t work**. After running the tests, you will need to **manually inspect the console output**. (You can do this in VS Code by running “Focus on Debug Console View” from the command palette.) Be sure to do this _before_ you change it, so you know what the output is supposed to look like!

## Bonus Challenges <aside>(optional)</aside>

Hungry for more?

There are several more interesting exercises in `Library` that demonstrate different parts of the Java Streams API that we will not have time to cover in class. Convert the loop-based ones to streams and vice versa.
