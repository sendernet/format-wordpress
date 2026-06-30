# Tool heading anchors and h-toc class

In roundup posts each tool gets its own section, headed by an H3 like
"Mailpit — Lightweight Self-Hosted Alternative". These headings need a stable
HTML anchor (so the TL;DR and other sections can deep-link to them) and the
`h-toc` CSS class (so they're picked up by the in-page table of contents).

## What counts as a "tool heading"

An H3 (`<!-- wp:heading {"level":3} -->`) inside the body of a roundup that
introduces one of the tools being reviewed. Typical signals:

- The heading starts with the tool's brand name, often followed by an em-dash
  and a one-line tagline (e.g. `Mailpit — Lightweight Self-Hosted Alternative`).
- The tool is one of the items listed in the TL;DR / snapshot section near
  the top of the post.
- The H3 is followed by tool-section blocks (overview paragraph, Key Features,
  Pros & Cons, Pricing, Best for…).

Do **not** treat H3s inside the methodology, migration, comparison, or FAQ
sections as tool headings — only the per-tool review H3s in the main body.

## What to add

For each qualifying H3, update both the block JSON and the rendered tag:

1. **Anchor / id** — set the slug from the tool name (see "Picking the slug"
   below). Add `"anchor":"<slug>"` to the heading's JSON attributes and
   `id="<slug>"` on the `<h3>` element.
2. **`h-toc` class** — add `"className":"h-toc"` to the heading's JSON
   attributes (merge if `className` already exists; don't duplicate
   `h-toc` if it's already there). On the `<h3>`, append `h-toc` to the
   existing class list so it becomes `class="wp-block-heading h-toc"`.

Preserve any other existing attributes (e.g. `{"level":3}`, `textAlign`).

## Picking the slug

Prefer the slug the post already uses; only derive a new one as a fallback.

1. **Source of truth: the TL;DR section.** If the post has a TL;DR / snapshot
   section (see the [TL;DR group wrap rule](tldr-group-wrap.md)), its list
   items link to each tool via `<a href="#slug">Tool Name</a>`. Match each H3
   to its TL;DR list item by tool name and reuse that `#slug` verbatim.
2. **Fallback: slugify the tool name.** When the TL;DR doesn't contain a
   matching link (or there is no TL;DR), derive the slug from the tool name
   portion of the heading — the part before the first em-dash, en-dash, hyphen
   separator, or colon. Lowercase it, strip non-alphanumeric characters, and
   replace any internal whitespace with `-`. Examples: `Mailpit` → `mailpit`,
   `MailHog` → `mailhog`, `SendGrid` → `sendgrid`, `Amazon SES` →
   `amazon-ses`.

If the heading already has an `anchor` (and matching `id`), leave the slug
alone — only add `h-toc` if missing.

## Example

Before:

```html
<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Mailpit — Lightweight Self-Hosted Alternative</h3>
<!-- /wp:heading -->
```

After (TL;DR uses `<a href="#mailpit">Mailpit</a>`):

```html
<!-- wp:heading {"level":3,"anchor":"mailpit","className":"h-toc"} -->
<h3 class="wp-block-heading h-toc" id="mailpit">Mailpit — Lightweight Self-Hosted Alternative</h3>
<!-- /wp:heading -->
```
