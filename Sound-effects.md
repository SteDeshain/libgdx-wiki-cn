Sound effects are small audio samples, usually no longer than a few seconds, that are played back on specific game events such as a character jumping or shooting a gun.

Sound effects can be stored in various formats. Libgdx supports MP3, OGG and WAV files.
RoboVM (iOS) currently does not support OGG files.

***note:*** On Android, a Sound instance can not be over 1mb in size (uncompressed raw PCM size, not the file size). If you have a bigger file, use  [[Music|Streaming music]]

Sound effects are represented by the [Sound](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/audio/Sound.html) interface. Loading a sound effect works as follows:

```java
Sound sound = Gdx.audio.newSound(Gdx.files.internal("data/mysound.mp3"));
```

This loads an audio file called `"mysound.mp3"` from the internal directory `data`.

Once we have the sound loaded we can play it back:

```java
sound.play(1.0f);
```

This will playback the sound effect once, at full volume. The play method on a single `Sound` instance can be called many times in a row, e.g. for a sequence of shots in a game, and will be overlaid accordingly.

More fine-grained control is available. Every call to `Sound.play()` returns a long which identifies that sound instance. Using this handle we can modify that specific playback instance:

```java
long id = sound.play(1.0f); // play new sound and keep handle for further manipulation
sound.stop(id);             // stops the sound instance immediately
sound.setPitch(id, 2);      // increases the pitch to 2x the original pitch

id = sound.play(1.0f);      // plays the sound a second time, this is treated as a different instance
sound.setPan(id, -1, 1);    // sets the pan of the sound to the left side at full volume
sound.setLooping(id, true); // keeps the sound looping
sound.stop(id);             // stops the looping sound 
```

Note that these modifier methods have limited functionality in the JavaScript/WebGL back-end for now. As of 1.9.6 setPan() is only working when flash is supported and enabled `GwtApplicationConfiguration.preferFlash = true`

Once you no longer need a Sound, make sure you dispose of it:

```java
sound.dispose();
```

Accessing the sound after you disposed of it will result in undefined errors.