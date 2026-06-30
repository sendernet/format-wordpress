# Separator between tool sections

Each tool section in a roundup gets its own H3 heading, image, description,
features, pros & cons, pricing, and "Best for" block. Insert a separator
between consecutive tool sections so they read as distinct units.

The separator block to insert:

```html
<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->
```

## Where to place it

Between every pair of adjacent tool sections — i.e. immediately **before**
each tool's H3 heading **except** the first one. Place the separator on its
own line with a blank line above and below so it sits as a distinct block.

A "tool heading" is the same H3 identified by the
[tool heading anchors rule](tool-heading-anchors.md): the heading that
introduces one of the tools being reviewed. Headings inside the methodology,
migration, comparison, or FAQ sections are not tool headings — don't put a
separator before those.

## Constraints

- **One separator between two adjacent tool sections, not more.** If a
  separator already sits between two tool sections, don't add a second.
- **No separator before the first tool section** — the section before it is
  usually the methodology, which has its own banner.
- **No separator after the last tool section** — the next H2 (Migration
  Guide, Comparison, FAQ, etc.) starts the next part of the post and the
  heading itself is the divider.

## Example

Before (end of one tool section + start of the next):

```html
<!-- wp:paragraph -->
<p><strong>Best for:</strong> Solo developers and small teams running local SMTP capture during development …</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"mailhog","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="mailhog">MailHog — Free Local Email Testing Alternative</h3>
<!-- /wp:heading -->
```

After:

```html
<!-- wp:paragraph -->
<p><strong>Best for:</strong> Solo developers and small teams running local SMTP capture during development …</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3,"anchor":"mailhog","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="mailhog">MailHog — Free Local Email Testing Alternative</h3>
<!-- /wp:heading -->
```
