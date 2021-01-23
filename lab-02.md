Lab 02
================

-   [Overview](#overview)
-   [Making your first R Markdown
    document](#making-your-first-r-markdown-document)
    -   [Step 1: Get ready to work](#step-1-get-ready-to-work)
    -   [Step 2: Practice with RStudio’s boilerplate R Markdown
        document](#step-2-practice-with-rstudios-boilerplate-r-markdown-document)
    -   [Step 3: Swap out the “guts” of the
        document](#step-3-swap-out-the-guts-of-the-document)
    -   [Step 4: Develop your report](#step-4-develop-your-report)
-   [Create a bio page](#create-a-bio-page)
-   [But I want to do more!](#but-i-want-to-do-more)
-   [Tips and tricks](#tips-and-tricks)

## Overview

In this lab, you will author an R Markdown document and render it to
HTML.

In case it’s helpful, here is the official R Markdown documentation:

-   <http://rmarkdown.rstudio.com>

## Making your first R Markdown document

### Step 1: Get ready to work

If you haven’t already done so, launch RStudio and create an R project
that corresponds to this lecture/lab. Make sure the workspace is clean
and you’ve launched a fresh R process.

### Step 2: Practice with RStudio’s boilerplate R Markdown document

First, we will test our system’s ability to render the [“hello
world”](http://en.wikipedia.org/wiki/%22Hello,_world!%22_program) of R
Markdown documents before we muddy the waters with our own, probably
buggy, documents.

Do this: *File &gt; New File &gt; R Markdown …*

-   Give it an informative title. This will appear in the document but
    does not necessarily have anything to do with the file’s name. But
    the title and filename should be similar! The title is for human
    eyeballs, so it can contain spaces and punctuation. The filename is
    for humans and computers, so it should have similar words in it but
    no spaces and no punctuation.
-   Accept the default Author or edit if you wish.
-   Accept the default output format of HTML.
-   Click OK.

Save this document to a reasonable filename and location. The filename
should end in `.Rmd` or `.rmd`. For now, I recommend saving in the
top-level of the directory of your RStudio project.

Click on “Knit HTML” or do *File &gt; Knit Document*. RStudio should
display a preview of the resulting HTML. Also look at the file browser
(which should be pointed at the directory where you saved the `.Rmd`
file). You should see the R Markdown document, i.e. `foo.Rmd` AND the
resulting HTML `foo.html`.

Congratulations, you’ve just made your first reproducible report with R
Markdown.

### Step 3: Swap out the “guts” of the document

Select everything but the YAML frontmatter and … delete it!

Write a single English sentence.

Insert an empty R chunk, via the “Chunk” menu in upper right of source
editor or with corresponding keyboard shortcut (Cmd/Ctrl + Alt + I).

``` r
## insert your brilliant WORKING code here
```

Insert 1 to 3 lines of functioning code that begin the task at hand.
“Walk through” and run those lines using the “Run” button or the
corresponding keyboard shortcut. You MUST make sure your code actually
works!

Satisfied? Save!

Now render the whole document via “Knit HTML.” Voilà!

### Step 4: Develop your report

In this incremental manner, develop your report. Add code to this chunk.
Refine it. Add new chunks. Go crazy! But keep running the code
“manually” to make sure it works. When I say manually, I mean running
your code by sending it to the R console. This helps you debug problems.
If your code doesn’t work when you send it to the console, I can
guarantee you it will fail, in a more spectacular and cryptic way, when
run at arms-length via “Knit HTML”.

Clean out your workspace and restart R and re-run everything
periodically, if things get weird. There are lots of chunk menu items
and keyboard shortcuts to accelerate this workflow. Render the whole
document often to catch errors when they’re easy to pinpoint and fix.
Remember to keep saving your document.

You’ll develop your own mojo soon, but this should give you your first
successful R Markdown experience.

## Create a bio page

Use R Markdown to create a bio page that introduces yourself to the
class. Add a picture (or gif or two) and link to your labs’ website.

In your page you should try and experiment with 4 or more aspects of the
Markdown syntax. Examples: section headers, links, bold, italic, bullet
points, images, gifs, emojis 😎 , etc. Experiment and have some fun.
Remember to document your process so you can come back to this file
later and use it as a resource.

I’ll ask a few of you to share your work at the end of the lab session.

## But I want to do more!

If you finish up everything above, you can start working on [homework
1](hw01.md).

## Tips and tricks

**If you are writing R code chunks in your R Markdown docuemnt, check
those code chunks work by sending them to the console.** This will help
you debug your code and will avoid things breaking when you are ready to
`knit` your document.

**Your document won’t knit if you have errors in your R code. However,
if you want to tolerate errors in one specific chunk, you can do that.**
You set this for a specific code chunk like so:

    ```r
    ## your sketchy code goes here ;) 
    ```

This lets you see what your document looks like without worrying about
errors in your R code.

**Don’t be in a hurry to create a complicated sub-directory structure.**
But Will, you spent a lot last lecture telling us to create
sub-directories to organize our projects!!! I still think you should to
that, just not today :-)

RStudio/`knitr`/`rmarkdown` (which bring you the “Knit HTML” button) are
rather opinionated about the working directory being set to the `.Rmd`
file’s location and about all files living together in one big happy
directory. This can all be worked around.

In the coming weeks, if you have a moment a boredom, create
sub-directory with an R Markdown file in it. Put an image in your the
root folder of your R Project and try and get that image into your
rendered R Markdown file. You might experience a bit of frustration.
That’s great! Do some googling and see what you can figure out. Then
take a look at the [here package](https://here.r-lib.org/). If `here()`
doesn’t help you out, email me and we can set up an office hour to chat
about R Markdown and sub-directories.
