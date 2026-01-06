---
display_mode: article
original_url: https://github.com/mac-comp127/maps-reading/blob/main/index.md
---

# Map and HashMap

{:standard_toc}

_This reading is adapted from [Part 8.2](https://java-programming.mooc.fi/part-8/2-hash-map) of [the University of Helsinki’s online Java course](https://java-programming.mooc.fi/). Paul Cantrell added coverage of the `Map` interface; the original only discusses the `HashMap` class._

## Learning objectives

- You're familiar with the concept of a map and know how one works.
- You know how to use [Java's Map interface](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Map.html): you know how to create a map, add information to it, and retrieve information from it.
- You can describe situations where using a map could be useful.
- You know how to use a map as an instance variable.
- You know how to go through keys and values of a map using the for-each loop.

## Introduction

A <def>Map</def> is, in addition to List and Set, one of the most widely used of Java's pre-built data structures. A map is used whenever data is stored as key-value pairs, where values can be added, retrieved, and deleted using keys. Other languages call this data structure a “dictionary,” “associative array,” or “hash table.”

The core concept is that a `Map` contains two kinds of things: <def>keys</def> and <def>values</def>. The connection between those is described using various terms:

- The map *associates* each key with at most one value (hence "associative array");
- It *maps* each key to at most one value (hence "map");
- Each key *refers to* at most one value.
- For the "dictionary" metaphor: the words are the keys, and the corresponding definition of each word is the value.

(If you know Python: Java's Maps correspond to [Python dictionaries](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict).)

Just as `ArrayList` is the most commonly used implementation of the `List` interface, [HashMap](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html) is the most commonly used implementation of the `Map` interface.

In the example below, a `HashMap` object has been created to search for cities by their postal codes, after which four postal code-city pairs have been added to the map with `put()`. At the end, the postal code "00710" is retrieved from the map using `get()`. Both the postal code and the city are represented as strings.

```
Map<String, String> postalCodes = new HashMap<>();
postalCodes.put("00710", "Helsinki");
postalCodes.put("90014", "Oulu");
postalCodes.put("33720", "Tampere");
postalCodes.put("33014", "Tampere");

System.out.println(postalCodes.get("00710"));
```

Output:

```
Helsinki
```

The state of the map created above looks like this. Each key refers to,
is associated with, or maps to, some value.

| Key | Value |
|-----|-------|
| "00710" | "Helsinki" |
| "90014" | "Oulu" |
| "33720" | "Tampere" |
| "33014" | "Tampere" |

If a map does not contain the key used for the search, its `get` method retuns `null`.

```
Map<String, String> numbers = new HashMap<>();
numbers.put("One", "Uno");
numbers.put("Two", "Dos");

String translation = numbers.get("One");
System.out.println(translation);

System.out.println(numbers.get("Two"));
System.out.println(numbers.get("Three"));
System.out.println(numbers.get("Uno"));
```

Output:
```
Uno
Dos
null
null
```

Using the Map interface and the `HashMap` class requires the `import java.util.Map;` and `import java.util.HashMap;` statements at the beginning of the file.

Two type parameters are required when declaring or instantiating a map: the type of the key and the type of the value added. If the keys of the map are of type `String`, and the values of type `Integer`, the map is created with the following statement:

```
Map<String, Integer> map = new HashMap<>();
```

Adding to the map is done through the `put(*key*, *value*)` method that has two parameters, one for the key, the other for the value. Retrieving from a map happens with the help of the `get(*key*)` method that is passed the key as a parameter and returns a value.

<blockquote>

*Programming exercise:*

In the `main` method create a new `HashMap<String,String>` object. Store the names and nicknames of the following people in this map so, that the name is the key and the nickname is the value. Use only lower case letters.

 -  matthew's nickname is matt
 -  michael's nickname is mix
 -  arthur's nickname is artie

Then get Matthew's nickname from the map, and print it.

</blockquote>

## Map Keys Correspond to a Single Value at Most

A map has a maximum of one value per key. If a new key-value pair is added to the map, but the key has already been associated with some other value stored in the map, the old value will vanish from the map.

```
Map<String, String> numbers = new HashMap<>();
numbers.put("Uno", "One");
numbers.put("Dos", "Zwei");
numbers.put("Uno", "Ein"); // replaces the existing value associated with the key "Uno"!

String translation = numbers.get("Uno");
System.out.println(translation);

System.out.println(numbers.get("Dos"));
System.out.println(numbers.get("Tres"));
System.out.println(numbers.get("Uno"));
```

Output:

```
Ein
Zwei
null
Ein
```

## A Reference Type Variable as a Map Value

Let's take a look at how a database works using a library example. You can search for books by book title. If a book is found with the given search term, the library returns a reference to the book. Let's begin by creating an example class `Book` that has its name, content and the year of publication as instance variables.

```
public class Book {
    private String name;
    private String content;
    private int published;

    public Book(String name, int published, String content) {
        this.name = name;
        this.published = published;
        this.content = content;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getPublished() {
        return this.published;
    }

    public void setPublished(int published) {
        this.published = published;
    }

    public String getContent() {
        return this.content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public String toString() {
        return "Name: " + this.name + " (" + this.published + ")\n"
            + "Content: " + this.content;
    }
}
```

Let's create a map that uses the book's name (a `String`) as a key, and the book we've just created as the value.

```
Map<String, Book> directory = new HashMap<>();
```

Let's add two books to the directory: ""Sense and Sensibility" and "Pride and Prejudice".

```
Book senseAndSensibility = new Book("Sense and Sensibility", 1811, "...");
Book prideAndPrejudice = new Book("Pride and Prejudice", 1813, "....");

Map<String, Book> directory = new HashMap<>();
directory.put(senseAndSensibility.getName(), senseAndSensibility);
directory.put(prideAndPrejudice.getName(), prideAndPrejudice);
```

Books can be retrieved from the directory by book name. A search for "Persuasion" will not produce any results, in which case the map returns `null`. The book "Pride and Prejudice" is found, however.

```
Book book = directory.get("Persuasion");
System.out.println(book);
System.out.println();
book = directory.get("Pride and Prejudice");
System.out.println(book);
```

Output:

```
null

Name: Pride and Prejudice (1813)
Content: ...
```

## When Should Maps Be Used?

The hash map is implemented internally in such a way that searching by a key is very fast. The hash map generates a <def>hash value</def> from the key, i.e. a piece of code which is used to store the value of a specific location. When a key is used to retrieve information from a hash map, this particular code identifies the location of the value associated with the key. In practice, it's not necessary to go through all the key-value pairs in the hash map when searching for a key; the set that's checked is significantly smaller. We'll be taking a deeper look into the implementation of a hash map in COMP 128, and compare it to other implementations of the `Map` interface.

Consider the library example that was introduced above. The whole program could just as well have been implemented using a list. In that case, the books would be placed on the list instead of the directory, and the book search would happen by iterating over the list.

In the example below, the books have been stored in a list and searching for them is done by going through the list.

```
List<Book> books = new ArrayList<>();
Book senseAndSensibility = new Book("Sense and Sensibility", 1811, "...");
Book prideAndPrejudice = new Book("Pride and Prejudice", 1813, "....");
books.add(senseAndSensibility);
books.add(prideAndPrejudice);

// searching for a book named Sense and Sensibility
Book match = null;
for (Book book: books) {
    if (book.getName().equals("Sense and Sensibility") {
        match = book;
        break;
    }
}

System.out.println(match);
System.out.println();

// searching for a book named Persuasion
match = null;
for (Book book: books) {
    if (book.getName().equals("Persuasion")) {
        match = book;
        break;
    }
}

System.out.println(match);
```

Output:

```
Name: Sense and Sensibility (1811)
Content: ...

null
```

For the program above, you could create a separate class method `get` that is provided a list and the name of the book to be fetched as parameters. The method returns a book found by the given name if one exists. Otherwise, the method returns `null`.

```
public static Book get(ArrayList<Book> books, String name) {
    for (Book book: books) {
        if (book.getName().equals(name)) {
            return book;
        }
    }
    return null;
}
```

Now the program is a bit more clear.

```
List<Book> books = new ArrayList<>();
Book senseAndSensibility = new Book("Sense and Sensibility", 1811, "...");
Book prideAndPrejudice = new Book("Pride and Prejudice", 1813, "....");
books.add(senseAndSensibility);
books.add(prideAndPrejudice);

System.out.println(get(books, "Sense and Sensibility"));

System.out.println();

System.out.println(get(books, "Persuasion"));
```

Output:

```
Name: Sense and Sensibility (1811)
Content: ...

null
```


The program would now work in the same way as the program implemented with the hash map, right?

Functionally, yes. Let's, however, consider the performance of the program. Java's `System.nanoTime()` method returns the time of the computer in nanoseconds. We'll add functionality to the program considered above to calculate how long it took to retrieve the books.

```
List<Book> books = new ArrayList<>();

// ...add ten million books to the list...

// now do two searches:
long start = System.nanoTime();
System.out.println(get(books, "Sense and Sensibility"));

System.out.println();

System.out.println(get(books, "Persuasion"));
long end = System.nanoTime();
double durationInMilliseconds = 1.0 * (end - start) / 1000000;

System.out.println("The book search took " + durationInMilliseconds + " milliseconds.");
```

Output:

```
Name: Sense and Sensibility (1811)
Content: ...

null
The book search took  881.3447 milliseconds.
```

With ten million books, it takes almost a second to find two books. Of course, the way in which the list is ordered has an effect. If the book being searched was first on the list, the program would be faster. On the other hand, if the book were not on the list, the program would have to go through all of the books in the list before determining that such book does not exist.

Let's consider the same program using a hash map.

```
Map<String, Book> directory = new HashMap<>();

// adding ten million books to the list

long start = System.nanoTime();
System.out.println(directory.get("Sense and Sensibility"));

System.out.println();

System.out.println(directory.get("Persuasion"));
long end = System.nanoTime();
double durationInMilliseconds = 1.0 * (end - start) / 1000000;

System.out.println("The book search took " + durationInMilliseconds + " milliseconds.");
```

Output:

```
Name: Sense and Sensibility (1811)
Content: ...

null
The book search took 0.411458 milliseconds.
```

It took about 0.4 milliseconds to search for two books out of ten million books with the hash map. The difference in performance in our example is over a thousandfold.

The difference in performance is due to the fact that when a book is searched for in a list, the worst-case scenario involves going through all the books in the list. In a hash map, it isn't necessary to check all of the books as the key determines the location of a given book in a hash map. The difference in performance depends on the number of books - for example, the performance differences are negligible for 10 books. However, for millions of books, the performance differences are clearly visible.

Does this mean that we'll only be using maps going forward? Of course not. Maps work well when we know exactly what we are looking for, using a unique key. If we wanted to identify books whose title contains a particular string, a map would be of little use.

The hash maps also have no internal order, and it is not possible to search a map based on the indexes. The items in a list are in the order they were added to the list.

Typically, maps and lists are used together. The map provides quick access to a specific key or keys, while the list is used, for instance, to maintain order.

## Map as an Instance Variable

The example considered above on storing books is problematic in that the book's spelling format must be remembered accurately. Someone may search for a book with a lowercase letter, while another may, for example, enter a space to begin typing a name. Let's take a look at a slightly more forgiving search by book title.

We make use of the tools provided by the String class to handle strings. The `toLowerCase()` method creates a new string with all letters converted to lowercase. The `trim()` method, on the other hand, creates a new string where empty characters such as spaces at the beginning and end have been removed.

```
String text = "Pride and Prejudice ";
text = text.toLowerCase(); // text currently "pride and prejudice "
text = text.trim(); // text now "pride and prejudice"
```

The conversion of the string described above will result in the book being found, even if the user happens to type the title of the book with lower-case letters.

Let's create a `Library` class that encapsulates a map containing books, and enables you to case-independent search for books. We'll add methods for adding, retrieving and deleting to the `Library` class. Each of these is based on a sanitized name - this involves converting the name to lowercase and removing extraneous spaces from the beginning and end.

Let's first outline the method for adding. The book is added to the map with the book name as the key and the book itself as the value. Since we want to allow for minor misspellings, such as capitalized or lower-cased strings, or ones with spaces at the beginning and/or end, the key - the title of the book - is converted to lowercase, and spaces at the beginning and end are removed.

```
public class Library {
    private HashMap<String, Book> directory;

    public Library() {
        this.directory = new HashMap<>();
    }

    public void addBook(Book book) {
        String name = book.getName()
        if (name == null) {
            name = "";
        }

        name = name.toLowerCase();
        name = name.trim();

        if (this.directory.containsKey(name)) {
            System.out.println("Book is already in the library!");
        } else {
            directory.put(name, book);
        }
    }
}
```

The `containsKey` method of the map is being used above to check for the existence of a key. The method returns `true` if any value has been added to the map with the given key. Otherwise, the method returns `false`.

We can already see that code dealing with string sanitization is needed in every method that handles a book, which makes it a good candidate for a  separate helper method. The method is implemented as a class method since it doesn't handle object variables.

```
public static String sanitizedString(String string) {
    if (string == null) {
        return "";
    }

    string = string.toLowerCase();
    return string.trim();
}
```

The implementation is much neater when the helper method is used.

```
public class Library {
    private HashMap<String, Book> directory;

    public Library() {
        this.directory = new HashMap<>();
    }

    public void addBook(Book book) {
        String name = sanitizedString(book.getName());

        if (this.directory.containsKey(name)) {
            System.out.println("Book is already in the library!");
        } else {
            directory.put(name, book);
        }
    }

    public Book getBook(String bookTitle) {
        bookTitle = sanitizedString(bookTitle);
        return this.directory.get(bookTitle);
    }

    public void removeBook(String bookTitle) {
        bookTitle = sanitizedString(bookTitle);

        if (this.directory.containsKey(bookTitle)) {
            this.directory.remove(bookTitle);
        } else {
            System.out.println("Book was not found, cannot be removed!");
        }
    }

    public static String sanitizedString(String string) {
        if (string == null) {
            return "";
        }

        string = string.toLowerCase();
        return string.trim();
    }
}
```

Let's take a look at the class in use.

```
Book senseAndSensibility = new Book("Sense and Sensibility", 1811, "...");
Book prideAndPrejudice = new Book("Pride and Prejudice", 1813, "....");

Library library = new Library();
library.addBook(senseAndSensibility);
library.addBook(prideAndPrejudice);

System.out.println(library.getBook("pride and prejudice");
System.out.println();

System.out.println(library.getBook("PRIDE AND PREJUDICE");
System.out.println();

System.out.println(library.getBook("SENSE"));
```

Output:

```
Name: Pride and Prejudice (1813)
Content: ...

Name: Pride and Prejudice (1813)
Content: ...

null
```

In the above example, we adhered to the <def>DRY (Don't Repeat Yourself)</def> principle: avoid code duplication. Sanitizing a string, i.e., changing it to lowercase, and *trimming*, i.e., removing empty characters from the beginning and end, would have been repeated many times in our library class without the `sanitizedString` method. Repetitive code is often not noticed until it has already been written, which means that it almost always makes its way into the code. There's nothing wrong with that - the important thing is that the code is cleaned up so that places that require tidying up are noticed.

<blockquote>

*Programming exercise:*

Create a class `Abbreviations` for managing common abbreviations. The class must have a constructor which does not take any parameters. The class must also provide the following methods:

- `public void addAbbreviation(String abbreviation, String explanation)` adds a new abbreviation and its explanation.
- `public boolean hasAbbreviation(String abbreviation)` checks if an abbreviation has already been added; returns `true` if it has and `false` if it has not.
- `public String findExplanationFor(String abbreviation)` finds the explanation for an abbreviation; returns `null` if the abbreviation has not been added yet.

Example:

```
Abbreviations abbreviations = new Abbreviations();
abbreviations.addAbbreviation("e.g.", "for example");
abbreviations.addAbbreviation("etc.", "and so on");
abbreviations.addAbbreviation("i.e.", "more precisely");

String text = "e.g. i.e. etc. lol";

for (String part: text.split(" ")) {
    if(abbreviations.hasAbbreviation(part)) {
        part = abbreviations.findExplanationFor(part);
    }

    System.out.print(part);
    System.out.print(" ");
}

System.out.println();
```

Output:
```
for example more precisely and so on lol
```

</blockquote>


## Going Through A Map's Keys

We may sometimes want to search for a book by a part of its title. The `get` method in the map is not applicable in this case because the keys are the full title of the book; searching by a part of a key (book title) is not possible with maps.

We can, however, iterate over the keys of a map using a for-each loop on the set returned by the `keySet()` method of the map.

Below, a search is performed for all the books whose names contain the given string -- that is, we find all values whose key contains the given string.

```
public ArrayList<Book> getBookByPart(String titlePart) {
    titlePart = sanitizedString(titlePart);

    ArrayList<Book> books = new ArrayList<>();

    for(String bookTitle : this.directory.keySet()) {
        if(!bookTitle.contains(titlePart)) {
            continue;
        }

        // if the key contains the given string,
        // retrieve the corresponding value
        // and add it to the set of books to be returned

        books.add(this.directory.get(bookTitle));
    }

    return books;
}
```

This way, however, we lose the speed advantage that comes with the hash map. The hash map is implemented in such a way that retrieving a value when you know the key is extremely fast. In the example above, we need to examine every key to see if the provided title part is a substring of the map key (i.e., the book title). There's no way to avoid accessing every key in the map if we want to ensure we find all `Book`s whose title contains the provided string.

<blockquote>

*Programming exercise:*

The exercise template contains a class `Program`. Implement the following class methods in the class:

 -  `public static void printKeys(HashMap<String,String> hashmap)`, prints all the keys in the hashmap given as a parameter.
 -  `public static void printKeysWhere(HashMap<String,String> hashmap, String text)` prints the keys in the hashmap given as a parameter which contain the string given as a parameter.
 - `public static void printValuesOfKeysWhere(HashMap<String,String> hashmap, String text)`, prints the values in the given hashmap whose keys contain the given string.

Example of using the class methods:

```
Map<String, String> hashmap = new HashMap<>();
hashmap.put("f.e", "for example");
hashmap.put("etc.", "and so on");
hashmap.put("i.e", "more precisely");

printKeys(hashmap);
System.out.println("---");
printKeysWhere(hashmap, "i");
System.out.println("---");
printValuesOfKeysWhere(hashmap, ".e");
```

Output:

```
f.e
etc.
i.e
---
i.e
---
for example
```

<callout>
⚠️ The order of the output can vary because the implementation of hashmaps makes **no guarantees** about the order of the keys produced by `keySet()`, or the order of the values from `values()`.
</callout>

(Another way to express this is: preserving the order of the keys and values from those methods is "not part of the contract of the Maps interface". You will eventually [learn more about contracts](/readings/contracts).)

</blockquote>

## Going Through a Map's Values

The preceding functionality could also be implemented by going through the map's values. The set of values can be retrieved with the map's `values()` method. This set of values can also be iterated over with a for-each loop.

```
public ArrayList<Book> getBookByPart(String titlePart) {
    titlePart = sanitizedString(titlePart);

    ArrayList<Book> books = new ArrayList<>();

    for(Book book : this.directory.values()) {
        if(!book.getName().contains(titlePart)) {
            continue;
        }

        books.add(book);
    }

    return books;
}
```

As with the previous example, the speed advantage that comes with the map is lost.

<blockquote>

*Programming exercise*

The exercise template contains the already familiar classes `Book` and `Program`. In the class `Program` implement the following class methods:

- `public static void printValues(HashMap<String,Book> hashmap)`, which prints all the values in the hashmap given as a parameter using the toString method of the Book objects.
- `public static void printValueIfNameContains(HashMap<String,Book> hashmap, String text)`, which prints only the Books in the given hashmap which name contains the given string. You can find out the name of a Book with the method `getName`.

An example of using the class methods:

```
Map<String, Book> hashmap = new HashMap<>();
hashmap.put("sense", new Book("Sense and Sensibility", 1811, "..."));
hashmap.put("prejudice", new Book("Pride and prejudice", 1813, "...."));

printValues(hashmap);
System.out.println("---");
printValueIfNameContains(hashmap, "prejud");
```

Output:

```
Name: Pride and prejudice (1813)
Contents: ...
Name: Sense and Sensibility (1811)
Contents: ...
---
Name: Pride and prejudice (1813)
```

<callout>
⚠️ Remember, the order of the output may vary. The implementation of a hashmap does not guarantee the order of the objects in it.
</callout>

</blockquote>

## Primitive Variables In Maps

A map can only contain values that are reference types (in the same way that `List` does). Java converts primitive variables to their corresponding reference types when using any Java's built in data structures (such as `List` and `Map`). Although the value `1` can be represented as a value of the primitive `int` type, its type for use in a Map or List must be `Integer`.

```
Map<Integer, String> hashmap = new HashMap<>(); // key type "Integer": works
hashmap.put(1, "Ole!");
Map<int, String> map2 = new HashMap<>(); // key type "int": doesn't work
```

A map's keys and values are always reference-type variables. If you want to use a primitive variable as a key or value, there exists a reference-type version for each one. A few have been introduced below.

| Primitive | Reference-type equivalent |
|-----------|---------------------------|
| int       | [Integer](http://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html) |
| double    | [Double](http://docs.oracle.com/javase/8/docs/api/java/lang/Double.html) |
| char      | [Character](http://docs.oracle.com/javase/8/docs/api/java/lang/Character.html) |


Java converts primitive types to reference types automatically as they are added to either a `HashMap` or an `ArrayList`. This automatic conversion to a reference-type value is called <def>[auto-boxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)</def> in Java, i.e. putting something in a box automatically. The automatic conversion is also possible in the other direction.

```
int key = 2;
Map<Integer, Integer> hashmap = new HashMap<>();
hashmap.put(key, 10);
int value = hashmap.get(key);
System.out.println(value);
```

Output:

```
10
```

The following examples describes a class used for counting the number of vehicle license plate sightings. Autoboxing and unboxing takes place in the  `addSighting` and `timesSighted` methods.

```
public class RegisterSightingCounter {
    private HashMap<String, Integer> allSightings;

    public RegisterSightingCounter() {
        this.allSightings = new HashMap<>();
    }

    public void addSighting(String sighted) {
        if (!this.allSightings.containsKey(sighted)) {
            this.allSightings.put(sighted, 0);
        }

        int timesSighted = this.allSightings.get(sighted);
        timesSighted++;
        this.allSightings.put(sighted, timesSighted);
    }

    public int timesSighted(String sighted) {
        return this.allSightings.get(sighted);
    }
}
```

There is, however, some risk in type conversions. If we attempt to convert a `null` - a sighting not in the HashMap, for instance - to an integer, we witness a `java.lang.reflect.InvocationTargetException` error. Such an error may occur in the `timesSighted` method in the example above - if the `allSightings` map does not contain the value being searched, it returns `null` and the conversion to `int` fails.

When performing automatic conversion, we should ensure that the value to be converted is not `null`. For example, the `timesSighted` method in the program should be fixed in the following way:

```
public int timesSighted(String sighted) {
    return this.allSightings.getOrDefault(sighted, 0);
}
```

The `getOrDefault`  method of the `HashMap` searches for the key passed to it as a parameter from the HashMap. If the key is not found, it returns the value of the second parameter passed to it. The one-liner shown above is equivalent in its function to the following.

```
public int timesSighted(String sighted) {
    if (this.allSightings.containsKey(sighted)) {
        return this.allSightings.get(sighted);
    }

    return 0;
}
```

Let's make the `addSighting` method a little bit neater. In the original version, 0 is set as the value of the sighting count in the map if the given key is not found. We then get the count of the sightings, increment it by one, and replace the previous value of the sightings with incremented count. A part of this can also be replaced with the `getOrDefault` method.

```
public class registerSightingCounter {
    private HashMap<String, Integer> allSightings;

    public registerSightingCounter() {
        this.allSightings = new HashMap<>();
    }

    public void addSighting(String sighted) {
        int timesSighted = this.allSightings.getOrDefault(sighted, 0);
        timesSighted++;
        this.allSightings.put(sighted, timesSighted);
    }

    public int timesSighted(String sighted) {
        return this.allSightings.getOrDefault(sighted, 0);
    }
}
```

<blockquote>

*Programming exercise:*

Create a class called `IOUs` which has the following methods:

- constructor `public IOUs()` creates a new collection of IOUs.
- `public void setSum(String toWhom, double amount)` saves the amount owed and the person owed to the IOUs.
- `public double howMuchDoIOweTo(String toWhom)` returns the amount owed to the person whose name is given as a parameter. If the person cannot be found, it returns 0.

The class can be used like this:

```
IOUs mattsIOUs = new IOUs();
mattsIOUs.setSum("Arthur", 51.5);
mattsIOUs.setSum("Michael", 30);

System.out.println(mattsIOUs.howMuchDoIOweTo("Arthur"));
System.out.println(mattsIOUs.howMuchDoIOweTo("Michael"));
```

The code above prints:

Output:
```
51.5
30.0
```

Be careful in situations when a person does not owe anything to anyone.

<callout>
⚠️ An IOUs object does not care about old debt. When you set a new sum owed to a person when there is some money already owed to the same person, the old debt is forgotten.
</callout>

```
IOUs mattsIOUs = new IOUs();
mattsIOUs.setSum("Arthur", 51.5);
mattsIOUs.setSum("Arthur", 10.5);

System.out.println(mattsIOU.howMuchDoIOweTo("Arthur"));
```

Output:
```
10.5
```

</blockquote>

## License

This course material is licensed under a [Creative Commons BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed) license.
