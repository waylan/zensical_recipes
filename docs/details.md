---
icon: lucide/chevrons-up-down
---


# Details (Accordions)

Zensical supports Accordions (using `details` blocks) out of the box, although
[documentation] is minimal.

[documentation]: https://zensical.org/docs/compatibility/markdown/python-markdown-extensions/#details

## Enable The Extension

First, enable the extension by adding `pymdownx.blocks.details` to the list of `markdown.extensions`.

/// tab | `zensical.toml`

``` toml
[project.markdown_extensions]
pymdownx.blocks.details = {}
```
///

/// tab | `mkdocs.yml`

``` yaml
markdown_extensions:
- pymdownx.blocks.details
```
///

## Markdown Syntax

Use the details block syntax as documented in the [`pymdownx.blocks.details`]
[details] extension. Details blocks can be assigned the [same 12 types][types]
as admonitions. 

``` markdown
/// details | Custom Types
    type: tip
    open: True

For information on defining custom types, see [Custom Admonitions].
///

[Custom Admonitions]: custom_admonitions.md
```
//// html | div.result
/// details | Custom Types
    type: tip
    open: True

For information on defining custom types, see [Custom Admonitions].
///
////

[details]: https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/details/
[types]: https://zensical.org/docs/authoring/admonitions/#supported-types 
[Custom Admonitions]: custom_admonitions.md

### Heading in Title

As of version 12.0 of the `pymdownx.blocks.details` extension, you can use a
Markdown heading in the title of a details block. However, Zensical needs
some extra CSS defined to properly display the headings.

First, ensure the[`extra_css`][extra_css] configuration option is set and
points to a css file. For example:

[extra_css]: https://zensical.org/docs/customization/#additional-css

/// tab | `zensical.toml`

``` toml
[project]
extra_css = ["assets/stylesheets/extra.css"]
```
///

/// tab | `mkdocs.yml`

``` yaml
extra_css:
    - assets/stylesheets/extra.css
```
///

Then add the following to the css file.

``` css
/* Support headings in details blocks */
.md-typeset details>summary>:is(h1,h2,h3,h4,h5,h6) {
    margin: 0;
    font-weight: inherit;
    font-size: inherit;
}
```

Now, you can use a heading within the title of a details block.

``` markdown
/// details | ### Why would I want to use a heading in the title of a details block?
    type: question


There are as least three benefits. 

1. The titles to your details blocks are now easily linkable. While you could
   always manually set an id on a details block, the visitors to your site
   would not easily discover it. However, as a title, Zensical provides a
   permalink, which is easily discoverable.
2. The titles get listed in your table of contents and are more easily
   discoverable. This may or may not be desirable, depending on the context.
3. The site search properly displays the title in search results for any
   content which matches a search term. This is especially useful when details
   blocks are used for a series of frequently asked questions (FAQs).
///
```
//// html | div.result
/// details | #### Why would I want to use a heading in the title of a details block?
    type: question

There are as least three benefits. 

1. The titles to your details blocks are now easily linkable. While you could
   always manually set an id on a details block, the visitors to your site
   would not easily discover it. However, as a title, Zensical provides a
   permalink, which is easily discoverable.
2. The titles get listed in your table of contents and are more easily
   discoverable. This may or may not be desirable, depending on the context.
3. The site search properly displays the title in search results for any
   content which matches a search term. This is especially useful when details
   blocks are used for a series of frequently asked questions (FAQs).
///
////

### Connecting Multiple Blocks

Sometimes it may be desirable to only show the contents of one of a series of
details blocks. For example, when a user clicks on one block in the series,
not only does that block open, but any other open block in the series closes.

This can be accomplished by assigning all related blocks the same `name`
attribute. Any blocks with no `name` attribute or a different `name`
attribute would be unaffected.

``` markdown
/// details | Example 1
    type: example
    attrs:
        name: group-1

Example 1 content.
///

/// details | Example 2
    type: example
    attrs:
        name: group-1

Example 2 content.
///
```

//// html | div.result
/// details | Example 1
    type: example
    attrs:
        name: group-1

Example 1 content.
///

/// details | Example 2
    type: example
    attrs:
        name: group-1

Example 2 content.
///
////

### Supporting the printing of details blocks

By default, most browsers do not print the content of any closed details
blocks. However, by adding the following CSS to `extra.css`, you can ensure
that the contents or all blocks are included when a page is printed.

``` css
/* Always print the content of details blocks */
@media print {
  ::details-content {
    content-visibility: visible;
    height: auto !important;
  }
}
```
