---
original_url: https://docs.google.com/document/d/1K0GU7oRlr4zc1sDyFke6WHvnIkAf2FEFQ7Xw1potQog/edit
display_mode: instructions
---

# Conceptual Mastery Puzzles


{:standard_toc}


## Rationale and Overview

You are not only learning to _build_ software, but also learning to _think about_ software.

For some aspects of software development, it is good to treat the computer as a partner and let it assist you. However, there are some fundamental concepts you should be **comfortable reasoning about in your own head**, without leaning on the computer for assistance.

To help you build this conceptual mastery, we will give you a series of conceptual mastery goals. Along with each goal, we will give you software that can generate an infinite stream of exercises — and solutions! — for that goal. You can practice these exercises as many times as you want, whenever you want.

When you feel you are ready, you can attempt one of these exercises for credit. When you make this official attempt, you can use your notes but not your development tools. **Remember, no leaning on the computer for assistance**! We grade your attempt for **precise correctness**. You can receive full credit, “almost!,” half credit, or no credit.

Here’s the catch: **you can attempt each goal repeatedly.**  Your final score for each conceptual mastery goal is the score from your best attempt. This means that you have many chances to do well in each goal. If you do badly on the first try, don’t panic! That bad attempt doesn’t permanently bring down your final score in any way. It just means you need to try again.

**The conceptual mastery puzzles count toward a portion of your final grade** (see syllabus). We encourage you to use this opportunity to challenge yourself, and use the puzzles as a way to sharpen your skills and deepen your own learning.

Enjoy!

## Part 1: Get the puzzle generator

To access the conceptual puzzles, open:

> **[Conceptual Puzzles Repo on GitHub](https://github.com/mac-comp127/conceptual-puzzles)**

Click the green “Code” button to clone the repository. Note that this is **not**  a GitHub Classrooms assignment, so you do not have to “accept the assignment” first; just clone the shared repository.

Open the repository in Github Desktop, as you would for a normal assignment. Once there, please open the project in Visual Studio Code.

Here is where things depart from the usual. <highlight>**Do not run the classes in the project from VS Code.**</highlight> You will use the command line for these puzzles!

### What’s the “command line?”

Most of the programs you use have a <def entry="GUI">graphical user interface</def> (<def>GUI</def>). Some programs, however, also have — and many _only_ have — a <def entry="CLI">command line interface</def>  (<def>CLI</def>). This is a kind of text-based interface in which you type commands to the computer one line of text at a time (thus the name “command line”), and the computer prints the result as text.

To get to the command line in a GUI-based computer, you need to open a program that puts the command line in a window for you. **On macOS**, this program is called “Terminal.” **On Windows**, there are two options, “PowerShell” (the newer one) and “Command Prompt” (the older one). **On both platforms**, VS Code also has built-in access to the command line. The instructions below will walk you through using VS Code’s built-in terminal.

The basic form of the commands you type at the command line is:

```sh
somecommand arg0 arg1 arg2 ...etc...
```

This is _aaaalllmost_ like calling a function named `somecommand` and passing it string arguments:

```sh
somecommand("arg0", "arg1", "arg2", ...etc...)
```

…but with one big difference: `somecommand` is not just a function; it is a _whole separate program_ that runs independently, takes input from the terminal, prints its output back to the terminal, and then exits.

Many software development tools work only at the command line, or have features that are only available there. Some remote, powerful, tiny, and/or specialized computers are only accessible through a command line. And knowing the command line can help you accelerate and automate software development tasks. The CLI is a good thing for software developers to learn about!

Curious? Here is [more information on command line interfaces](https://www.w3schools.com/whatis/whatis_cli.asp). Note that the things that page calls “Linux” commands are in fact all UNIX commands, and also work on macOS.

### Using the `puzzle` command

With the `conceptual-puzzles` project open in Visual Studio Code, go to the **Terminal**  menu (in the menu bar) and choose **New Terminal**. Now type `bin/puzzle` and press return. (If you are using Windows and that gives you an error, try using a backslash instead: `bin\puzzle` .) It will take a few minutes to build the first time, and then you should get a help message listing available puzzle commands.

Note that because the `puzzle` is not installed as a system-wide command, you need to open a command line **inside the `conceptual-puzzles` project**  for this to work. Otherwise, the command line will not find `bin/puzzle`. Opening the project in VS Code and _then_ opening the terminal from there should do this for you. (What does it mean for the command line to be “inside” the project? Ask an instructor for help, or read up on the `cd` and `pwd` commands.)

## Part 2: Read the instructions

The README instructions on the Github repository’s landing page contain some starter commands and a brief description of how to access the puzzles through your terminal.

<callout>
⚠️ **Please read the README.** ⚠️

It has that name for a reason!
</callout>

Please let your instructor know if you have any issues.

## Part 3: Practice!

Following the usage instructions the `puzzle` command gives you at the command line:


- List the available puzzle types.

- Choose one. Generate a puzzle! Some good ones to start with, whose content we cover early in the course, are `ast`, `bool`, and `loop`.

- Try solving the puzzle. (Remember: no computer help! Solve it using your own head. This is tricky, but that’s the point!)

- Now use the command line to view the correct solution to the puzzle. **Compare carefully** to your own solution. What made sense? What did you miss? Did you give your answer in the form that the puzzle expects? What questions do you need to ask? Ask them!

- Try again, as many times as you like.

- We strongly recommend **practicing each puzzle type a minimum of 3 times**, even if you get it right the first time. There is a little variation in each puzzle type, and it doesn’t necessarily ask exactly the same thing every time. You don’t want to be caught off guard when you make your official attempt.

- If you are looking for a challenge, **try a higher difficulty level**  if the puzzle offers one. (It will tell you when you view the solution.) You do _not_ need to attempt a higher difficulty to get credit, but some puzzles do reveal additional twists for additional learning at the higher difficulties. This isn’t an opportunity for more points, but it _is_ an opportunity for more learning…and which one are you here in college for, again?

- If you are looking for a laugh, **try generating a puzzle at a ridiculously high difficulty level**, with the `--include-solutions` option (because there is a certain point where a human attempt is just silly!). For example, for a good chuckle, try:

    ```sh
    bin/puzzle gen ast --difficulty 50 --include-solutions
    ```

## Part 4: Make your official attempts for credit

### Procedure for an official attempt

Visit the puzzle web site:

> **[puzzles.comp127.innig.net](https://puzzles.comp127.innig.net)**

Log in (making sure you use your **Macalester**  Google account, or it will lock you out), and click the **Make New Attempt**  button for any puzzle type you like.

Complete your solution **on paper**. We will not accept solutions electronically — not over Slack, not over email. (It’s too much bookkeeping to track a mix of electronic and paper submissions.)

<callout>
  ⚠️ Write down what the problem asks you to submit, in the format it asks, and nothing else. You should follow **exactly the same format as the solutions the puzzle program generated for your practice attempts**.

  ⚠️ **Please write legibly.**
</callout>

Make sure you write your name and puzzle number at the top, **exactly as the instructions at the top of the puzzle tell you to do**.

When you are ready to hand in your solution, you can either **bring it to class**  or **slip it under your instructor’s office door**. When your instructor has received your solution, the web site will show that it is awaiting grading.

Your instructor will return your graded solution in a future class. If you didn’t get full credit, you can attempt the same puzzle type again. You can make as many attempts as you like at each puzzle type during the semester, but you must wait for your instructor to grade one attempt before you can make another.

### What’s allowed?

<callout>
Your official puzzle attempts must be ⚠️ **your own individual work** ⚠️.
</callout>

It’s all supposed to come from your own head. If you find that it’s not in your head yet, practice more first!

Do not work with other students **at all**  when you are attempting a puzzle for credit. Do not ask preceptors for help with your official attempt. (Yes, **do**  work with classmates and preceptors on the practice puzzles you generate for yourself! But do not get help from _anyone else_ except the instructor on your official attempts.)

<callout>
⚠️ **Do not use a computer** ⚠️ to help you with an official puzzle attempt.
</callout>

There is one exception to this rule: it is OK to use a calculator for doing arithmetic. That’s it. Absolutely nothing else. No typing or running code. No searching online. No AI. Don’t even _open_ a browser or any development tools. Remember: all from your own head!

You are not required to do all the parts of each puzzle in one sitting — it is OK to do one part, take a break, do the next part — but you must submit **all the parts**  to get full credit for an attempt, even if you already got some parts correct in a past attempt.

## Updating the puzzle generator

Throughout the semester, we instructors may post new puzzle types. We may also improve some of the existing puzzles. **We will make a class announcement**  if there is an update to the puzzle generator for you. When there is:

- Open up the `conceptual-puzzles` repo in GitHub Desktop (or whatever Git client you are using).

- Pull the changes. In GitHub Desktop, you do this by clicking **Fetch Origin**  and then **Pull Origin**, as [described here](https://docs.github.com/en/desktop/working-with-your-remote-repository-on-github-or-github-enterprise/syncing-your-branch-in-github-desktop#pulling-to-your-local-branch-from-the-remote).

- Open up a terminal inside the puzzle project, as usual.

- To make sure you got the updated code, check the puzzle generator version:

    ```sh
    bin/puzzle --version
    ```

---

Please let your instructor know if the puzzle system doesn’t seem to be working, or if any part of these instructions is confusing.

Happy puzzling!


