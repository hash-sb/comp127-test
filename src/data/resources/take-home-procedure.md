---
display_mode: instructions
original_url: "https://docs.google.com/document/d/1zbYG1SOJvar52_XmtKRq0yOuDNIHS5jjaDn8Rz3_m7Y/edit"
---

# Homework / Take-Home Exercise Procedure

Follow these instructions closely, step by step.

<callout>
  ⚠️ **Do not skim or skip!** ⚠️

  Missed steps = chaos and sadness.
</callout>


{:standard_toc}


## Start the assignment

1. Open the GitHub Classroom link for the assignment, which you will find at the top of the assignment instructions.

2. **If this is your first time accepting an assignment:**
    1. You will get a screen asking you to “Join the classroom.”
    2. **Find your name**  in the list of students, and follow the instructions to associate your GitHub account with your name.
    3. If you cannot find your name, **contact the instructor**  and then choose “Skip this step.”

3. Click the **Accept this assignment**  button.

4. You should see a page saying that “Your assignment repository has been created,” or something to that effect. There will be a repository url that looks something like this:

    `https://github.com/mac-comp127-f23/assignment_name-your_github_name`

    **Click that link.**

5. This will take you to the GitHub page for your personal copy — called a “fork” — of the homework. **Read the homework instructions**  that are on this page.

6. When you are ready to start coding, click the big green **Code**  button.

7. Choose **Open with GitHub Desktop**  from the drop-down menu that appears. (Your computer may ask you if it should allow the link to open the GitHub Desktop application. Allow it.)

8. You should get a **Clone a Repository**  dialog in **GitHub Desktop**. If you do not, then ask an instructor for help.

9. As always, check the **Local Path**  setting. Make sure that you approve of where GitHub Desktop is going to put the code — and remember that location.

10. Click **Clone**.

11. If presented with the dialog titled “How are you planning to use this fork?”  Select the “ **For my own purposes** ” option then click Continue.

12. Now you have a copy of the homework project on your own computer! Click the **Open in Visual Studio Code**  button.

Now **follow the instructions in the homework repo**.

When you have made some progress…


## Save your progress with a commit

<definition-callout>
  In Git, a <def context="Git">commit</def> is a snapshot of your code at a point in time. You can use it to preserve your work as it is now for future reference, in case you want to see what changed or go back to the way it was.
</definition-callout>

Follow these steps **every time you have progress worth saving**:

1. Open your homework project in GitHub Desktop.
2. In the **Changes**  tab, you will see all the source files you changed. Click on each file, and make sure that **all**  the changes you see are changes that make sense and look good to you. This self-review process is important. Measure twice, cut once!
3. To approve the changes, check the checkbox next to each file (if it isn’t already checked). Note that your snapshot **will not include**  any changes for files that are not checked.
4. In the box at the bottom that either says something like “Update Foo.java” or says “Summary (required),” type a very short one-line summary of what you did. This is your **commit message**. Some examples of commit messages:

    | ✅ Good: | Fixed area calculations |
    | ✅ Good: | Add wings to elephant |
    | ✅ Good: | Completed assignment part 2 |
    {:.compact}

    | ❌ Bad: | Updated Thing.java  | _← Updated why? with what?_ |
    | ❌ Bad: | More changes        | _← Communicates nothing_ |
    | ❌ Bad: | Committing!!!       | _← Communicates nothing_ |
    | ❌ Bad: | asgsdfglakjsdfgl    | _← Communicates nothing_ |
    | ❌ Bad: | BUG FIX ATTEMPT     | _← What bug?_ |
    {:.compact}

    ❌ Bad: Made changes to add feather and wing structure methods to Elephant class and also implemented flapping logic in the ElephantController class and added unit tests for both classes in the test suite directory _← Way too long!_

1. Click **Commit to main**. This saves a snapshot of your work on your computer. Note that it **does not copy your work to the cloud**, and **does not submit your assignment**  (yet). Committing only saves a snapshot on your computer.


## Are you done?

In this class, it pays to **read the instructions with extreme diligence**. If you think you are done, read the instructions once again, line by line, looking for things you might have missed.

Check your work against the [Comp 127 Style Guide](/resources/style-guide/). Are there things you need to clean up? Just because the code _runs_ doesn’t mean it’s clean or healthy.


## Submit the assignment by pushing

<definition-callout>
  In Git, <def context="Git">push</def> means “move commits from one computer to another,” most commonly “move commits from _my_ computer to the cloud.”

  It is a way of sharing your work with other people — in this case, your instructor and preceptors. You can also use pushing to share work with teammates when you are on a team. (This is how professional developers use it.)

  Pushing is also a great way of backing up your work in case of laptop disaster!
</definition-callout>

1. In GitHub Desktop, look in the **Changes**  tab to make sure that you have no uncommitted changes. If it isn’t committed, Git won’t push it! You should see a big message that says “ **No local changes**.”

2. Just under that message, click the **Push origin**  button to send your changes to GitHub, i.e. to the cloud. (You can also choose the **Repository → Push**  menu option.)

3. Click the **View on GitHub**  button. Check that you can see your changes. This ensures that the push worked. Do not skip this step if you are submitting homework!

    <callout>
      ⚠️ **If it’s not pushed, it’s not turned in.**  ⚠️
    </callout>

4. Note that you can **push as many times as you want**. Each time, Git copies your commits to the cloud. We will grade whatever you have pushed by the deadline, unless you tell us you need extra time. The “submission time” for your homework is the time of the last commit you pushed.

## Do the self-evaluation _(take-home exercises only)_ {#self-evaluation}

1. Find the `SELF_EVALUATION.md` file in the project, and open it in your editor.

2. Work through the **checklist** in that file, marking each item as follows:

    ```md
    - [ ] did not check this item yet
    - [x] checked this item and it was all good
    - [!] checked this item and I spotted a problem in my code
    ```

   Note that if you spot a problem, **you do not need to fix it** to get full credit for a take-home exercise! The goal of these exercises is _completion_, not _correctness_:

   | Assignment type     |   | Goal
   |---------------------|---|---------------------|
   | In-class activities | → | participation       |
   | Take-home exercises | → | completion          |
   | Homeworks           | → | correctness         |
   | Conceptual puzzles  | → | _exact_ correctness |
   | Project             | → | creation            |
   {:.compact}

   You _can_ fix the problems in take-home exercises if you want to, but the most important thing is to _notice_. Notice what you missed, and learn from it. Notice what kinds of things are in the checklist, and learn to infer those same things from the assignment instructions.

   The self-evaluation checklists in the take-home exercises are very similar to the rubrics the preceptors use to check your homework assignments. In the homeworks, however, you can’t see the rubric; you have to read the instructions carefully, and take charge of checking your own work.

3. Answer the questions in the **reflection** section. Just a few thoughtful sentences for each question is sufficient. **Do not write a long essay.**

4. **Commit** your self-evaluation and **push** it. Remember:

   <callout>
     ⚠️ **If it’s not pushed, it’s not turned in.**  ⚠️
   </callout>
