This page is a notice on how to edit libgdx wiki pages. Please read this before contributing to the libgdx wiki.


## Questions ##

If you have any questions about how to edit this wiki, please do not hesitate to ask! See our [[community  page | Community & Support]] for more information.

## General Syntax ##

We use Markdown in this wiki, [heres a reference](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). Please learn your way around with this, as to avoid markup differences.

## Style ##

*(This section is mostly just so sinistersnare does not go crazy looking at different syntax styles all the time... 'consistency is key')*

This section is for the markdown that is used, it will have no user facing changes, so does not particularly matter to the lay-reader.

Make sure to end each header with the appropriate amount of pound signs! `### test` is a valid `<h3>` tag in markdown, but it looks much cleaner to do `### test ###` instead. 

Also, after headers, leave a line of white space please.

### Notable syntax ###

* Wiki links are made using `[ [` and `] ]` (without the spaces) with a `|` (pipe) to separate text from link. For example:

`[[link text to simple game | A simple game]]` renders this: [[link text to simple game | A simple game]]  

***Do NOT use conventional \[]() syntax for wiki-links.***

## Linking to code/docs ##
Links are done as follows: `[ClassName](link to docs) [(code)](link to code)` for example:
```
[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html)
[(code)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java) 
```

renders the following:

[Texture](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html)
[(code)](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/graphics/Texture.java) 

Don't use non-alphabetic characters in Wiki page names, because not all operating systems can handle
them when cloning Wiki as Git repository (for example, Windows doesn't support ":").

### Notes on Doc Links ###

* Please note that there should be a space in between `ClassName (Code)` style formatting, in order to differentiate the two.

* Please make the format `ClassName (Code)` with the word `Code`, not `Source` or any derivative of that. Consistency is key!

*  If a link to documentation ends in a right paren `)`, it will mess up the markdown. take this as an example: 

```
http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth()
```

when using the markdown formatting of `[]()` the end paren will mess up the link, so please remember to escape the ending paren (`)`) so for example, it should be :

```
[Link to Texture#getWidth](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth(\))
```

without the escaped paren, a 404 is imminent!

## Videos ##

Videos are not supported on github :( so we use a small workaround by posting a screenshot of the video, which leads to a link to the youtube video. Here is the syntax:

```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="480" height="360" border="10" /></a>
```

Lets hope Github eventually supports embedded videos!

## Sidebar Issues ##

The sidebar used in this wiki causes a few problems with preformatted text. This example will (most likely) be chopped off due to the sidebar: `http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/graphics/Texture.html#getWidth()`
 To work around this issue, one should wrap the large amount of unbroken link in a triple backtick (\```) block and put it on a newline. Check the source of this page, specifically, the 'Notes on Doc Links' section.

## The Main Table Of Contents ##

If you make a page, you will most likely want it to be displayed on the main Libgdx wiki Table of contents and the sidebar Table of Contents. When you create an article, please create a second edit of the Home page with the appropriate positioning of your article. Mirror this change in the sidebar ToC, as to maintain likeness between the two.

### Non pages on the Table of Contents ###

The Table of contents contains a few pages that do not have a link, and are appended with a `??`. This is to signify that during translation from Google Code wiki to Github Wiki, there were a couple of pages without links. If you have something to contribute on the topic of one of those pages without a page (yet!), please feel free to add a page, and add your content, then reflect your changes in the ToC by adding a link.


## Tables of Contents per page ##

Tables of contents have to be manually created on a per-page basis. For an example of how to do so outside of this section, please refer to our [[box2d]] article.

When creating headers in markdown, we specify using a number of octothorpes (`#`) that define the header level. When we create a header `## Comments and Questions/Concerns ##` in an article entitled `Help Me` the corresponding link would be `help-me#comments-and-questionsconcerns` 

so when we go to make our table of contents, it would be in an unordered list, and using these qualified page fragment links. Please see the [[box2d]] article for more information.

## Adding Images ##

Images need to be added manually through a desktop interface (A.K.A. not through the github web interface). Images are stored in the `images/` of the libgdx wiki, which arent accessed through the libgdx wikis github interface. To add an image, you must clone the repo: `$ git clone https://github.com/libgdx/libgdx.wiki.git` add your images to the images folder using the appropriate naming scheme `my-page-name#` where # is the order of the picture displayed on the page (this can be ommitted if only one image is used in the page, but recommended). Images are linked to with the following syntax (assuming the image is stored in the `images/` directory) `[ [images/using-libgdx-with-intellij-idea01.png] ]` (without the spaces in between brackets) which will display:

[[images/using-libgdx-with-intellij-idea01.png]]


## Large Multi-Page Edits ##

Github's web interface is the only way that a non-contributor can easily edit a wiki. If a person is to make a large edit that spans multiple pages, it can be done via the web interface, but it is recommended to take the following steps:

* Fork the repo
* Clone your forked repo locally
* Make necessary changes
* Commit and push that to your forked repo
* Email a libgdx maintainer (who has commit rights) to clone your repo and push it themselves.

Github does NOT have a robust pull request system for wiki changes, so this is the way it is for non-trivial changes to the wiki. If you have any problems, **PLEASE** contact support@github.com with your wishlist (hopefully something like "please make github wikis better! $IDEAS"). If you have a problem, it is necessary that you voice yourself to Github!

### Small Typo Fixes ###
Github wiki's diff system is not as robust as the diff system for code. If you make a tiny change, in the commit message please say "Fixed typo alpa -> alpha" or something of the sort.
