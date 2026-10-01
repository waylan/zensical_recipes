---
icon: lucide/file-stack
---

# Cards

Zensical provides good support for [grid cards]. However, there are a number
of ways they can be extended with some additional CSS.

[grid cards]: https://zensical.org/docs/authoring/grids/

## Setup

First, ensure the[`extra_css`][extra_css] configuration option is set and
points to a css file. For example:

[extra_css]: https://zensical.org/docs/customization/#additional-css

=== "`zensical.toml`"

    ``` toml
    [project]
    extra_css = ["assets/stylesheets/extra.css"]
    ```

=== "`mkdocs.yml`"

    ``` yaml
    extra_css:
        - assets/stylesheets/extra.css
    ```

I prefer to use the [`pymdownx.blocks.html`][pymdownx.blocks.html] plugin
rather than raw HTML. Therefore, all examples below use the blocks syntax. To
enable the extension, add `pymdownx.blocks.html` to the list of
`markdown.extensions`.

[pymdownx.blocks.html]: https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/html/

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


## Card Headings

Zensical's documentation includes an example of cards which appear to have headings. However, the text is just bold, which is not ADA complient. With a litte CSS, actual headings will appear correctly in a card. Therefore, add the following CSS to `extra.css`.

``` css
.cards h2, .cards h3 {
    margin: 0 0 .4em;
}

.cards h2 a.headerlink, .cards h3 a.headerlink {
    display: none;
}
```

Now you can include actual level 2 and 3 headings in your cards

``` markdown
/// html | div.grid.cards

- ### :lucide-balloon: Card One

    Content

- ### :lucide-ferris-wheel: Card Two

    Content
///
```

//// html | div.result
/// html | div.grid.cards

- ### :lucide-balloon: Card One

    Content

- ### :lucide-ferris-wheel: Card Two

    Content
///
////

## Centered Cards

To have the content of a  set of cards centered, add the following to `extra.css`.

``` css
/* Center Card */
.cards.center {
    text-align: center;
    text-wrap: balance;
}
```

Then, add the `.center` class to the wrapping `div` to center content of all cards in the grid.

``` markdown
/// html | div.grid.cards.center

- ### :lucide-balloon: Card One

    Content

- ### :lucide-ferris-wheel: Card Two

    Content
///
```

//// html | div.result
/// html | div.grid.cards.center

- ### :lucide-balloon: Card One

    Content

- ### :lucide-ferris-wheel: Card Two

    Content
///
////

## Darken Border

In certain situations I have prefered a slightly darker border. To get a
darker border, add the following to `extra.css`.

``` css
/* Darken Card Border */
.md-typeset .grid.cards.darken>ol>li, .md-typeset .grid.cards.darken>ul>li, .md-typeset .grid>.card.darken {
    border-color: var(--md-default-fg-color--lighter);
}
```

Then, add the `.darken` class to the wrapping `div` to darken the borders of all cards in the grid.

``` markdown
/// html | div.grid.cards.darken

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
```

//// html | div.result
/// html | div.grid.cards.darken

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
////

## Block Cards

If you would like to wrap some content in the card style border but do not want that content to be in a grid, then you can use the followign CSS to define an alternate __Block Card__.

``` css
/* Block Cards */
.md-typeset .block {
    margin: 1em 0;
}

.md-typeset .block.cards h2, .md-typeset .block.cards h3 {
    text-wrap: pretty;
}

.md-typeset .block.cards>ol, .md-typeset .block.cards>ul {
    display: contents;
}

.md-typeset .block>*, .md-typeset .grid>.admonition, .md-typeset .block>.highlight>*,
.md-typeset .block>.highlighttable, .md-typeset .block>.md-typeset details, 
.md-typeset .block>details, .md-typeset .block>pre {
    margin-bottom: 0;
    margin-top: 0;
}

.md-typeset .block.cards>ol>li, .md-typeset .block.cards>ul>li, .md-typeset .block>.card {
    border: .05rem solid var(--md-default-fg-color--lightest);
    border-radius: .4rem;
    display: block;
    margin: .4rem 0;
    padding: .8rem;
    transition: background-color .25s,border .25s,box-shadow .25s;
}

.md-typeset .block.cards>ol>li:hover, .md-typeset .block.cards>ol>li:focus-within,
.md-typeset .block.cards>ul>li:hover, .md-typeset .block.cards>ul>li:focus-within,
.md-typeset .block>.card:focus-within, .md-typeset .block>.card:hover {
    border-color: #0000;
    box-shadow: var(--md-shadow-z2);
}

.md-typeset .block.cards.darken>ol>li, .md-typeset .block.cards.darken>ul>li, .md-typeset .block.darken>.card {
    border-color: var(--md-default-fg-color--lighter);
}

.md-typeset .block.cards>ol>li>:first-child, .md-typeset .block.cards>ul>li>:first-child, 
.md-typeset .block>.card>:first-child {
    margin-top: 0;
}
```

Then define a `div.block.card` using the same basic syntac as grid cards.

``` markdown
/// html | div.block.cards

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
```

//// html | div.result
/// html | div.block.cards

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
////

Note that block cards also support darkened borders.

``` markdown
/// html | div.block.cards.darken

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
```

//// html | div.result
/// html | div.block.cards.darken

- :lucide-balloon: Card One

- :lucide-ferris-wheel: Card Two

///
////

## Welcome Card

A welcome card is a combination of grid cards and a page title combined with a
background image. It might be used on the home page to highlight some
features of a product or service.


To start, add the following CSS to `extra.css`.

``` css
/* Home Page Welcome Card */
div#welcome {
    padding: 1.5em;
    border-radius: .4rem;
    background: no-repeat center url("../images/orange_mountains.jpg");
    background-size: cover;
}

div#welcome h1 {
    text-align: center; 
    background-color: var(--md-default-bg-color--light); 
    padding: .75em;
    border-radius: .4rem;
    margin-bottom: .75em;
    color: var(--md-typeset-a-color);
    text-wrap: balance;
}

div#welcome h1 a.headerlink {
    display: none;
}

div#feature {
    margin: 0;
    grid-gap: 1.5em;
}

div#feature li {
    background-color: var(--md-default-bg-color--light);
    border: .05rem solid var(--md-default-fg-color--lightest);
}

div#feature li p {
    font-weight: 510;
}

div#feature li h2 {
    color: var(--md-typeset-a-color);
}

@media print {
    div#welcome {
        background: none;
    }

    div#feature {
        display: none;
    }
}
```

You will need to update the URL for the background image in the CSS and ensure
it points to an image you have access to.

Then build up the necessary HTML.

``` markdown
/// html | div#welcome

# Welcome Card

//// html | div#feature.grid.cards.center

*   ## :lucide-balloon:{ .lg .middle } Cool Feature

    Highlight a cool feature of your product here.

    [See More](#){ .md-button .md-button--primary }

*   ## :lucide-thumbs-up:{ .lg .middle } Great Benefit

    Highlight a great benefit users receive by using your product.

    [Learn More](#){ .md-button .md-button--primary }

*   ## :material-frequently-asked-questions:{ .lg .middle } FAQs

    Get answers to frequently asked questions.

    [View FAQs](#){ .md-button .md-button--primary }
////
///
```

///// html | div.result
/// html | div#welcome

# Welcome Card

//// html | div#feature.grid.cards.center

*   ## :lucide-balloon:{ .lg .middle } Cool Feature

    Highlight a cool feature of your product here.

    [See More](#feature){ .md-button .md-button--primary }

*   ## :lucide-thumbs-up:{ .lg .middle } Great Benefit

    Highlight a great benefit users receive by using your product.

    [Learn More](#feature){ .md-button .md-button--primary }

*   ## :material-frequently-asked-questions:{ .lg .middle } FAQs

    Get answers to frequently asked questions.

    [View FAQs](#feature){ .md-button .md-button--primary }
////
///
/////

Resize your browser window and the welcome card will adjust accordingly. I
find it best to use on a full width page. Therefore, I suggest hiding the
navigation and table of contents on your home page (or whatever page you use
it on).

To accomplish that, in the frontmatter for the page, add the following.

``` markdown
---
title: Home
hide:
    - navigation
    - toc
---
```
