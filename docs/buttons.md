---
icon: lucide/rectangle-horizontal
description: Add additional button styles.
---

# Buttons

Zensical provides support for two styles of [buttons]. A generic gray button
and a button which uses the [primary color]. However, the theme also provides
variations on the primary color as well as an [accent color]. Additional
button styles can be created using these alternate colors.

[buttons]: https://zensical.org/docs/authoring/buttons/
[primary color]: https://zensical.org/docs/setup/colors/#primary-color
[accent color]: https://zensical.org/docs/setup/colors/#accent-color

## Setup

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

## The Buttons

To define additional button styles, add the following CSS to `extra.css`.

``` css
/* Button Styles */
.md-typeset .md-button.md-button--primary-light {
    background: var(--md-primary-fg-color--light);
    color: var(--md-primary-bg-color--light);
}

.md-typeset .md-button.md-button--primary-dark {
    background: var(--md-primary-fg-color--dark);
    color: var(--md-primary-bg-color);
}

.md-typeset .md-button.md-button--accent{
    background: var(--md-accent-fg-color);
    color: var(--md-accent-bg-color);
}

.md-typeset .md-button.md-button--transparent {
    background: var(--md-accent-fg-color--transparent);
}
```

## The Options

You now have all of the following options available to use for buttons.

``` markdown
[Default](#){ .md-button }

[Primary Light](#){ .md-button .md-button--primary-light}

[Primary](#){ .md-button .md-button--primary}

[Primary Dark](#){ .md-button .md-button--primary-dark}

[Accent](#){ .md-button .md-button--accent}

[Accent Transparent](#){ .md-button .md-button--transparent}
```

/// html | div.result
[Default](#){ .md-button }

[Primary Light](#){ .md-button .md-button--primary-light}

[Primary](#){ .md-button .md-button--primary}

[Primary Dark](#){ .md-button .md-button--primary-dark}

[Accent](#){ .md-button .md-button--accent}

[Accent Transparent](#){ .md-button .md-button--transparent}
///

!!! tip

    The `md-button--transparent` style is actually semi-transparent and the
    apparent color will shift dependent upon the background color.

    [Accent Transparent](#){ .md-button .md-button--transparent}
