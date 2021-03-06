当你遇到困难或 bug 时，libgdx 社区非常乐意伸出援手，但首先你应该知道如何高效地获得帮助

## 内容

 * [自己查找解决办法](#helping-yourself)
 * [Topic Title](#top-title)
 * [Context](#context)
 * [Relevance](#relevance)
 * [Problem Statement](#problem-statement)
 * [Exceptions](#exceptions)
 * [Code Snippets](#code-snippets)
 * [Executable Example Code](#executable-example-code)
   * [Example Resources](#example-resources)
   * [Application](#barebones-application)
   * [SpriteBatch](#barebones-spritebatch)
   * [Barebones Stage](#barebones-stage)
 * [Actually Executable](#actually-executable)
 * [Attitude](#attitude)
 * [Formatting](#formatting)

## <a id="Helping_Yourself"></a>自己查找解决办法 ##

请先看看下面几条建议，你可能会找到问题的解决办法

  * 你有没有在用 libgdx 最新的 [nightly 版本](http://libgdx.badlogicgames.com/download.html)？你可以试一试，因为我们每天都在修 bug。
  * 你可以先浏览一遍[wiki 主页](./Home.md)上的条目。也可以去查找官方的 [API 文档](http://libgdx.badlogicgames.com/nightlies/docs/api/)或参考[源码](https://github.com/libgdx/libgdx)。也可以在[这里](https://github.com/libgdx/libgdx/tree/master/tests/gdx-tests/src/com/badlogic/gdx/tests)搜索具体类的示例代码
  * 你可以试试在论坛内[搜索](http://www.badlogicgames.com/forum/search.php)你的问题
  * 你可以在[issue tracker](https://github.com/libgdx/libgdx/issues)搜索你的问题。注意要在所有的问题(All issues)中搜索，不要只搜索仍然开放的问题(Open issues)

如果你的问题还没有解决，通常最快的方法就是去 libgdx 的聊天室。一般聊天室内会有不到100个人在线，他们都可以回答你的问题。聊天室在这里 [#libgdx on irc.freenode.net](irc://irc.freenode.net/libgdx)。

另一个可以寻求帮助的地方就是 [libgdx discord](https://discord.gg/6pgDK9F)。

除此之外，如果你想在[论坛](http://www.badlogicgames.com/forum/)内发帖，或在 [tracker](https://github.com/libgdx/libgdx/issues) 内发起一个新的问题，请往下阅读。

## <a id="Help_us_help_you"></a>让我们更好的帮助你 ##
如果你确信你的问题，错误或疑似 bug 是平台相关的，请在展示你的问题时，一并带上以下的信息。如果你正在 IRC 内，也请准备好以下的信息。

**Android 平台上的问题**
 - **Note:** Android issues can sometimes be more difficult due to device manufactures breaking things or buggy drivers.

- **Note 2:** LibGDX only works completely on the Android Runtime (ART) on devices running the Android L developer preview and higher. Some functions will not work on the ART builds in 4.4.x due to an issue in ART that was fixed and included in the L developer preview.

- Please have the device name and Android version in the bug report. Providing things won't be broken, we will make an attempt to fix the issue or implement a workaround for the device.

**For Desktop backend issues (jglfw, lwjgl)**
 - Please list the operating system and version, architecture, and if necessary OpenGL version.
 - Also mark specifically which of those backends have this issue.

**For iOS (RoboVM) backend issues**
 - Please list the iOS version, and device the issue occurs on

**For GWT (WebGL) backend issues**
 - Please list the operating system and version, and architecture.
 - Please list the browser and browser version
Listing this information can greatly reduce the workload on us and can greatly increase the chances your issue will be resolved or an available fix or workaround implemented.

## <a id="Topic_Title"></a>Topic Title ##

Write a clear and short topic title. Titles that do not describe the topic (such as "please help") or contain all caps, exclamation marks, etc make it much less likely that your post will be read.

## <a id="Context"></a>Context ##

Describe what you are trying to achieve. If it might help your question get answered, also explain the reasons why. If specific solutions are unacceptable, list them and why.

Try to keep the information relevant. If you aren't sure, include the extra information but if your text gets very long, provide an executive summary separate from the rest.

## <a id="Problem_Statement"></a>Problem Statement ##

Concisely describe the problem. Describe each approach you have tried and, for each of those, explain what you expected and what actually happened.

If you fail to do this, likely you will be ignored. No one wants to guess what your problem is and often they don't have the time or patience to ask for the information you should have included from the start.

## <a id="Exceptions"></a>Exceptions ##

If an exception occurred, include the full exception message and stack-trace.

Often the first line number just after the deepest (nearest to the bottom) exception message is most relevant. If this line is in your code, along with the full exception message and stack-trace you should include your code for this line and 1-2 surrounding lines.

```
Exception in thread "LWJGL Application" com.badlogic.gdx.utils.GdxRuntimeException: java.lang.NullPointerException
	at com.badlogic.gdx.backends.lwjgl.LwjglApplication$1.run(LwjglApplication.java:111)
Caused by: java.lang.NullPointerException
	at com.badlogic.gdx.scenes.scene2d.ui.Button.setStyle(Button.java:155) <- **MOST IMPORTANT LINE**
	at com.badlogic.gdx.scenes.scene2d.ui.TextButton.setStyle(TextButton.java:55)
	at com.badlogic.gdx.scenes.scene2d.ui.Button.<init>(Button.java:74)
	at com.badlogic.gdx.scenes.scene2d.ui.TextButton.<init>(TextButton.java:34)
	at com.badlogic.gdx.backends.lwjgl.LwjglApplication.mainLoop(LwjglApplication.java:125)
	at com.badlogic.gdx.backends.lwjgl.LwjglApplication$1.run(LwjglApplication.java:108)
```

## <a id="Code_Snippets"></a>Code Snippets ##

Code snippets are most often not very useful. Unless you are blatantly misusing the API, most problems cannot be solved just by looking at a code snippet. Code snippets mostly lead to vague guesses at what might be wrong instead of a real answer to your question. Instead, include executable example code.

## <a id="Executable_Example_Code"></a>Executable Example Code ##

Example code that can be copied, pasted, and run is the best way to get help. It saves those helping you time because they can see the problem right away. They can quickly fix your code or fix the bug, verify the fix, and show you the result. No matter what, an executable example has to be written to properly test, fix, and verify the fix. If you can't debug and fix the problem yourself, you can still help by providing the executable example.

Creating executable example code does take some time. You need to take apart your application and reconstruct the relevant parts in a new, barebones application that shows the problem. Quite often just by doing this you will figure out the problem. If not, you will get help very quickly and the people helping you will have more time to help more people.

Example code should be contained entirely in a single class (use static member classes if needed) and executable, meaning it has a main method and can simply be copied, pasted, and run. Do not use a GdxTest, as that cannot be copy, pasted, and run.

For more about how to make executable example code, please see [SSCCE](http://sscce.org/) and [MCVE](http://stackoverflow.com/help/mcve).

### <a id="Example_Resources"></a>Example Resources ###

Often executable examples need some resources, such as an image or sound file. It is extra work for those trying to help if they must download your specific resources. Instead, it is ideal to use resources from the [libgdx tests](https://github.com/libgdx/libgdx/tree/master/tests/gdx-tests-android/assets). This enables your example code to be simply pasted into the `gdx-tests-lwjgl` project and run.

The easiest way to write an executable example is to paste one of the barebones applications below into the `gdx-tests-lwjgl` project and then modify it to show your problem, using only the [test resources](https://github.com/libgdx/libgdx/tree/master/tests/gdx-tests-android/assets). Note the test resources are pulled in by `gdx-tests-lwjgl` from the `gdx-tests-android` project.

### <a id="barebones_Application"></a>Barebones Application ###

Below is a simple, barebones, executable application. This can be used as a base for creating your own executable example code.

```java
import com.badlogic.gdx.*;
import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.graphics.GL20;

public class Barebones extends ApplicationAdapter {
	public void create () {
		// your code here
	}

	public void render () {
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
		// your code here
	}

	public static void main (String[] args) throws Exception {
		new LwjglApplication(new Barebones());
	}
}
```

### <a id="barebones_SpriteBatch"></a>Barebones SpriteBatch ###

This barebones application uses SpriteBatch to draw an image from the `gdx-tests-lwjgl` project.

```java
import com.badlogic.gdx.*;
import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.graphics.*;
import com.badlogic.gdx.graphics.g2d.SpriteBatch;

public class BarebonesBatch extends ApplicationAdapter {
	SpriteBatch batch;
	Texture texture;

	public void create () {
		batch = new SpriteBatch();
		texture = new Texture(Gdx.files.internal("data/badlogic.jpg"));
	}

	public void render () {
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
		batch.begin();
		batch.draw(texture, 100, 100);
		batch.end();
	}

	public static void main (String[] args) throws Exception {
		new LwjglApplication(new BarebonesBatch());
	}
}
```

### <a id="barebones_Stage"></a>Barebones Stage ###

This barebones application has a [[scene2d]] Stage and uses [[scene2d.ui]] to draw a label and a button. It uses the [[Skin]] from the `gdx-tests-lwjgl` project.

```java
import com.badlogic.gdx.*;
import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.graphics.GL20;
import com.badlogic.gdx.scenes.scene2d.Stage;
import com.badlogic.gdx.scenes.scene2d.ui.*;
import com.badlogic.gdx.utils.viewport.*;

public class BarebonesStage extends ApplicationAdapter {
	Stage stage;

	public void create () {
		stage = new Stage(new ScreenViewport());
		Gdx.input.setInputProcessor(stage);

		Skin skin = new Skin(Gdx.files.internal("skin.json"));
		Label label = new Label("Some Label", skin);
		TextButton button = new TextButton("Some Button", skin);

		Table table = new Table();
		stage.addActor(table);
		table.setFillParent(true);

		table.debug();
		table.defaults().space(6);
		table.add(label);
		table.add(button);
	}

	public void render () {
		Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
		stage.draw();
	}
	
	public void resize (int width, int height) {
		// Pass false to not modify the camera position.
		stage.getViewport().update(width, height, true);
	}

	public static void main (String[] args) throws Exception {
		new LwjglApplication(new BarebonesStage());
	}
}
```

## <a id="Actually_Executable"></a>Actually Executable ##

If your executable example cannot be pasted into the `gdx-tests-lwjgl` project and run, then it is not actually an executable example. Others should not have to fix up your code to run it, not even to add a main method.

## <a id="Attitude"></a>Attitude ##

Begging for help or a quick answer tends to turn people off and makes it less likely you will receive help at all. Just be polite and your question will get answered politely as time allows. If you are rude, you will be ignored or met with rudeness in return. The people helping you are busy and providing you help for free simply because they are nice. They don't owe you anything and they don't have to care about you or your problems.

## <a id="Formatting"></a>Formatting ##

If you spend a little bit of your time to format your post nicely, it is more likely others will spend their time responding to your post. This means capital letters where appropriate, paragraphs to separate ideas, use actual words (rather than "u", "bcoz", etc), put code in code blocks, etc. If English is not your first language, we understand. No need to apologize, just do your best to make an effort.