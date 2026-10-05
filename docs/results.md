---
icon: lucide/square-code
description: Show output of a code block in nested results block.
---

# Code Results

Zensical provides support out-of-the-box for showing the results form a code
block in a nested block immediately after the code block. In fact, you will
see it used a few places within Zensical's documentation. However, the
feature is not documented anywhere in the Zensical's documentation. The
feature has been documented here with extensions.

## Basic Setup

There are at least two ways to author result blocks. You can use raw HTML or
the syntax supported by the [`pymdownx.blocks.html`][pymdownx.blocks.html]
extension.

[pymdownx.blocks.html]: https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/html/

/// tab | Raw HTML

To use raw HTML, ensure that the [`md_in_html`][md_in_html] extension is
enabled.

//// tab | `zensical.toml`

``` toml
[project.markdown_extensions]
md_in_html = {}
```

////

//// tab | `mkdocs.yml`

``` yaml
markdown_extensions:
- md_in_html
```

////
///

/// tab | HTML Blocks

To enable the HTML Blocks extension, add `pymdownx.blocks.html` to the list of
`markdown.extensions`.

//// tab | `zensical.toml`

``` toml
[project.markdown_extensions]
pymdownx.blocks.html = {}
```

////

//// tab | `mkdocs.yml`

``` yaml
markdown_extensions:
- pymdownx.blocks.html
```

////
///

[md_in_html]: https://python-markdown.github.io/extensions/md_in_html/

## The Basic Syntax

A result block will be displayed when a block is assigned the class
`highlight` followed by its next sibling being assigned the class `result`.
The feature is intended to be used with Zensical's default [code highlighting
configuration]. When configured as recommended in the linked documentation,
code blocks will meet all of the requirements for the first block. All you
need to do is follow up a code block with a result block.

[code highlighting configuration]: https://zensical.org/docs/authoring/code-blocks/#configuration

////// tab | Raw HTML

```` text
``` markdown
Some *Markdown* text.
```

<div class="result" markdown>
``` html
<p>Some <em>Markdown</em> text.</p>
```
</div>
````

//// html | div.result
``` markdown
Some *Markdown* text.
```

<div class="result" markdown>
``` html
<p>Some <em>Markdown</em> text.</p>
```
</div>
////
//////

////// tab | HTML Blocks

```` text
``` markdown
Some *Markdown* text.
```

/// html | div.result
``` html
<p>Some <em>Markdown</em> text.</p>
```
///
````

//// html | div.result
``` markdown
Some *Markdown* text.
```

/// html | div.result
``` html
<p>Some <em>Markdown</em> text.</p>
```
///
////
//////

As you can see, a code block which shows the rendered output of the Markdown
source text is wrapped in an HTML block which consists of a `<div>` element
that has the class `result` assigned to it. Zensical neatly nests the
`result` block within the border of the code lock before it.

The result block does not need to contain a code block. For example, you could
show the result as rendered HTML, rather than as source code.

////// tab | Raw HTML

```` text
``` markdown
Some *Markdown* text.
```

<div class="result" markdown>
Some *Markdown* text.
</div>
````

//// html | div.result
``` markdown
Some *Markdown* text.
```

<div class="result" markdown>
Some *Markdown* text.
</div>
////
//////
////// tab | HTML Blocks

```` text
``` markdown
Some *Markdown* text.
```

/// html | div.result
Some *Markdown* text.
///
````

//// html | div.result
``` markdown
Some *Markdown* text.
```

/// html | div.result
Some *Markdown* text.
///
////
//////

You could even nest multiple levels deep.

////// tab | Raw HTML

```` text
``` markdown
Some *Markdown* text.
```
<div class="result" markdown>
``` html
<p>Some <em>Markdown</em> text.</p>
```
<div class="result" markdown>
Some *Markdown* text.
</div>
</div>
````

///// html | div.result
``` markdown
Some *Markdown* text.
```
<div class="result" markdown>
``` html
<p>Some <em>Markdown</em> text.</p>
```
<div class="result" markdown>
Some *Markdown* text.
</div>
</div>
/////
//////
////// tab | HTML Blocks

```` text
``` markdown
Some *Markdown* text.
```
/// html | div.result
``` html
<p>Some <em>Markdown</em> text.</p>
```
//// html | div.result
Some *Markdown* text.
////
///
````

///// html | div.result
``` markdown
Some *Markdown* text.
```
/// html | div.result
``` html
<p>Some <em>Markdown</em> text.</p>
```
//// html | div.result
Some *Markdown* text.
////
///
/////
//////

Nesting only works if every level contains a code block with the exception of
the final level.

The examples above all show Markdown content and HTML output. Of course, your
code and result blocks could contain anything that can be represented in
Markdown.

## Custom Rendering

All of the examples above have been hand-crafted. However, with some
customizations, the code in the code block can be run and the result block
automatically generated.

The [Markdown Exec] library provides various prepackaged solutions for you and
is probably sufficient for most use-cases. See the documentation for details.

If that does not meet your needs, the [Superfences] extension to
Python-Markdown (which is part of Zensical's default configuration), includes
support for defining [custom fences]. In short, in your configuration you
would define a custom formatter which points to a Python function. That
function would accept the content of the code block as text and return a
rendered result to replace the code block.

While defining custom formatters is outside the scope of this document, there
are various examples listed below that you can look at for inspiration.

- The documentation for [PyMdown Extensions] defines 3 custom formatters (see
  [config][pymdown config] and [code][pymdown code]).
- The documentation for [ColorAide] defines 4 custom formatters (see
  [config][coloraide config] and [code][coloraide code]).
- The documentation for [Python-Markdown] defines 2 custom formatters (see
  [config][pm config], [code][pm code], and [docs][pm docs]).

[Superfences]: https://facelessuser.github.io/pymdown-extensions/extensions/superfences/
[custom fences]: https://facelessuser.github.io/pymdown-extensions/extensions/superfences/#custom-fences
[Markdown Exec]: https://pawamoy.github.io/markdown-exec/
[PyMdown Extensions]: https://facelessuser.github.io/pymdown-extensions/
[pymdown code]: https://github.com/facelessuser/pymdown-extensions/blob/main/tools/pymdownx_md_render.py
[pymdown config]: https://github.com/facelessuser/pymdown-extensions/blob/92186b5615e24de0e8a569c30326d32e2bc3a794/zensical.yml#L136-L145
[ColorAide]: https://facelessuser.github.io/coloraide/
[coloraide code]: https://github.com/facelessuser/coloraide/blob/main/docs/src/py/notebook.py
[coloraide config]: https://github.com/facelessuser/coloraide/blob/977f95aba74d21d21e5146c316fceccb533c5d39/zensical.yml#L180-L214
[Python-Markdown]: https://python-markdown.github.io/
[pm config]: https://github.com/Python-Markdown/markdown/blob/ee57673228fdab926f596e45c01e0dee0b754221/mkdocs.yml#L160-L166
[pm code]: https://github.com/Python-Markdown/markdown/blob/master/tools/superfences_formaters.py
[pm docs]: https://python-markdown.github.io/contributing/#code-blocks
