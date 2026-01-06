---
original_url: https://docs.google.com/document/d/1IdM4lH_6roJDOq6-GFcxvRvbT3qauClaqPsxxvOt1jo/edit?usp=sharing
title_image:
  url: images/abstract.jpeg
  alt_text: "An abstract pattern made from colorful pieces of string from the Idea Lab"
hide_general_instructions: true
---

# Idea Lab Activity


{:standard_toc}


## Learning goals

1. Achieve a concrete understanding of classes, objects, methods, instance variables, and local variables.
2. Note subtle differences that often confuse programmers when learning about object-oriented programming:
    - local vs. instance variables
    - reference variables vs. the objects they reference
3. Learn about the special `this` parameter.
4. Learn about object aliasing.
5. Combine programming + art + fun! (“Fun” is of course redundant when we say “programming” and “art,” right?)

## Introduction

Writing code is one way for us to develop our knowledge of the concepts taught in class. Drawing pictures on paper or a whiteboard is another. But in today’s activity, we will use a physical metaphor: you will build a representation of objects and their many parts using materials in the Idea Lab.

Connecting abstract concepts to physical objects is helpful in a number of ways:

- Working through the physical process helps us slow down, focus on details, and thus understand the intricacies of concepts that we may have not noticed before.
- Designing a physical metaphor and sharing / discussing it with others assists in our long-term retention of knowledge.
- Bringing out our inner artists and engaging in the creative process is good for our brains and our well-being!
- Our minds/bodies evolved to inhabit a physical world, and our brains often use capabilities we evolved for that physical world to think about abstract things. Programmers in particular often use spatial thinking to reason about code. (This is possibly part of the reason that programmers are so often musicians: both music and code are intangible things that seem to draw on spatial cognition.)

Ready for some crafting?


## The starter code

Here is some code you will be working with throughout this activity.

(**Hint:** open this document again in a second browser tab so you can quickly refer to this code throughout the activity.)

    public class MenuItem {
         private String name;
         private int price; // in cents

         public MenuItem(String name, int price) {
             this.name = name;
             this.price = price;
         }

         public String getName() {
             return name;
         }

         public int getPrice() {
             return price;
         }
    }

    public class MealCard {
         private int studentID;
         private String studentName;
         private int storedValue = 0;

         public MealCard(String studentName, int studentID) {
             this.studentName = studentName;
             this.studentID = studentID;
         }

         public String getStudentName() {
             return studentName;
         }

         public void addValue(int value) {
             storedValue += value;
         }

         public boolean deduct(int value) {
            if (this.storedValue < value) {
                 return false;
             }
             this.storedValue -= value;
             return true;
         }
    }

And here is some code that uses those classes:

    public static void main(String[] args) {
       MenuItem spaghetti = new MenuItem("Spaghetti", 500);
       MenuItem taco = new MenuItem("Taco", 600);
       MenuItem fries = new MenuItem("Fries", 200);

       MealCard esraCard = new MealCard("Esra", 1234);
       MealCard paulCard = new MealCard("Paul", 5678);

       List<MenuItem> esraItems = new ArrayList<>();
       // We will add more code here
    }

Take a moment to skim that code and get some idea what it does — but you don’t need to understand it all perfectly just yet.

(We encourage you to make up different foods, and/or change the names in the code to match the names of your team members!)


## Create some objects

Your first task will be to make a physical representation of all the objects this code creates.

<highlight>**Don’t start yet!**</highlight> Read the directions through the end of Part 1 and to get an idea of what you’re creating first. Then create it!

You have materials at your station that may help you. You are also free to use any other materials from around the Idea Lab.

You should find a way to physically represent the following concepts:

- Objects are separate things.
- Each object knows what class it is.
- Each object has its own instance variables.
- Each object has methods.
- Public methods are exposed “outside” the class, but private instance variables are hidden “inside” it (encapsulation).
- Variable values can change.
- When a variable’s type is a primitive type, it keeps its value right there on the spot, but…
- When a variable’s type is an object type, it keeps a _reference_ to a different object.
- When there are local variables, they are in their own separate space that is not inside any object.

Here is one possible approach. We encourage you to modify this basic example or come up with a totally new one!

![A small yellow piece of paper labeled MenuItem has a name and a price, and two attached purple tags labeled getName and getPrice. The name is connected by a blue ribbon that points to a second piece of paper, labeled String: spaghetti.](images/starting-example.png)

Note that the `int` instance variable is right there inside the `MenuItem` because `int` is a primitive type, but `String` is its own separate object that the `name` variable references, or “points to.”

Again, you’re free to choose your own materials. **Don’t be afraid to get creative. Have fun with it!** Just watch the clock, and don’t let the whole hour slip away making color decisions!

Now, with that example to get you started and with all your team’s artistic input at work, **make a physical representation of all the objects the starter code creates.**

### Checkpoint

You should be able to answer the following questions **using only at your physical model**, and not looking at the code:

- How many `MenuItem` objects exist? <hidden>3</hidden>
- How many `MealCard` objects exist? <hidden>2</hidden>
- How many `String` objects exist? <hidden>5</hidden>
- How many `ArrayList` objects exist? <hidden>1</hidden>


## Make a stack frame

Your model has several object now — but what ties them together? Who is using them? Where is that information in your physical model?

The answer is that you haven’t represeneted all of the code yet. There are variables like `spaghetti` and `esraCard` that do not yet appear in your physical model. Let’s give them a home.

Get a new piece of paper (or whatever material you like!) to create a home for **all the local variables of the `main` method**. Write the names of the variables, and make them all point to the appropriate objects.

### Checkpoint

You should be able to answer the following questions **using only at your physical model**, and not looking at the code:

- How many local variables are there in the `main` method? <hidden>6</hidden>
- Is there any object that does _not_ have some variable pointing to it yet? <hidden>No</hidden>
- Does any object have _more than one_ variable pointing to it yet? <hidden>Also no</hidden>

### New terminology

This thing you just created is called a <def>stack frame</def>: a container that holds all the local variables of one function / method call. It might look like an object in your physical model, and that is not completely wrong: just like objects hold instance variables, a stack frame holds local variables.

The first big difference between the two is their lifecycle. A stack frame appears when a function is called, and vanishes as soon as the function returns. An object appears with the word `new`, and then lives as long as any other variables point to it. Objects can be created and destroyed in any order, but stack frames always are destroyed in the exact opposite order they were created. (There are some other big differences, and you can learn a little bit more about them in COMP 240 if you take it. For now, the big idea you need to hang on to is that **instance variables live inside objects** and **local variables live inside stack frames**.)


## Change some objects

Suppose we add the following code at the end of `main()`:

    esraCard.addValue(300);
    paulCard.addValue(700);

As a team, talk through what the code does: following the references out from the local variables, reading instance variables, changing instance variables.

Update your diagram to show the effect of running this code.


## Trace a method call

Here is a helper method:

    public static void purchase(
        MealCard mealCard,
        MenuItem item,
        List<MenuItem> purchasedItems
    ) {
        if (mealCard.deduct(item.getPrice())) {
            purchasedItems.add(item);
        }
    }

You will trace through the following method call _in detail_, as it would run if it were added to the end of the main method:

    purchase(esraCard, fries, esraItems);

Here are the steps to trace it:

1. Every time you call a method, its local variables appear. For example, to start, you will need to a place to put the local variables for a call to `purchase()`. That means you need a new <hidden>stack frame</hidden>.
   - Take note! A function’s **parameters** _also count as local variables_. So how many variables will be in your new `purchase` stack frame? <hidden>3</hidden>

2. Note that in several places, there are now **two variables pointing to the same object**. This is called <def>aliasing</def>. Why did this happen in this particular code? Why would you want two different variables to point to the same object? What is that good for? What problems might it cause? Discuss with your team.

3. When the `purchase()` method calls `deduct()`, **add something separate** to represent `deduct()`’s local variables. (Note that it is critically important that you use two different containers to represent the different methods’ local variables!)

   With `deduct`, **there is a special local variable called `this`.** Whenever you send a message to an object — when you call an instance method instead of a static method — `this` is a hidden parameter that points to the object that received the message.

   In other words, whenever you say `foo.bar()`, then when the `bar()` method runs, its local variable “`this`” will point to `foo`.

   Show this in your physical model.

   (Should the `purchase` method also have a `this` variable? <hidden>No.</hidden> Why? <hidden>Because `purchase` is a static method.</hidden>)

4. Apply `deduct()`’s logic. Change whatever instance variables need to change. As you do, think through the code carefully! Don’t just change values because you know they’re supposed to change. Think about how the code sees the world. Which local variables does it have immediate access to? Which local variables does it _not_ have access to? What references does it follow to find the objects it needs to find? Discuss as a group.

   **Checkpoint**

   - Does any object have _more than one_ variable pointing to it now? <hidden>Yes!</hidden>
   - How many variables point to Esra’s meal card? <hidden>3. They are <hidden>`esraCard` in `main`</hidden>, <hidden>`mealCard` in `purchase`</hidden>, and <hidden>`this` in `deduct`</hidden>.</hidden>

5. When `deduct()` is finished, its instance variables go away, so **remove its stack frame** along with any object references that were pointing out from it.

6. Now apply the logic for the rest of `purchase()`. Pay attention! What happens? What does it do? (Hint: it will create a new object _reference_, but it won’t create a new _object_.)

7. When `purchase()` is complete, remove its local variables.

### Checkpoint

- How many items does the `esraItems` `ArrayList` contain? <hidden>1</hidden>
- But the code never said `esraItems.add(fries)`. Where did that reference come from? What code added it? <hidden>The `purchase` method.</hidden> The code that added the new element didn’t know it was specifically adding _fries_ to _Esra’s_ list; it only knew it was adding _some_ item to _some_ list. The specifics were **abstracted away**.

How do all the programming language constructs you’ve laid out in your physical model today enable abstraction? Discuss as a team.

After you have completed Parts 1–3 (or if you are getting a bit lost), you can compare your physical model to [this example](example_solution).


## Trace different purchase scenarios <aside>(optional)</aside>

Try tracing through some different method calls using your object graph. Here, for example, is a scenario where Esra’s card ran out, so Paul bought her a taco:

       purchase(esraCard, taco, esraItems);
       purchase(paulCard, taco, esraItems);  // Paul’s card, but still Esra’s tray

(Esra’s card only ran out because she bought _three_ tacos for Paul yesterday. He likes tacos a lot.)


## After: Garbage collection <aside>**(required!)**</aside>

Before your leave, please take a minute to **clean up your workstation**:

- Tidy up your unused materials.
- Recycle any paper that you’ve written on.
- Leave any reusable materials (e.g. ribbon, post-it arrows) at your workstation for the next class to use.


## Digital Visualization <aside>(optional)</aside>

Here is an [interactive digital visualization](https://pythontutor.com/render.html#code=import%20java.util.*%3B%0A%0Apublic%20class%20MainClass%20%7B%0A%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20MenuItem%20spaghetti%20%3D%20new%20MenuItem%28%22Spaghetti%22,%20500%29%3B%0A%20%20%20MenuItem%20taco%20%3D%20new%20MenuItem%28%22Taco%22,%20600%29%3B%0A%20%20%20MenuItem%20fries%20%3D%20new%20MenuItem%28%22Fries%22,%20200%29%3B%0A%0A%20%20%20MealCard%20esraCard%20%3D%20new%20MealCard%28%22Esra%22,%201234%29%3B%0A%20%20%20MealCard%20paulCard%20%3D%20new%20MealCard%28%22Paul%22,%205678%29%3B%0A%20%20%20%0A%20%20%20esraCard.addValue%28300%29%3B%0A%20%20%20paulCard.addValue%28700%29%3B%0A%0A%20%20%20List%3CMenuItem%3E%20esraItems%20%3D%20new%20ArrayList%3C%3E%28%29%3B%0A%20%20%20purchase%28esraCard,%20fries,%20esraItems%29%3B%0A%20%20%20purchase%28esraCard,%20taco,%20esraItems%29%3B%0A%20%20%20purchase%28paulCard,%20taco,%20esraItems%29%3B%0A%20%20%7D%0A%20%20%0A%20%20public%20static%20void%20purchase%28%0A%20%20%20%20%20%20%20MealCard%20mealCard,%0A%20%20%20%20%20%20%20MenuItem%20item,%0A%20%20%20%20%20%20%20List%3CMenuItem%3E%20purchasedItems%29%20%7B%0A%0A%20%20%20if%20%28mealCard.deduct%28item.getPrice%28%29%29%29%20%7B%0A%20%20%20%20%20%20%20purchasedItems.add%28item%29%3B%0A%20%20%20%7D%0A%20%20%7D%0A%7D%0A%0Aclass%20MenuItem%20%7B%0A%20%20%20private%20String%20name%3B%0A%20%20%20private%20int%20price%3B%20//%20in%20cents%0A%0A%20%20%20public%20MenuItem%28String%20name,%20int%20price%29%20%7B%0A%20%20%20%20%20%20%20this.name%20%3D%20name%3B%0A%20%20%20%20%20%20%20this.price%20%3D%20price%3B%0A%20%20%20%7D%0A%0A%20%20%20public%20String%20getName%28%29%20%7B%0A%20%20%20%20%20%20%20return%20name%3B%0A%20%20%20%7D%0A%0A%20%20%20public%20int%20getPrice%28%29%20%7B%0A%20%20%20%20%20%20%20return%20price%3B%0A%20%20%20%7D%0A%7D%0A%0Aclass%20MealCard%20%7B%0A%20%20%20private%20int%20studentID%3B%0A%20%20%20private%20String%20studentName%3B%0A%20%20%20private%20int%20storedValue%20%3D%200%3B%0A%0A%20%20%20public%20MealCard%28String%20studentName,%20int%20studentID%29%20%7B%0A%20%20%20%20%20%20%20this.studentName%20%3D%20studentName%3B%0A%20%20%20%20%20%20%20this.studentID%20%3D%20studentID%3B%0A%20%20%20%7D%0A%0A%20%20%20public%20String%20getStudentName%28%29%20%7B%0A%20%20%20%20%20%20%20return%20studentName%3B%0A%20%20%20%7D%0A%0A%20%20%20public%20void%20addValue%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20storedValue%20%2B%3D%20value%3B%0A%20%20%20%7D%0A%0A%20%20%20public%20boolean%20deduct%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20if%20%28this.storedValue%20%3C%20value%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20return%20false%3B%0A%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20this.storedValue%20-%3D%20value%3B%0A%20%20%20%20%20%20%20return%20true%3B%0A%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false) of this code. (Yes, the web site says “Python Tutor,” but it also understands Java.)

Step through the code by clicking the **Next** button below the code, and notice how it visualizes the objects with instance variables and the stack frames with local variables.
