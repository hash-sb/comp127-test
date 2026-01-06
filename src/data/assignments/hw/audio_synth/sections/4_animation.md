# Part 4: Animation the visualization

Your goal: make the music scroll as it plays.

---

We’d like to make the visualization move as the music plays, so that we can always see the currently
playing notes, but we have a tricky problem: how do we make the animation and the audio stay in sync?
This isn’t as easy as it might sound: Maybe the audio doesn’t start instantly. Maybe the Kilt
Graphics `animate` callback isn’t called with perfect regularity. Small timing errors accumulate,
and the audio and the animation start to drift apart....

Fortunately, `AudioBuffer` already solves the hard part for us: when you tell it to play, you can
provide a callback closure, and `AudioBuffer` will repeatedly call the closure with an accurate
report of the audio playback clock. We’ll use that instead of `animate`. We just need to wire it up
to our visualization.

---

Add a new method to `SongVisualization`:

```
/**
 * Moves the visualization to show that the given time is the current time.
 *
 * @param seconds Time from the beginning of the song
 * @param done    True if the song is done playing
 */
public void setTime(double seconds, boolean done) {
    // TODO: move all notes horizontally (see instructions below)
}
```

What we want to do is make all the notes move leftwards as time passes. But how to do that?

One way would be to loop over all the rectangles that represent notes, and adjust all of their
positions. But there is a _much_ simpler way!

Think about it, then…

<details>
  <summary>
  …expand this to reveal the answer.
  </summary>

  All of the notes are in a graphics group. Calling `setPosition` on the whole group moves all of the
  notes at once! No looping necessary.

  In your implementation of `setTime`, make `SongVisualization` set its own position so that it moves
  left as time passes, at the appropriate speed.

  <details>
  <summary>
  Hint about how to do that
  </summary>

  Remember that `SongVisualization` is a `GraphicsGroup`. You want to update the x coordinate of the
  graphics group.

  Your `setTime` method takes a parameter that says how much time has passed. You will need to do a
  unit conversion on that parameter, and negate it so that it moves left.
  </details>
</details>

---

Once you have that working, in the `main()` method in `AudioSynth`, change the last line that plays
the music so that it calls your new `setTime()` method as the music plays. The `play` method optionally takes one argument, `playingCallback`. You want to turn the `setTime` method of `visualization` into a closure / lambda, and pass that as that `playingCallback` argument.

How do you do that? Think, then…

<details>
  <summary>
  …here’s the answer.
  </summary>

  ```
  song.renderAudio().play(visualization::setTime);
  ```
</details>

Try it! It’s pretty awesome when it works. 😎

Be sure to try your visualization with both `bach.csv` and `kondo.csv`.

---

If you are looking for a little extra challenge, and a little extra dose of UI layout principles…
<details>
<summary>
…here is a minor bonus challenge.
</summary>

It’s a little problematic for `SongVisualization` to be updating its own position. To make UI layout
coherent, we usually prefer for a UI element’s _container_ to be in charge of its position. For
example, suppose we made our sing visualizer part of a larger, more complex UI with some buttons
above it, such that the visualization is now placed a little bit lower and to the right within the
window. But then as soon as we start playing, it resets its own position and leaps back to the
upper left!

To fix this, a more robust solution would be to create a _second_ `GraphicsGroup` named `nodeGroup`
inside `SongVisualization`, and add all the notes to _that_ group. Then the inner group can move while
the `SongVisualization`’s position stays fixed. For a nice little challenge, try that.
</details>

If you are looking for a _bigger_ challenge, and something that looks _totally awesome_, then
proceed to the next part!

Next: [Note highlighting (bonus)](5_highlight)
