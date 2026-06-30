# Headings must not be bold

Headings (`<h1>` through `<h6>`) already render bold by default — any
`<strong>` or `<b>` inside them is redundant and adds visual noise. For every
heading element in the markup, strip wrapping `<strong>`/`</strong>` and
`<b>`/`</b>` tags so only the plain text (and any other inline formatting like
`<em>`, `<a>`) remains.

- Apply to all heading levels, whether the heading is a `<!-- wp:heading -->`
  block (H2 by default) or has `{"level":N}` for H3/H4/etc.
- If the entire heading text is wrapped in `<strong>`, remove just the wrapper
  — don't touch the inner content.
- If only part of the heading is wrapped in `<strong>`, remove that wrapper
  too — partial bolding inside a heading is also redundant.
- Don't touch `<strong>` tags that appear in non-heading blocks (paragraphs,
  lists, table cells, etc.).

Example — before:

```html
<!-- wp:heading -->
<h2 class="wp-block-heading"><strong>The Alternatives</strong></h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Mailpit <strong>vs</strong> MailHog</h3>
<!-- /wp:heading -->
```

After:

```html
<!-- wp:heading -->
<h2 class="wp-block-heading">The Alternatives</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Mailpit vs MailHog</h3>
<!-- /wp:heading -->
```
