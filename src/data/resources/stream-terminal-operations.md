---
display_mode: article
original_url: https://drive.google.com/file/d/1XcIeKUKZGcyOHiytn5nSbSJ5meecQXtm/view?usp=sharing
---

# Terminal Operations on Streams: Collectors and Reducers

{:standard_toc}

So far we've seen *intermediate* stream operations: things like `map`
and `filter` take in a stream, transform it somehow, and return another
stream.

But at some point you've got to stop, right?

That's what <def>terminal operations</def> do. They, uh, terminate the
stream and return...well, different kinds of things. There's two broad
categories for what you can do to terminate a stream and do something
with all its elements:

- You can collect them together somehow into some data structure that
  *isn't* a stream.
- You can do some computation on them and return different kinds of
  things. The most common thing here is to count them, or calculate some kind
  of statistical measure.


## Collecting all the elements of a stream: collections and collectors

We're going to investigate the distinction between these closely-related
things in Java:

1. **Collection**: a certain kind of data structure; `List` is one of the
  most basic examples.
2. **Collector**: an interface for operations that turn streams into
  collections.
3. **Collectors** (note the plural!): a utility class with many static
  methods that implement the `Collector` interface.
4. **`.collect()`**: a method in the `Stream` class that applies a
  `Collector` to the stream as a terminal operation.

### A brief review of collections

In English, what *is* a collection? One reasonable description:

<definition-callout>
  A <def>collection</def> is a group or set of things.
</definition-callout>

And what can you *do* with a collection? What are some of the actions
you could perform on a collection? Some examples include:

- getting the size of the collection;
- adding, removing things from the collection;
- asking if the collection contains a certain thing;
- seeing all the things in the collection

Let's translate that to Java: a collection sounds like an instance of a
class. Class instances have state and behavior. Here:

- The *state* is some data structure that contains the things in the
  collection.
- The *behavior* includes methods that do the above actions. For
  collections in Java, we have:
  - `.size()`
  - `.add()`, `.remove()`
  - `.contains()`
  - `.iterator()`

There are lots of ways to implement the above, so Java has a generic
`Collection` *interface*. [Here's the definition of that
interface](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Collection.html).

You are already familiar with several subtypes of this interface.
The `List` interface is the most common: a `Collection` whose elements have a specific ordering. Another example is a `Set`: a `Collection` whose elements do not have a particular order, but are unique.

### The Collector interface

As we've seen, streams are similar to collections, with an important difference. A
nice high-level way to think about the distinction is from the
philosophical bit from the streams reading from *Modern Java in Action*:
a stream is a set of values spread out in *time*; a collection is a set
of values spread out in *space*.

A <def>collector</def> is something that gathers the elements from a stream into a collection.
[The `Collector` interface](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/Collector.html) describes such an operation, specifying how the collector accumulates and combines elements, and specifying what kind of collection it produces.

From above, if you have a stream, perhaps the most basic way you'd like
to collect those spread-out-in-time elements is to put them into a
`List`. Another operation you might want to do with the stream is to
group the elements according to some characteristic,

These common operations are provided for you in Java, and conveniently
bundled up into...

### The Collectors utility class

The [Collectors
class](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/Collectors.html)
contains a, well, collection of static methods that implement the
`Collector` interface. Let's take a look at two of the most common:
`toList` and `groupingBy`.

#### Converting a stream into a list

When we want to collect the elements of a stream, we use the stream's
`.collect()` method and provide it with a *collector* --- something
implementing the `Collector` interface. The most straightforward and
frequently used collector is the `toList` static method, which gathers
all the elements of a stream into a `List`.

For example: let's:

1. create an infinite stream of numbers, then...
2. limit to the first 10 elements of the stream, and then...
3. use `collect()` to gather those into a list:

```
Stream<Integer> infiniteStream = Stream.iterate(0, i -> i + 1); // step 1

List<Integer> intItems = infiniteStream
                         .limit(10) // step 2
                         .collect(Collectors.toList()); // step 3

System.out.println(intItems);
```

The output is:

```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Notice above that we've printed `intItems`, and that the type of
`intItems` is `List<Integer>` --- that is, a `List` of `Integers`.

Observe that the `List` contains `Integer`s: the autoboxed
reference-type version of `int`s. That means you can have `null` in this
list: add this to the code above:

```
intItems.add(null);
intItems.add(42);

System.out.println(intItems);
```

...and you get what you'd expect: `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, null,
42]`.

#### Converting a stream into a map

Another common operation is to take the elements of the stream and group
them according to some characteristic of each element. The static
factory method `Collectors.groupingBy()` does this and produces the
results as a `Map` instance.

Let's modify the above example and group those numbers according to
their remainder when dividing by 3.

```
Stream<Integer> infiniteStream = Stream.iterate(0, i -> i + 1);

Map<Object, List<Integer>> intItems = infiniteStream
 .limit(10)
 .collect(Collectors.groupingBy(i -> i % 3);

System.out.println(intItems);
```

(Note the lambda expression there!)

The output is:

```
{0=[0, 3, 6, 9], 1=[1, 4, 7], 2=[2, 5, 8]}
```

This is what Java calls a <def term="Map (data structure)">Map</def>. We'll see more on `Map`s
later; for now, just observe what we're doing: we have various
*remainders from division by 3*, and we want to *map* them --- or
associate them with --- *lists of numbers associated with that
remainder*.

The remainders are called the *keys* of the `Map`; the lists are the
*values* associated with those keys.

Above, the 0, 1, and 2 on the left side of the equals are the keys; the
lists on the right side of the equals are the values. For example, the
list `[1, 4, 7]` is the value associated with 1 because when you divide
1, 4, or 7 by 3, the remainder is 1.

More specifically, in the context of `groupingBy`:

- The *map key* is the value the returned by the classification
  function: what was passed into `groupingBy`. Above, that's the lambda
  `i -> i % 3`. The output from that lambda is only ever 0, 1, or 2, and
  so those are the keys in the map.
- The *map value* is a list of all the elements from the stream
  associated with the classified value --- that is, the classification
  function, when given that element, output that key. The values here
  are the Integers 0 to 9.

For example, for the stream element 5: the function `i -> i % 3`, when
given 5 as input, produces 2; therefore, the element 5 is in the list
associated to the key 2. Note how input and output to the function
correspond to values and keys.

(The above stream produces `Integer`s and could in principle yield
`null`. What do you think would happen above if `groupingBy` sent a null
value to our lambda function?)


## Transforming elements a stream to something else: reducers

Perhaps we don't want to collect together all the elements of a stream,
but instead transform them into...something else. This kind of terminal
operation is called <def>[reduction](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/package-summary.html#Reduction)</def>.

Here are two such reducers.

### Counting the number of elements in a stream

How many numbers less than 100 are prime?

This example...

1. Creates the same kind of infinite stream of numbers we've been using,
   then...
2. limits to the first 100 elements of the stream, then...
3. filters out non-prime numbers (equivalently, only allows through
   those numbers for which the predicate `isPrime` is true), then...
4. counts the number of elements in the resulting,
   filtered-to-just-primes stream.

```
Predicate<Integer> isPrime = i -> (code that returns true for primes);

infiniteStream = Stream.iterate(0, i -> i + 1);

long count = infiniteStream
            .limit(100)
            .filter(isPrime)
            .count();

System.out.println("There are " + count + " prime numbers less than 100.");

```

We get:

```
There are 25 prime numbers less than 100.
```

<details>
  <summary>
  An aside on streams and collectors compared with collections
  </summary>

  We interrupt this discussion of reducers to bring you this note on
  collectors...

  That code is a nice example of the spread out in time/space idea: as the
  numbers get bigger, the `isPrime` function is going to take longer and
  longer to decide whether or not the number is prime. This means that as
  `filter(isPrime)` gets larger and larger numbers, it's going to take
  longer and longer to produce the next value of the resulting primes-only
  stream --- that is, the elements are spread out further and further in
  **time**.

  But if we are applying a collector to the stream, once we get them all,
  they will all simultaneously be in the computer's memory or hard drive.
  And if we have *lots* of primes --- perhaps millions --- those elements
  will be spread out over millions of different bytes of **space** in memory
  or storage.

  Note that there's another way to write that stream. Look up these ingredients see if you can assemble them together to do the same thing: [`BigInteger`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigInteger.html#%3Cinit%3E(java.lang.String)), [`BigInteger.nextProbablePrime()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigInteger.html#nextProbablePrime()), [`iterate()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/math/BigInteger.html#%3Cinit%3E(java.lang.String)), and [`takeWhile()`](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/IntStream.html#takeWhile(java.util.function.IntPredicate)).

<!--
something like

```
BigInteger TWO = new BigInteger("2");
Stream<BigInteger> primes = Stream.iterate(TWO, BigInteger::nextProbablePrime);
long numPrimes = primes.takeWhile(x -> x.intValue() < 100).count();
```
if anyone asks...
-->

</details>

### Beyond counting: the summary statistics reducer

When you are collecting numbers, we often want to reduce them down into an
average, or find the minimum or maximum, or similar. Java provides these
related summary statistics bundled together into the [summaryStatistics
reducer](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/IntStream.html#summaryStatistics())
(the link is to the int version; there are corresponding long and double
ones).

Usage is pretty straightforward: here, we get the average number of
pages of some books:

```
IntSummaryStatistics stats = books.stream()
                                  .mapToInt(Book::getPageCount)
                                  .summaryStatistics()
                                  .getAverage();
stats.getAverage();
```

The output will be what you expect. See the documentation above for the
other statistics bundled together in a summary statistics object.


## Java tips

### Streams have their own `toList` method

Collecting a stream into a list is very, very common, and it gets a
little tedious to always write something like
`.collect(Collectors.toList())` every time. Since this terminal
operation is so common, streams themselves provide a `toList` method:
for example, above we had

```
List<Integer> intItems = infiniteStream
                         .limit(10)
                         .collect(toList());
```

could equivalently be written

```
List<Integer> intItems = infiniteStream
                         .limit(10)
                         .toList());
```


### Import statement to avoid writing “`Collectors.`” so many times

One tiny little note about the import statement: we usually write this:

```
import static java.util.stream.Collectors;
```

That allows you to write `Collectors.toList()` and so on, but if you
get tired of typing "Collectors", you can shorten your code a bit by
changing the import statement.

```
import static java.util.stream.Collectors.*; // note the asterisk
```

Then you can write `toList()` and `groupingBy()` and so on instead of
`Collectors.groupingBy()`.
