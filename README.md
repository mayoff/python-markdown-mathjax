
# About `python-markdown-mathjax`

This is a trivial [`python-markdown`](https://github.com/waylan/Python-Markdown) extension for embedding LaTeX math markup in Markdown so that [MathJax](http://www.mathjax.org/) can process it.

I assume that you'll be using `$...$` and `$$...$$` to surround your LaTeX math markup.  MathJax already recognizes `$$...$$` by default, but you need to tell it to recognize `$...$`:

    <script type="text/javascript">
        MathJax.Hub.Config({
            "tex2jax": { inlineMath: [ [ '$', '$' ] ] }
        });
    </script>

The `python-markdown` processor doesn't normally recognize `$...$` or `$$...$$`, so it tends to wreak havoc on your math markup by treating `*` and `_` as delimiters for italics and boldface and removing backslashes.  This extension tells `python-markdown` not to look for markup inside `$...$` and `$$...$$`.

## Installing the extension

You can install the extension in one of two ways:

-   You can put it right into your `python-markdown` installation.  Run this command:

        python -c 'import markdown; print markdown.__path__[0]'

    That prints the path of the `markdown` module.  It should have a subdirectory named `extensions`.  Rename `mdx_mathjax.py` to `mathjax.py` and copy it to that `extensions` subdirectory.

    If `python-markdown` was installed as a zipped egg file, you won't find that `markdown/extensions` directory because it's inside the egg file.  You'll have to delete the egg file (you can find the name in the output of the command above) and reinstall it unzipped: `easy_install -Z Markdown`.  Then you should have a `markdown/extensions` directory in which to put `mathjax.py`.

-    You can put it in your `PYTHONPATH`.  If you do this, don't rename the file.  It needs to be named `mdx_mathjax.py`.

## Using the extension from the command-line

After you've installed the file, you need to tell `python-markdown` to use it.  If you're using the `markdown` command-line program, use `-x mathjax` on the command line, like this:

    markdown -x mathjax p-equals-np-proof.md > p-equals-np-proof.html

By default, `markdown` won't report an error if it can't find the extension. It will just silently continue on, and your math markup will probably get munged.  If you give `markdown` the `-v` flag, it will report the problem, but it will still continue on and won't exit with an error code.

## Using the extension from a Python program

If you're using the `markdown` module from a Python program, you will need to import `mdx_mathjax` if you put the file `mdx_mathjax.py` in your `PYTHONPATH`.  If you renamed it `mathjax.py` and put it in the `markdown/extensions` directory, you don't import it. You can use a `try`/`except` block to handle both cases:

    import markdown
    try: import mdx_mathjax
    except: pass
    mdProcessor = markdown.Markdown(extensions=['mathjax'])
    myHtmlFragment = mdProcessor.convert(r"Euler's identity, $e^{i\pi} = -1$, is widely considered the most beautiful theorem in mathematics.")

If you're using the `markdown` module from a Python program, and it can't load the `mathjax` extension, it will raise a `markdown.MarkdownException` error.

# No Copyright

The author of this repository, Rob Mayoff, dedicates the contents of this repository to the public domain, in accordance with the [CC0 1.0 Universal Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/).
reproduced in the file named COPYRIGHT in this repository.

