# Wrap the TL;DR / snapshot section in a wp:group

Roundup posts include a short summary section near the top that lists every
tool covered in the article — sometimes called the TL;DR, snapshot, overview,
or "at a glance" section. Wrap that section in a `<!-- wp:group -->` block so
it renders as a visually distinct unit.

## What counts as the TL;DR section

A heading + list pair where:

- The heading is an `<!-- wp:heading -->` H2 (no `level` override, or
  `{"level":2}`).
- The heading text signals a summary — e.g. contains `TL;DR`, `TLDR`,
  `Snapshot`, `At a Glance`, `Quick Look`, `Summary`, `Overview`,
  `Worth Your Attention`, or a similar tag-line phrase. If unsure whether a
  heading qualifies, prefer the **first** H2 in the post since that's where
  the snapshot lives in roundup posts.
- It's immediately followed by a `<!-- wp:list -->` block whose items each
  describe one of the tools covered later in the article (typically with a
  bold lead-in and an anchor link).

Wrap exactly one such section per post. If there is no matching section, skip
this rule.

## How to wrap

Insert the `wp:group` opening directly before the heading and the closing
directly after the list, keeping the inner blocks unchanged.

The exact shape — `</div>` glued to `<!-- /wp:list -->`, `<div ...>` glued to
the first inner block's opening comment, to match Gutenberg's serialized
output:

```
<!-- wp:group {"layout":{"type":"constrained"}} -->
<div class="wp-block-group"><!-- wp:heading -->
<h2 class="wp-block-heading">…heading text…</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list">…list items…</ul>
<!-- /wp:list --></div>
<!-- /wp:group -->
```

If the heading + list pair is already inside a `<!-- wp:group -->` with the
same `{"layout":{"type":"constrained"}}` attribute, leave it alone — don't
nest a second group.

## Example

Before:

```html
<!-- wp:heading -->
<h2 class="wp-block-heading">Mailtrap Alternatives Worth Your Attention: A Snapshot</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Local development default — <a href="#mailpit">Mailpit</a>.</strong> Docker container running in under two minutes…</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Legacy local testing, with caveats — <a href="#mailhog">MailHog</a>.</strong> Still functional for basic SMTP capture…</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
```

After:

```html
<!-- wp:group {"layout":{"type":"constrained"}} -->
<div class="wp-block-group"><!-- wp:heading -->
<h2 class="wp-block-heading">Mailtrap Alternatives Worth Your Attention: A Snapshot</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Local development default — <a href="#mailpit">Mailpit</a>.</strong> Docker container running in under two minutes…</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Legacy local testing, with caveats — <a href="#mailhog">MailHog</a>.</strong> Still functional for basic SMTP capture…</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></div>
<!-- /wp:group -->
```
