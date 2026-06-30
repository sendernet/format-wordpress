# Sender section CTAs

When the post includes a section that overviews Sender, drop in two CTA
placements inside that section: one right after the overview paragraph and
two more at the very end of the section.

## Where the section is

In a roundup, this is the tool section for Sender — the H3 tool heading whose
text starts with "Sender" or whose anchor is `#sender` (set by the
[tool heading anchors rule](tool-heading-anchors.md)). The expected layout
inside the section is:

```
H3 heading        → "Sender — Transactional + Marketing in One Platform"
image (optional)
paragraph         ← first / overview paragraph of the section
…more blocks: "Key Features", list, Pros & Cons, Pricing, Best for…
(end of section: next H3, next H2, separator, or end of post)
```

If the post has no Sender section (no H3 about Sender, no `#sender` anchor
target), skip this rule entirely.

## Insertion 1: CTA after the overview paragraph

Place this block on its own line **immediately after the first
`<!-- wp:paragraph -->` block inside the Sender section**, with a blank line
above and below:

```html
<!-- wp:block {"ref":8435} /-->
```

The "first paragraph" is the first prose paragraph after the H3 (any image
or other non-paragraph block between the heading and that paragraph is
skipped). Don't place this CTA after later paragraphs ("Key Features",
"Pros", "Pricing", "Best for", etc.).

## Insertion 2: CTAs at the end of the section

Place these two blocks on their own lines, in this exact order, **as the
last blocks of the Sender section** — i.e. immediately after the section's
final block (typically the "Best for" paragraph) and immediately before
whichever of the following comes next:

- the [tool section separator](tool-section-separators.md) (if present),
- the next `<!-- wp:heading -->` (next tool H3 or next H2), or
- the end of the post.

```html
<!-- wp:block {"ref":22923} /-->

<!-- wp:block {"ref":29446} /-->
```

Keep a blank line between each block and from the surrounding blocks so they
sit as distinct blocks. The two end-of-section blocks belong to the Sender
content, so they go **before** the separator, not after it.

## Constraints

- **At most one of each block per post.** If `<!-- wp:block {"ref":8435} /-->`
  already appears anywhere in the markup, skip the first insertion. Same for
  `{"ref":22923}` and `{"ref":29446}` — only insert the ones missing.
- The two end-of-section blocks must appear together, with `22923` first and
  `29446` second.

## Example

Before (a Sender section in its entirety):

```html
<!-- wp:heading {"level":3,"anchor":"sender","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="sender">Sender — Transactional + Marketing in One Platform</h3>
<!-- /wp:heading -->

<!-- wp:image ... -->
<figure class="wp-block-image …"><img …/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Sender combines transactional SMTP with marketing automation and custom events triggering, on a generous free tier and a single dashboard for both sides of email.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Key Features</strong></p>
<!-- /wp:paragraph -->

<!-- (list, Pros & Cons, Pricing blocks omitted for brevity) -->

<!-- wp:paragraph -->
<p><strong>Best for:</strong> Teams that want transactional and marketing email in one platform without juggling two vendors.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3,"anchor":"mailgun","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="mailgun">Mailgun — High-Volume Transactional</h3>
<!-- /wp:heading -->
```

After:

```html
<!-- wp:heading {"level":3,"anchor":"sender","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="sender">Sender — Transactional + Marketing in One Platform</h3>
<!-- /wp:heading -->

<!-- wp:image ... -->
<figure class="wp-block-image …"><img …/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Sender combines transactional SMTP with marketing automation and custom events triggering, on a generous free tier and a single dashboard for both sides of email.</p>
<!-- /wp:paragraph -->

<!-- wp:block {"ref":8435} /-->

<!-- wp:paragraph -->
<p><strong>Key Features</strong></p>
<!-- /wp:paragraph -->

<!-- (list, Pros & Cons, Pricing blocks omitted for brevity) -->

<!-- wp:paragraph -->
<p><strong>Best for:</strong> Teams that want transactional and marketing email in one platform without juggling two vendors.</p>
<!-- /wp:paragraph -->

<!-- wp:block {"ref":22923} /-->

<!-- wp:block {"ref":29446} /-->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3,"anchor":"mailgun","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="mailgun">Mailgun — High-Volume Transactional</h3>
<!-- /wp:heading -->
```
