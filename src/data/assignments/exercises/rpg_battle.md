---
repo: https://github.com/mac-comp127/rpg-battle
original_url: https://docs.google.com/document/d/1RFONv0970ayc7tSyfxncIPJgmEh1ztLvuLFHDvvEcYA/edit?usp=sharing
---

# RPG Battle

This take-home exercise gives you practice refactoring to introduce an interface.

The self-evaluation rubric for this assignment is in `SELF_EVALUATION.md`.

## Step 0: Plan

<definition-callout>
  <def lowercase>Refactoring</def> (n, v): changing the structure of code without changing its functionality.
</definition-callout>

In this activity you will start with some code that works, but is just a bit messy. You will introduce an interface to clean up the code.

The code is the start of the battle system for an RPG. Characters can battle each other with dwarven swords and magic fireballs. Here are the rules:

- Each character in the game has a name, hit points, and energy.
- “Hit points” refers to how much life the character has left; when hit points reach zero, the character is defeated.
- “Energy” refers to how much magical energy the character has.
- Each character in the game has the ability to attack with either a sword or a fireball, but not both.
- The sword does a random amount of damage (where “damage” means “subtracts hit points”).
- The fireball does more damage, but requires energy (because they are magical fireballs). If the character does not have enough energy, the fireball attack fails.

Just like real life! This game is going to be great…if only we can clean up this code.

General hints:

- **Run your tests early and often.**  You should be able to break the refactoring into smaller steps. If so, run the tests after each step. (If you’ve driven off the road, you want to find out as soon as possible! Same thing goes for code.)
- **Did you break something and aren’t sure how to make it work again?**  No worries! Remember that you still have the original code through GitHub Desktop, so you can always copy the original code for a method and start over.
- **You can peek at the solution.**  The text below includes links to a solution you can use to check your work. But use the hints below first, or it will ruin the “aha” moment!

Play the game by running the `Game` class. Get a sense of how it works.

Look at `GameCharacter`. Which methods here have some complexity? There is a way to break this class into multiple classes, and use polymorphism to make the logic less tangled. Take a moment to study and think about this before you move on.

The `attack()` method has a giant conditional that does two totally separate things, and the `getWeaponDescription()` method has a similar bipartite structure. Can you imagine a class design that would let you split these methods apart with polymorphism instead of conditionals? **Sketch it out on paper.**

Related Hint: <hidden>There is one pair of instance variables (`swordMinDamage`, `swordMaxDamage`) that is mutually exclusive with another pair (fireballDamage, fireballEnergyRequired). This also suggests a split.</hidden>

Second hint: <hidden>This is a <strong>lot</strong> like the Library / Media activity.</hidden>

Think, then when you are ready, scroll down to the next step to break this big change into manageable steps.

<scroll-for-answer />

Don’t proceed until you’ve done your planning and thought about a solution!

## Step 1: Extract a new class

To split apart this class using polymorphism, you will need to make a `Weapon` interface. (You might have come up with a different name for it in step 0. That’s fine! Use the name that makes the most sense to you.)

This refactoring will be a lot to do at once. You’re going to get there using small, testable steps.

**Before you make a new interface**, your first step will be to create a separate `Weapon` _class_. After this first step, the code will work so that `GameCharacter`  **has a** `Weapon`. `Weapon` will still include both fireball and sword attacks, and it will still have the giant conditional. However, this change will set you up well for the work to come.

**Leave an `attack()` method in `GameCharacter`** that means “attack this other character with my weapon.” `GameCharacter`’s `attack()` method should do nothing but delegate its work to `Weapon`’s `attack()` method.

Things to think about:

- Make sure that `Weapon` ends up in the `rpgbattle.model` package, alongside `GameCharacter`.
- What do you need to add so that each `GameCharacter`  **has a**  `Weapon`?
- You should change the `GameCharacter` constructor so that it takes a `Weapon` object instead of all those separate parameters. How will you do that?
- You’ll want to move `attack()` into `Weapon`. When you do, it will need to take _two_ `GameCharacter`s as parameters instead of just one. Why?
- What other methods and instance variables need to move into `Weapon`, if any? If they do move, should they keep the same name?
- How should other code tell a character to use their weapon to attack?

When you change `GameCharacter`’s constructor, this will break code in both the `Game` class and the unit tests: you will need to update all the code that calls the `GameCharacter` constructor. You may also need to make another small change or two — but if you followed the directions above, you should **not**  need to change any of the calls to attack() in either `Game` or `GameCharacterTest`.

Code compile? Now get all of your tests passing.

<callout>
**Do not move ahead until all your tests pass.**  Remember that your tests don’t pass when you are confident in your work and everything seems obviously good. They pass when you **actually run them and see them pass!**
</callout>

Once you have it solved — or if you’re stuck — you can peek at the [solution for step 1](https://github.com/mac-comp127/rpg-battle-solution/commit/e00970b23573739ad2308f7493191b46bcdeba80) to check your work. Your solution might not be identical! If you aren’t sure whether that’s OK, check with the instructor or a preceptor.

## Step 2: Create a new abstraction

Now turn `Weapon` into an interface.

What method signature will the `Weapon` interface contain? Look at your `Weapon` class for guidance.

Create classes to represent `Sword` and `Fireball`. What is the relationship between these and the `Weapon` interface?

Implement the method required by your `Weapon` interface in `Sword` and in Fireball. Hint: <hidden>You can get rid of the giant conditional in attack</hidden>

Write constructors for Sword and for Fireball. You get to delete some code! Hint: <hidden>You can get rid of the <code>IllegalArgumentException</code> now!</hidden>

**Get all of your tests passing.**  You will need to make a slight change in the two lines that create the Sally and Marvin test objects. You should **not need to change**  any of the other code in the tests.

## Step 3: Rename things

Look at `Sword` and `Fireball`. Do you notice how all of `Sword`’s instance variables have the prefix “`sword`,” and all of `Fireball`’s have the prefix “`fireball`?” Those prefixes were necessary to keep everything straight when it was all one class — but they aren’t necessary anymore. (Why?) Remove the unnecessary prefixes.

When many methods or instance variables share a prefix, that is often a sign that there is an abstraction waiting to emerge. In this case, the abstraction was the “`Weapon`” interface.

Tip: **Use your IDE to help with renaming!**  Right-click one of those names, and choose “Rename symbol.” This renames it everywhere at once. Much easier than fixing each occurrence by hand!

## Step 4: Check

Tests all passing? Run them again to double check.

Look back at `Game` where it creates the characters. Is the code more readable now? Anything you can do to tidy it up?

Try playing the game again by running `Game`’s `main` method. Everything working?

When you’re done (or if you get stuck), compare your solution to the [official solution](https://github.com/mac-comp127/rpg-battle-solution/commit/4b3194bf9aa355961abcdd978723209e2bf69122). Again, your solution might not be identical! If you aren’t sure whether that’s OK, check with the instructor or a preceptor.

## Don’t forget your reflection!

The self-evaluation rubric for this assignment is in `SELF_EVALUATION.md`.

See the [Take-Home Exercise Procedure](https://comp127.innig.net/resources/take-home-procedure/#self-evaluation) for full instructions.

## Bonus step: Expand the game <aside>(optional)</aside>

Now, add some of your own attack types! Suggestions:

- `Bow`: Shoots an arrow at the target for 10 damage, but is limited by the number of arrows a character owns. When the character has zero arrows, this attack doesn’t work!
- `Drain`: Uses 5 energy from the attacker, does 20 damage to the target, and restores 20 health to the attacker.
- `Heal`: Uses 5 energy from the “attacker” and restores 20 health to the target. This one is especially tricky. It requires some restructuring of the game UI, since right now it won’t let you attack your own party! (Hmmmmm…strictly speaking, healing is not a weapon or an attack. Does that interface need a different name?)
