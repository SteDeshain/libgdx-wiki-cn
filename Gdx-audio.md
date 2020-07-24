**This extension has been removed as per 8th of April 2014**

Gdx-audio is an extension offering a lower-level API for audio. Features are:
* Decoders for mp3, ogg Vorbis and wav, using [Mpg123](http://www.mpg123.de/), [Xiph Tremor](http://wiki.xiph.org/Tremor)
* [KissFFT](http://sourceforge.net/projects/kissfft/)and Java FFT by Damien Di Fede for comparison
* [SoundTouch](http://www.surina.net/soundtouch/)for pitch shifting, time stretching and playback rate modification

It is being distributed with every release in the extensions folder. To use the extension add the gdx-audio.jar and gdx-audio-natives.jar to your desktop project. For your android project add the gdx-audio.jar and copy the libgdx-audio.so files to your libs/armeabi and libs/armeabi-v7a folders. GWT (html) and iOS backends are currently not supported.

For usage examples see:
* [Mpg123Test](http://code.google.com/p/libgdx/source/browse/trunk/tests/gdx-tests/src/com/badlogic/gdx/tests/Mpg123Test.java), shows how to decode an mp3 with the Mpg123Decoder class
* [VorbisTest](http://code.google.com/p/libgdx/source/browse/trunk/tests/gdx-tests/src/com/badlogic/gdx/tests/VorbisTest.java), shows how to decode an ogg with the VorbisDecoder class
* [WavTest](http://code.google.com/p/libgdx/source/browse/trunk/tests/gdx-tests/src/com/badlogic/gdx/tests/WavTest.java), shows how to decode an wav with the WavDecoder class
* [SoundTouchTest](http://code.google.com/p/libgdx/source/browse/trunk/tests/gdx-tests/src/com/badlogic/gdx/tests/SoundTouchTest.java), shows how to apply pitch shifting to a PCM stream

Caveat: The vorbis and mp3 decoder can only decode files stored on the external storage. There might be the possibility to work around that limitation in the future. For most practical purposes it shouldn’t be too limiting.