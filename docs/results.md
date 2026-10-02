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

=== "Raw HTML"

    To use raw HTML, ensure that the [`md_in_html`][md_in_html] extension is
    enabled.

    === "`zensical.toml`"

        ``` toml
        [project.markdown_extensions]
        md_in_html = {}
        ```

    === "`mkdocs.yml`"

        ``` yaml
        markdown_extensions:
        - md_in_html
        ```

=== "HTML Blocks"

    To enable the HTML Blocks extension, add `pymdownx.blocks.html` to the
    list of `markdown.extensions`.

    === "`zensical.toml`"

        ``` toml
        [project.markdown_extensions]
        pymdownx.blocks.html = {}
        ```

    === "`mkdocs.yml`"

        ``` yaml
        markdown_extensions:
        - pymdownx.blocks.html
        ```

[md_in_html]: https://python-markdown.github.io/extensions/md_in_html/

## The Basic Syntax

A result block will be displayed when a block is assigned the class `highlight`
followed by its next sibling being assigned the class `result`.

As an example, consider the following Markdown.

=== "Raw HTML"

    ``` text
    <div class="highlight" style="background: var(--md-code-bg-color); padding: .5em 1em;">
    <p>Highlight content</p>
    </div>

    <div class="result">
    <p>Result Content</p>
    </div>
    ```

    //// html | div.result
        attrs: {style: "padding-top: 1em"}
    /// html | div.highlight
        attrs: {style: "background: var(--md-code-bg-color); padding: .5em 1em;"}
    Highlight content
    ///

    /// html | div.result
    Result Content
    ///
    ////

=== "HTML Blocks"

    ``` text
    /// html | div.highlight
        attrs: 
            style: "background: var(--md-code-bg-color); padding: .5em 1em;"
    Highlight content
    ///

    /// html | div.result
    Result Content
    ///
    ```

    //// html | div.result
        attrs: {style: "padding-top: 1em"}
    /// html | div.highlight
        attrs: {style: "background: var(--md-code-bg-color); padding: .5em 1em;"}
    Highlight content
    ///

    /// html | div.result
    Result Content
    ///
    ////

As you can see, we needed to define some custom styles to get it to look
mostly right. There is more work to do to properly define a radius for the
top corners and there is still room for margin and padding adjustments.

While not particularly useful this way, it does demonstrate the basic
requirements. So long as the `.highlight` block has a background which
matches the background of code blocks, the `.result` block's border is merged
with the bottom and nests its content.

The feature is intended to be used with Zensical's default [code highlighting
configuration]. When configured as recommended in the linked documentation,
code blocks will meet all of the requirements for the first block. All you need
to do is follow up a code block with a result block.

[code highlighting configuration]: https://zensical.org/docs/authoring/code-blocks/#configuration

=== "Raw HTML"

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

    /// html | div.result
    ``` html
    <p>Some <em>Markdown</em> text.</p>
    ```
    ///
    ////

=== "HTML Blocks"

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

Alternatively, you could show the result as rendered HTML, rather than as source code.

=== "Raw HTML"

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

    /// html | div.result
    Some *Markdown* text.
    ///
    ////

=== "HTML Blocks"

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

You could even nest multiple levels deep.

=== "Raw HTML"

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
    /// html | div.result
    ``` html
    <p>Some <em>Markdown</em> text.</p>
    ```
    //// html | div.result
    Some *Markdown* text.
    ////
    ///
    /////

=== "HTML Blocks"

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
