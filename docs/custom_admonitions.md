---
icon: lucide/square-exclamation-point
---

# Custom Admonitions

Zensical already has good support for [Admonitions] and defines styles for 12
[supported types]. However, if you use anything other than those 12 supported
types, then they all look like the default `note` type, with the same color
and icon.

[Admonitions]: https://zensical.org/docs/authoring/admonitions/
[supported types]: https://zensical.org/docs/authoring/admonitions/#supported-types

``` markdown
!!! custom

    Some content
```

/// html | div.result
!!! custom

    Some content
///

## Define CSS

To define a custom type, with its own color and icon, ensure the
[`extra_css`][extra_css] configuration option is set and points to a css file.
For example:

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

Then add the following to the css file to define a `seealso` custom admonition:

``` css
/* Add support for `seealso` Admonition */
.md-typeset .admonition.seealso, .md-typeset details.seealso {
    background-color: var(--md-accent-fg-color--transparent);
}

.md-typeset .seealso>.admonition-title:before, .md-typeset .seealso>summary:before {
    background-color: var(--md-accent-fg-color);
}

.md-typeset .seealso>.admonition-title:after, .md-typeset .seealso>summary:after {
    color: var(--md-accent-fg-color);
}

.md-typeset .seealso>.admonition-title:before, .md-typeset .seealso>summary:before {
    --md-admonition-icon--seealso: url('data:image/svg+xml;charset=utf-8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-square-arrow-out-up-right-icon lucide-square-arrow-out-up-right"><path d="M21 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h6"/><path d="m21 3-9 9"/><path d="M15 3h6v6"/></svg>');
    -webkit-mask-image: var(--md-admonition-icon--seealso);
    mask-image: var(--md-admonition-icon--seealso);
}
```

You may replace every occurance of `.seealso` with your adminition type.

Note that all of the colors point to variables defined by Zensical's theme
(in this case using the theme's [accent color]). While this is not nessecary,
it does ensure that the colors always match the theme and appropriately
switch with light and dark mode. If you want to use other colors, you can,
but then you will need to ensure things work with the light and dark modes
(if enabled).

[accent color]: https://zensical.org/docs/setup/colors/#accent-color

Finally, the `--md-admonition-icon--seealso` variable defines an inline svg
image for an icon. The `<svg>` tag was copied from
<https://lucide.dev/icons/square-arrow-out-up-right>. Any valid svg element
will work.

!!! tip

    You can use the same technique to redefine the styles for the 12 supported
    types. Just replace `seealso` above with the type name. Too redefine the
    default (no type provided), then define the rules without the class
    (remove `.seealso`).

/// details | Why are CSS rules for `summary` elements included?
    type: question

You may have noticed that the CSS above also defines matching rules for
`summary` elements, which are not used by admonitions. As it turns out,
Zensical defines the same 12 types for [details] as are defined for
admonitions. To maintain that consistency, I recommend defining both for any
custom types as well. Of course, to use them, you will need to enable the
appropriate extension.
///

[details]: https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/details/

## The Result

That's it. You can now define a `seealso` admonition and get your custom
styling.

``` markdown
!!! seealso "See Also"

    The content of the Admonition.
```

/// html | div.result
!!! seealso "See Also"

    The content of the Admonition.
///

## Defining a Default Title

By default, a lowercase type will be uppercased as the title of an admonition.
However, in the above example, that would result in `Seealso` which is not a
valid word. Therefore, we are also manually defining the title. If you would
like the title to automatically be defined for a type, you can use the
[`pymdownx.blocks.admonition`][pymdownx.blocks.admonition] extension instead.

[pymdownx.blocks.admonition]: https://facelessuser.github.io/pymdown-extensions/extensions/blocks/plugins/admonition/

In your configuration file replace the `admonition` extension with:

=== "`zensical.toml`"

    ``` toml
    [project.markdown_extensions]
    pymdownx.blocks.admonition = {
        types = [
            {'name': 'seealso', 'class': 'seealso', 'title': 'See Also'}
        ]
    }
    ```

=== "`mkdocs.yml`"

    ``` yaml
    markdown_extensions:
    - pymdownx.blocks.admonition:
        types:
            - name: seealso
              class: seealso
              title: See Also
    ```

You will need to use the different syntax for defining admonitions, but you
will not need to define the title each time.

``` markdown
/// seealso
The content of the Admonition.
///
```

/// html | div.result
!!! seealso "See Also"

    The content of the Admonition.
///

Some may prefer the shortcut, while others may prefer to stick to the original
syntax.
