---
layout: post
title: "Creating Interactive Slideshows in Jupyter Notebooks"
date: 2019-05-23
preview_image: "/assets/jupyter-notebooks-logo.png"
categories: posts
---

Have you ever wanted to create a slideshow using Python? I've found a lot of useful tools for making slideshows in Jupyter Notebooks while developing Python for data science workshops for the University of Cincinnati and <a href="http://www.8451.com/" target="_blank" class="class2">84.51°</a>, but I'm yet to see all of this information in one place. This blog post changes that by directly teaching you how to create interactive slideshows in Jupyter Notebooks. 

This post covers the benefits of using Python and Jupyter to create slideshows and how to set up the technology, create a slideshow, and view/operate the created slideshow.

<br>

### Why to create slideshows in Python and Jupyter

Using Python and Jupyter Notebooks to create slideshows has many benefits:

  * Version control your slideshows
  * Make your slideshows dynamic
  * Share your code in group settings
  * Maintain a single analysis/presentation document

While there are many other benefits to using Python and Jupyter Notebooks to create, these reasons alone are rather convincing - especially given how easy creating the slideshows is.

<br>

### What you need to already know

Creating slideshows is easy, but only if you know a few of the basics. This tutorial assumes you have a working knowledge of basic Python, Jupyter Notebooks, and the command line. In addition, it will be helpful to have experiences installing and working with Jupyter Notebooks extensions and Python packages.

If you don't already have this knowledge, that's alright - you should still be able to follow most of this tutorial if you have Python and Jupyter Notebooks installed.

<br>

### How to set up the tech

#### Installing Python and Jupyter Notebooks

If you don't already have Python and Jupyter Notebooks installed, the first thing you'll need to do is install them. I recommend using Anaconda Navigator for a fast and easy setup. Anaconda's <a href="https://docs.anaconda.com/anaconda/install/" target="_blank" class="class2">installation instructions</a> are easy to follow.

#### Installing RISE

<a href="https://github.com/damianavila/RISE" target="_blank" class="class2">RISE</a>, developed by Damian Avila, is the best slideshow engine I've found for Python and Jupyter Notebooks. It stands for Reveal.js IPython/Jupyter Slideshow Extension.

In order to install RISE, you'll need to use the command line. If you've installed Anaconda, you can use:

```bash
conda install -c conda-forge rise
```

Otherwise, you can install with `pip`:

```bash
pip install RISE
```

You won't interact directly with RISE. Instead, you'll be able to access it through Jupyter Notebooks.

<br>

### How to create a slideshow

#### Enabling slideshow mode

In order to create a slideshow, you'll need to start Jupyter Notebooks and open a new notebook (note that you must do this after you've installed RISE). Once you have a fresh Jupyter Notebook, you'll need to enable the slideshow. You can do this by doing the following:

1. Click on "View" in the Jupyter toolbar
2. Hover over "Cell Toolbar" in the "View" menu
3. Click on "Slideshow" in the "Cell Toolbar" menu

<center>
<img src="/assets/jupyter-slideshow-mode.png" height="300">
</center>
<br>

You've now enabled slideshow mode. 

#### Creating the slides with cells

At this point, you should you a cell toolbar with a dropdown menu on the right-side:

<center>
<img src="/assets/jupyter-slideshow-toolbar.png" height="150">
</center>
<br>

You should see six options here. This dropdown and its options determine how each cell fits into the slideshow. These The options and their descriptions are below:

1. **slide** - indicates that the selected cell should be the start of a new slide
2. **sub-slide** - indicates that the selected cell should be the start of a new sub-slide, which appears in a new frame beneath the previous slide
3. **fragment** - indicates that the selected cell should appear as a build to the previous slide
4. **skip** - indicates that the selected cell should be skipped and not be a part of the slideshow
5. **notes** - indicates that the selected cell should just be presenter notes
6. **-** - indicates that the selected cell should follow the behavior of the previous cell, which is useful when a markdown cell and a code cell should appear simultaneously

Each of these options can include Python code or markdown/HTML/LaTeX code like a traditional Jupyter Notebook.

<br>

### Viewing and operating the slideshow

#### Viewing the slideshow

Once cells have been used to create slide material for the slideshow, the slideshow can be viewed from directly within the notebook.

There are two options to view the slideshow:

1. Using the <kbd>OPTION</kbd> + <kbd>R</kbd> shortcut (<kbd>ALT</kbd> + <kbd>R</kbd> on Windows) to enter and exit presentation mode from within the notebook
2. Clicking the "Presentation Mode" button frmo within the notebook - this will only appear if you've installed RISE

<center>
<img src="/assets/jupyter-slideshow-presentation-button.png" height="90">
</center>
<br>

After entering presentation mode, you should see a screen that looks like this:

<center>
<img src="/assets/jupyter-slideshow-active.png" height="400">
</center>
<br>

This means the presentation is active.

#### Operating the slideshow

*Changing slides*

While it may be tempting to use the <kbd><-</kbd> and <kbd>-></kbd> keys to change slides in the slideshow, this will not fully work - it will skip the cells marked as sub-slides. Instead, <kbd>SPACE</kbd> should be used to move the slideshow forward and <kbd>SHIFT</kbd> + <kbd>SPACE</kbd> should be used to move the slideshow backward.

There are many other keyboard shortcuts that can be accesed within the slideshow by clicking the question mark in the bottom-left corner.

*Running code and editing on-the-fly*

One of the great things about RISE is that it work in a live Python session - that means you can edit and run code while the presentation is running!

If a code cell is marked as slide, sub-slide, fragment, or -, it will appear in the slideshow as editable and runnable. Here's an example:

<br>
<center>
<img src="/assets/jupyter-slideshow-live.gif" height="400">
</center>
<br>

### Further resources

If you're interested in learning more about creating slideshows in Jupyter, stay tuned for additional posts.

You can also view an <a href="https://mybinder.org/v2/gh/uc-python/intro-python-datasci/master" target="_blank" class="class2">example here</a> of a training slideshow created using Python, Jupyter Notebooks, and RISE.
