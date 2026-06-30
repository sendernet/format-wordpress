# Convert Pros & Cons lists to the competitor-review block

In each tool section, the "Pros & Cons" subsection is written as two
paragraph + list pairs. Replace that sequence with the dedicated
`wp:competitor-review-blocks/pros-and-cons` block so it renders as the
side-by-side pros/cons card.

## What to detect

A sequence of five blocks in this exact order, anywhere in a tool section:

1. A "Pros & Cons" label block — either an H4 heading
   (`<!-- wp:heading {"level":4} --> … Pros & Cons … <!-- /wp:heading -->`)
   or a paragraph wrapping the text in `<strong>` (e.g.
   `<p><strong>Pros &amp; Cons</strong></p>`). The text may use a literal
   ampersand or `&amp;`.
2. A `<!-- wp:paragraph -->` containing `<strong>Pros</strong>` only.
3. A `<!-- wp:list -->` block — the pros list.
4. A `<!-- wp:paragraph -->` containing `<strong>Cons</strong>` only.
5. A `<!-- wp:list -->` block — the cons list.

The "Pros & Cons" label block (1) is **kept as-is**. Blocks (2)–(5) are
replaced by a single `wp:competitor-review-blocks/pros-and-cons` block.

## How to build the block

For each item in the pros list and the cons list, take the inner text of the
`<li>` (preserve inline formatting like `<strong>` and `<em>` if present;
don't strip it).

### JSON attributes

Set two string attributes:

- `pros`: each pros item, joined by a literal `\n\n` (two newline escape
  sequences). Inside the JSON the separator appears as `\n\n` between items.
- `cons`: same, for cons items.

Don't add other attributes.

### Rendered HTML

Emit this exact structure below the comment (single line in the file, broken
here for readability):

```html
<div class="wp-block-competitor-review-blocks-pros-and-cons competitor-review-pros-and-cons">
  <div class="competitor-review-pros-and-cons-pros">
    <div class="competitor-review-pros-and-cons-heading">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="none"><path fill="#5ABA85" d="M5.6 9.5 3.107 7.007a1 1 0 0 0-1.414 0l-.986.986a1 1 0 0 0 0 1.414l4.186 4.186a1 1 0 0 0 1.414 0l8.986-8.986a1 1 0 0 0 0-1.414l-.986-.986a1 1 0 0 0-1.414 0z"></path></svg>
      <span>Pros</span>
    </div>
    <ul>
      <li>…each pros item as its own li…</li>
    </ul>
  </div>
  <div class="competitor-review-pros-and-cons-border"></div>
  <div class="competitor-review-pros-and-cons-cons">
    <div class="competitor-review-pros-and-cons-heading">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="none"><path fill="#ED1E24" fill-rule="evenodd" d="M1.789 2.785a1 1 0 0 0 0 1.415l3.859 3.858L1.904 11.8a1 1 0 0 0 0 1.414l1.007 1.007a1 1 0 0 0 1.415 0l3.743-3.743 3.607 3.606a1 1 0 0 0 1.414 0l1.008-1.007a1 1 0 0 0 0-1.414L10.49 8.058l3.722-3.721a1 1 0 0 0 0-1.414l-1.008-1.008a1 1 0 0 0-1.414 0L8.069 5.636 4.211 1.778a1 1 0 0 0-1.415 0z" clip-rule="evenodd"></path></svg>
      <span>Cons</span>
    </div>
    <ul>
      <li>…each cons item as its own li…</li>
    </ul>
  </div>
</div>
```

The two SVG path data values are fixed — use them verbatim. Don't modify
colors (`#5ABA85` for pros checkmark, `#ED1E24` for cons cross) or any
attribute on the SVG/path.

The block closes with `<!-- /wp:competitor-review-blocks/pros-and-cons -->`
on the next line.

## Constraints

- If the section already uses
  `<!-- wp:competitor-review-blocks/pros-and-cons -->`, leave it alone.
- If the sequence is incomplete (e.g. there's a Pros list but no Cons list,
  or the order is different), don't transform it — leave the original blocks
  in place.
- The "Pros & Cons" label block is preserved exactly as it was, including
  its level (H4 vs. paragraph) and any class / formatting.

## Example

Before:

```html
<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Pros &amp; Cons</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Pros</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Most generous free tier in this comparison</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Transactional + marketing on one set of credentials</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom events fire sends directly from app code, no middleware</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Guided auth setup reduces DNS and deliverability errors</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Cons</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Dashboard noisy for pure transactional use — marketing UI fights for screen space</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Webhook and advanced API docs thinner than Postmark or Mailgun</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No self-hosting option</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Sender branding on free-plan emails</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
```

After:

```html
<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Pros &amp; Cons</h4>
<!-- /wp:heading -->

<!-- wp:competitor-review-blocks/pros-and-cons {"pros":"Most generous free tier in this comparison\n\nTransactional + marketing on one set of credentials\n\nCustom events fire sends directly from app code, no middleware\n\nGuided auth setup reduces DNS and deliverability errors","cons":"Dashboard noisy for pure transactional use — marketing UI fights for screen space\n\nWebhook and advanced API docs thinner than Postmark or Mailgun\n\nNo self-hosting option\n\nSender branding on free-plan emails"} -->
<div class="wp-block-competitor-review-blocks-pros-and-cons competitor-review-pros-and-cons"><div class="competitor-review-pros-and-cons-pros"><div class="competitor-review-pros-and-cons-heading"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="none"><path fill="#5ABA85" d="M5.6 9.5 3.107 7.007a1 1 0 0 0-1.414 0l-.986.986a1 1 0 0 0 0 1.414l4.186 4.186a1 1 0 0 0 1.414 0l8.986-8.986a1 1 0 0 0 0-1.414l-.986-.986a1 1 0 0 0-1.414 0z"></path></svg><span>Pros</span></div><ul><li>Most generous free tier in this comparison</li><li>Transactional + marketing on one set of credentials</li><li>Custom events fire sends directly from app code, no middleware</li><li>Guided auth setup reduces DNS and deliverability errors</li></ul></div><div class="competitor-review-pros-and-cons-border"></div><div class="competitor-review-pros-and-cons-cons"><div class="competitor-review-pros-and-cons-heading"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="none"><path fill="#ED1E24" fill-rule="evenodd" d="M1.789 2.785a1 1 0 0 0 0 1.415l3.859 3.858L1.904 11.8a1 1 0 0 0 0 1.414l1.007 1.007a1 1 0 0 0 1.415 0l3.743-3.743 3.607 3.606a1 1 0 0 0 1.414 0l1.008-1.007a1 1 0 0 0 0-1.414L10.49 8.058l3.722-3.721a1 1 0 0 0 0-1.414l-1.008-1.008a1 1 0 0 0-1.414 0L8.069 5.636 4.211 1.778a1 1 0 0 0-1.415 0z" clip-rule="evenodd"></path></svg><span>Cons</span></div><ul><li>Dashboard noisy for pure transactional use — marketing UI fights for screen space</li><li>Webhook and advanced API docs thinner than Postmark or Mailgun</li><li>No self-hosting option</li><li>Sender branding on free-plan emails</li></ul></div></div>
<!-- /wp:competitor-review-blocks/pros-and-cons -->
```
