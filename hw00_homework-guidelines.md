General Homework Guidelines
================

### Authoring R Markdown files

You will author your homework assignments in [R
Markdown](http://rmarkdown.rstudio.com), which is like Markdown, but
with the addition of R “code chunks” that are runnable. The filename
should end in `.Rmd` or `.rmd`. RStudio’s “Knit HTML” button will
compile the open document to actual HTML and open a preview.

Please knit your R markdown documents to HTML. To do this, you need to
include the following line in your YAML: `output: html_document`

If you need a refresher on R Markdown, visit RStudio’s [R Markdown
website](http://rmarkdown.rstudio.com).

### Which files to submit

-   Everything in your project folder.
-   But you should make it easy for your TA to know which files to focus
    on (see the section [Make your work shine](#make-your-work-shine)).

### How to submit homework

-   To submit your homework, you are going to submit a OneDrive link to
    the folder containing your R project.
-   Before you do this, make sure you have saved all the files
    associated with your solution locally.
-   To create a OneDrive link, follow the instructions
    [here](https://support.microsoft.com/en-us/office/share-onedrive-files-and-folders-9fcc2f7d-de0c-4cec-93b0-a82024800c07).
-   When creating the link, you will be asked who you would like the
    link the to work for. Please select *“People in your organization”*.
    This will help facilitate sharing your project with your peers.
-   Submit your assignment by pasting the link into the text box that is
    part of the assignment submission page on Sakai.

### Make your work shine!

Here are some minor tweaks that can make a big difference in how awesome
your product is.

#### Make it easy for people to access your work

Reduce the friction for the TA to get the hard-working source code (the
R markdown) **and** the front-facing report (HTML).

-   Use file names that make it easy for the TA to find the files you
    want them to be looking at.  
-   In most cases, the TA will be looking for:
    -   Your main R markdown document.
    -   The final pretty HTML report.
-   If your process is very complex (e.g., several R Markdown files),
    consider including a `README.html` file. You should write this file
    using R Markdown.
-   A `README.html` might be useful if you want the TA to look at
    several different Rmd and output files (i.e., HTML). You can use the
    README to describe which files they should be looking at and in what
    order.

#### Make it easy for others to run your code

-   In exactly one, very early R chunk, load any necessary packages, so
    your dependencies are obvious.
-   In exactly one, very early R chunk, import anything coming from an
    external file. This will make it easy for someone to see which data
    files are required, edit to reflect their locals paths if necessary,
    etc (hopefully not necessary).
-   Restart R so you get a new RStudio session and try to knit your R
    markdown file. Does it “just work”? It should!

#### Make pretty tables

Instead of just printing an object with R, you could format the info in
an attractive table. Some leads:

-   The `kable()` function from the `knitr` package.
-   If you want to get real fancy, check out the [gt
    package](https://gt.rstudio.com/)

### Rubrics

A very basic rubric will typically be provided for each homework. This
will provide guidance on how to do well on the homework. You can also
consult the [general homework rubric](general-rubric.md) to get a sense
of the way we will be evaluating your work. The [general homework
rubric](general-rubric.md) will also be useful for evaluating your
classmates work in the peer review.

### Report your process

I’d like you to end each homework with a section that reflects on what
was hard/easy, problems you solved, helpful tutorials you read, etc.

We are interested in your journey so tell us about it. Tell us where you
started in term of your skill level and what you learned by the end of
the homework. If you show me you’re pushing yourself to learn, you’ll do
well in the homework assignments.
