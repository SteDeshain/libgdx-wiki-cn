# TODOs Mario
 * Go through all articles one more time and see if all links are working
 * Completed everything up until and including Audio

## Possible Mistakes ##

some of the words (specifically PascalCase'd words) might come with a `!` prefacing them.(for example: !PascalCase) This is due to how Google Code parsed for other wiki pages. the bang stopped the text from being parsed, and i might have missed a lot of them, because they're quite hard to find.

if you see anything like {{{shouldBeCode}}} please change that to \`shouldBeCode\` (backticks!) more things like this are inevitable because im one person, so if you want to help with that, please look up [creole markup syntax](http://code.google.com/p/support/wiki/WikiSyntax) (which is basically what GCode uses) and then [markdown](https://help.github.com/articles/github-flavored-markdown) (what we are using now).

I also left a few articles (in the `deprecated` section) untranslated. This is because i feel like they are no longer in need of being in this repo. As a matter of fact i think that we can remove most, if not all, of the `deprecated` marked articles.

i might have missed a few images... theyre sort of tricky to find, but if you want, you can `grep "*.jpg"` or something like that, im not regex master (if grep uses regex, im no grep master).

## linking to code/docs ##
Also, I've decided (without the approval of nex or badlogic, this can be reversed though, if unliked) to change links to code, to the appropriate link to the docs. There are multiple mentions of code snippets like Input.Keys, which uses a github link to a line. This is all well and dandy, except for the fact that files are volatile in their numbering, someone can add a bunch of javadoc (good!) which can drastically change the line numbering (bad! but understandable!). Documentation is made to solve this problem! Linking to a specific line of code is for short term use only (in my opinion) for the sole reason that lines change all the time. UPDATE!: ive decided to do links such as the following `[ClassDocumentation](link to docs)[(code)](link to code)` for example:
```
[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html)
[(code)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java) 
```

renders the following:

[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html)
[(code)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java) 

_please note that there should be a space in between `Docs (Code)` style formatting, in order to differentiate the two._

## possible fixes and changes

id welcome a `grep fixme` :p

if we decide a way to store all images on the wiki on badlogic servers (to prevent 404s) we can `grep ![]` to find them!


## Misc. ##

* I'm not translating the [[Graphics]] post, because it needs to be rewritten! motivation! (see editors note for repetition)

* I'm also not translating a few of the pages marked `deprecated`, mostly the `beginner` tutorials. I chose not to because they're wildly out of date, and it seemed like it was futile

* Videos are not supported on github :( so we use a small workaround by posting a screenshot of the video, which leads to a link to the youtube video. Here is the syntax:

```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="480" height="360" border="10" /></a>
```

Lets hope Github eventually supports embedded videos!

* some articles on the Table of contents have a '??' appended. the question marks are supposed to represent a possible article that can go there. If you know what your doing/talking-about, please make an article there! you can also remove that if there is not supposed to be an article there!

## issues: ##

 * I'm not sure of anything that resembles the wiki ToC per-page syntax as we had in google code.

 * There is not a good way to show youtube videos in a page. I contacted github about this, i will pursue a quality solution!