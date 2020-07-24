Libgdx provides methods to playback small sound effects as well as stream larger music pieces directly from disk. It also provides convenient read and write access to the audio hardware.

All access to the audio facilities is done through the [audio module](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/Audio.html), referenced by:

```java
Audio audio = Gdx.audio;
```

Libgdx will automatically pause and resume all audio playback for you if your application is paused and resumed.