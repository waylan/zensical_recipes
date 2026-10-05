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