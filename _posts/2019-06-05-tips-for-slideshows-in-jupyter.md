---
layout: post
title: "Tips for Creating Slideshows in Jupyter"
date: 2019-06-05
preview_image: "/assets/jupyter-notebooks-logo.png"
categories: posts
---

In a <a href="https://www.markroepke.me/posts/2019/05/23/creating-interactive-slideshows-in-jupyter.html" target="_blank" class="class2">previous post</a> I wrote about how to create interactive slideshows in Jupyter Notebooks. It covered the benefits, basic technology, basic slide creation, and how to view and operate the slideshow. This post dives a little deeper by providing key tips on creating better-looking slides, customizing formatting, exporting to HTML, hosting slides live online, etc.

<br>

### Tip 1: Split slides into two columns

A built-in option in slideshow platforms like PowerPoint is to split content into two columns. While this isn't a default capability in Jupyter slideshows, it can be easily added using with the Jupyter extension `splitcell`. 

#### Installing and enabling `splitcell`

`splitcell` can be installed by running the following on the command line:

```bash
conda install -c conda-forge jupyter_contrib_nbextensions
```

or:

```bash
pip install jupyter_contrib_nbextensions
```

And then it can be enabled by running:

```bash
jupyter nbextension enable splitcell/splitcell
```

More information on installing/enabling Jupyter extensions is available on <a href="https://github.com/ipython-contrib/jupyter_contrib_nbextensions" target="_blank" class="class2">GitHub</a>.

#### Using `splitcell`

Once installed and enabled, `splitcell` is easy to use. With an open notebook and a cell selected, press the <kbd>SHIFT</kbd> + <kbd>S</kbd> keys to "split" the cell and align it to a certain column. By default, the first cell will align to the left side, and then the right side will be filled by pressing <kbd>SHIFT</kbd> + <kbd>S</kbd> on subsequent cells.

When the notebook is then put into presentation mode, the cells with stay in their columns giving two-column content on slides. The left column will be processed before the right column (but they can appear at the same time by selecint "-" in the right cell slideshow toolbar).

Below is an example of `splitcell` in action in a slideshow:

<br>
<center>
<img src="/assets/jupyter-splitcell.gif" height="400">
</center>
<br>

A major advantage of creating columns using `splitcell` is that it works for both code and HTML/markdown content.

<br>

### Tip 2: Hide code from slideshow

I often find that I want to hide the Python code used to generate output from a slideshow. A common example of this is hiding the code used to print a `pandas` DataFrame when only interested in talking about data.

If you want to hide all of the code within a cell but still run the cell and print its output to a slideshow, you can use the following Python function:

```python
def hide_code_in_slideshow():   
    from IPython import display
    import binascii
    import os
    uid = binascii.hexlify(os.urandom(8)).decode()    
    html = """<div id="%s"></div>
    <script type="text/javascript">
        $(function(){
            var p = $("#%s");
            if (p.length==0) return;
            while (!p.hasClass("cell")) {
                p=p.parent();
                if (p.prop("tagName") =="body") return;
            }
            var cell = p;
            cell.find(".input").addClass("hide-in-slideshow")
        });
    </script>""" % (uid, uid)
    display.display_html(html, raw=True)
```

All you have to do is import the function or define it within the slideshow notebook, and then call `hide_code_in_slideshow()` from within any cell whose code you wish to hide. The code will still appear in the Notebook, but will not be present in the slideshow.

Below is an example of `hide_code_in_slideshow()` in action:

<br>
<center>
<img src="/assets/jupyter-hide-code-gif.gif" height="400">
</center>
<br>

I find `hide_code_in_slideshow()` particularly useful when delivering a training session or delivering results to non-technical audiences.

<br>

### Tip 3: Add speaker notes

In my opinion, one of the most desirable slideshow features is the ability to include speaker notes. This functionality is built into RISE and is really easy to use.

In order to add speaker notes to a slide, you should create a new markdown cell directly beneath the final cell that is part of said slide and select 'Notes' from the cell toolbar. Next, you can type any of notes you'd like in the cell.

In order to activate the notes in a RISE slideshow, you can press the <kbd>T</kbd> key to toggle the speaker notes window. A new window will pop up that contains the live slideshow, a view of the next slide, and the speaker notes associated with the current slide.

Below is an example of RISE's speaker notes functionality:

<br>
<center>
<img src="/assets/jupyter-speaker-notes.gif" height="400">
</center>
<br>

The best part about this separate speaker window is that it's linked to the original slideshow window - this means you can extend your screen when presenting and have the speaker window on your laptop and the original slideshow window projecting to a larger screen.

<br>

### Tip 4: Turn your slides into a writable chalkboard

The next tip is another feature built directly into RISE: the ability to write over top of a slide and launch a blank chalkboard. This is particularly useful during training and teaching sessions.

RISE's chalkboard feature can be enabled by adding the below data to the Jupyter Notebook's metadata. You can edit the Jupyter Notebook metadata by clicking on "Edit" in the Jupyter Notebooks toolbar, and then clicking on "Edit Notebook Metadata".

```
{
  ...,
  "rise": {
      "enable_chalkboard": true
  }
}
```

Once added to the notebook metadata, the Jupyter Notebook must be shutdown and restarted to launch with the chalkboard feature enabled.

When the notebook is relaunched and presentation mode is enabled, you'll see two new buttons in the bottom-left corner of the slide:

<br>
<center>
<img src="/assets/jupyter-slide-chalkboard-enabled.png" height="100">
</center>
<br>

As mentioned, the chalkboard feature provides two core funcationalities: *writing on top of a current slide* and *launching a blank chalkboard*. The button on the right enables writing on top of the current slide and the button in the middle launches a blank chalkboard.

Here's a demonstration of writing on top of the current slide:

<br>
<center>
<img src="/assets/jupyter-write-current-slide.gif" height="400">
</center>
<br>

The chalkboard simply launches a blank black chalkboard with the ability to write using virtual chalk.

<br>

### Tip 5: Customize slide appearance with custom CSS

By default, slides appear as white with a basic black text. However, there is often a desire to customize the background, fonts, images, or headers and footers of slides. This can be achieved with CSS.

#### Adding default CSS

A default CSS can be added to all notebooks within a directory by creating a CSS file titled `rise.css` within the same directory of the notebooks. All of the notebooks in this directory will then use `rise.css` by default.

An example of a basic `rise.css` file can be found in the <a href="https://github.com/uc-python/intro-python-datasci/blob/master/notebooks/rise.css" target="_blank" class="class2">GitHub repository</a> for one of my existing trainings.

This little amount of CSS can make a large difference in the visual aesthetic of your slides:

<br>
<center>
<img src="/assets/jupyter-css-slide.png" height="400">
</center>
<br>

#### Adding notebook-specific CSS

If you are wanting to add specific CSS files for a specific notebook (let's call it `notebook-name.ipynb`), you can simply create a CSS file in the directory of the notebook titled `notebook-name.css`.

<br>

### Tip 6: Host live slides online with Binder

One of the drawbacks of RISE is that it requires a running Jupyter Notebooks instance. While this isn't too problematic for live presentations hosted on a local computer, it can be make the result hard to share. The next two tips provide ways to do that.

Live RISE slides can be shared using a Jupyter-created product called <a href="https://mybinder.org/" target="_blank" class="class2">Binder</a>. Binder allows you to host a live Jupyter Notebooks session for free within a browser -- therefore, no Python and/or Jupyter setup is required for users.

Binder works by accepting a link to your project's GitHub repository.

<br>

### Tip 7: Export slides directly to HTML for non-live viewing

Slideshows developed in Jupyter Notebooks can also be exported to a single HTML file. While this method does not include many of the live features RISE offerse through a running Pythons session, it does enable the sharing of slideshows through a single HTML file. 

This can be done using the  `jupyter nbconvert` command on the command line. This is the command I use:

```bash
jupyter nbconvert notebook-name.ipynb --to-slides --reveal-prefix reveal.js
```

If you do not have reveal.js installed locally, you can use an online version:

```bash
jupyter nbconvert notebook-name.ipynb --to-slides --reveal-prefix "http://cdnjs.cloudflare.com/ajax/libs/reveal.js/3.3.0"
```

A single HTML file will be created in the same directory as the notebook, and opening this HTML will will open the slideshow.

<br>

### Tip 8: Open slides automatically

Sometimes it's useful to share live slideshows rather than static ones. Binder makes this possible by providing an easy notebook-sharing mechanism, but viewers still need to active presentation mode within the notebook. RISE's autolaunch feature solves this problem.

RISE's autolaunch feature can be enabled by adding the below data to the Jupyter Notebook's metadata. You can edit the Jupyter Notebook metadata by clicking on "Edit" in the Jupyter Notebooks toolbar, and then clicking on "Edit Notebook Metadata".

```
{
  ...,
  "rise": {
      "autolaunch": true
  }
}
```

With this feature enabled, the slideshow will automatically launch when the notebook itself is launched

<br>
<center>
<img src="/assets/jupyter-autolaunch-slides.gif" height="400">
</center>
<br>
<br>