---
display_mode: instructions
---

# How to force Visual Studio Code to refresh a Java project

Visual Studio Code’s Java extension is notoriously unreliable. Sometimes VS Code will work itself into a state of utter confusion, giving you strange error messages that seem to have nothing to do with your code, not recognizing JUnit or Kilt Graphics imports, or refusing to even recognize that your code is Java code at all.

When this happens, it is **probably not your fault**. (Working with flaky tools is the life of a programmer, unfortunately! You are not alone.) Sometimes, if you are lucky, you can fix the problem by telling VS Code’s Java extension to just start over from the beginning and try again.


## Try this

- In Visual Studio Code, press **cmd-shift-P** (Mac) / **ctrl-shift-P** (Windows and Linux) to bring up the “command palette,” which lets you search for commands by name.
- Search for and run the **Clean Java language server workspace** command.
- A little alert will pop up in the lower right asking if you want to **Reload and delete**. Click that button.
- Give the project a minute to reload. You should see an “Opening Java Project” popup for a moment, and then when it is done, you should see “Java: Ready” at the bottom of the VS Code window, with no error message next to it.

## If that didn’t work

- Quit VS Code:
    - On Windows/Linux: close **all** your VS Code windows.
    - On a Mac: **quit** Visual Studio Code (Mac). (Closing all the windows wont’t do it. On a Mac, quitting the app altogether is a separate operation from just closing windows.)
- Open your project in VS Code again.

## If that still didn’t work

- Repeat all the steps above _after_ a clean VS Code restart: Clean Java, close VS Code, reopen. Sometimes it only works the second time.

## If that _still_ didn't work

**Contact your instructor or a preceptor for help.** Occasionally more drastic measures to reset the VS Code Java extension can help, such as using the Activity Monitor / Task Manager / command line to find and kill the background build prcess, or finding and manually deleting VS Code build files.

However, at this point it is quite likely that there is something wrong with either your project or your VS Code / Java installation, and just restarting things won’t help. You should **ask for help** before trying more drastic measures.

## The burning questions

<details>
  <summary>
  Why is VS Code Java support so bad?
  </summary>

  The Java extension for Visual Studio Code secretly runs a “headless” (hidden user interface) version of the Eclipse IDE in the background. (That’s basically what the “Java language server” is.) Eclipse is...all right. Eclipse being cajoled into doing things by VS Code is...less all right. Sometimes one program screws up. Sometimes the other does. Sometimes they confuse each other.

  Both Eclipse and VS Code store hidden files on your computer to remember the state and configuration of each project, so that they can reopen and run the project quickly. But sometimes those hidden files get messed up. “Clean the Java language server workspace” means “delete those hidden files and start over from the beginning.”
</details>

<details>
  <summary>
  So why do we even use VS Code, then?! Isn’t there something better?
  </summary>

  For Java projects, [IntelliJ](https://www.jetbrains.com/idea/) is a much better tool. If you are doing serious Java development beyond this class, you should use that instead. (The free “community edition” will be entirely sufficient for most projects.)

  However, IntelliJ is specifically made for Java — and not all of your future projects will use Java. We instructors have found it is helpful to set students up with a _general-purpose programmer’s text editor_, so that they have a familiar tool when they need to jump into unfamiliar programming languages.

  Visual Studio code has _pretty good_ support for a very wide variety of languages, even if it’s not always the _best_ tool for any one of them. It is a good starting point.

  In short, you are paying the price of inconvenience now for flexibility in the future.
</details>
