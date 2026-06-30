# Link tool brand names to their tool section

In a roundup, brand names that appear in the TL;DR / snapshot section and in
the comparison table should jump to that tool's review section when clicked.
For every tool that has its own H3 section (and therefore an anchor set by
the [tool heading anchors rule](tool-heading-anchors.md)), wrap the bare
brand name in an anchor link to that section's `#slug`.

## Which tools qualify

Only tools that have a corresponding tool H3 in the post. Build the
brand → slug map from the H3 headings (the same ones that received
`anchor="<slug>"` / `id="<slug>"`). A brand that is mentioned in the TL;DR
or comparison table but has no dedicated H3 section (e.g. "Mailtrap (kept)"
in a Mailtrap-alternatives roundup, where Mailtrap isn't being reviewed)
gets no link — leave that text alone.

## Where to apply

Two places, and only these two:

1. **TL;DR / snapshot list** — the `<!-- wp:list -->` inside the
   `<!-- wp:group -->` set by the
   [TL;DR group wrap rule](tldr-group-wrap.md). Each `<li>` typically has
   the shape `<strong>Use case description — Brand.</strong> Sentence…` —
   the brand name sits after the em-dash and before the period, inside the
   leading `<strong>`. Wrap just the brand text in
   `<a href="#slug">Brand</a>`, keeping the surrounding `<strong>`,
   em-dash, period, and trailing sentence intact.
2. **Comparison table** — the table that compares all tools side-by-side
   (typically titled "Comparison Table", "Quick Comparison", or similar,
   with a "Platform"/"Tool"/"Vendor" first column listing brand names).
   For each row's brand cell, wrap the brand text in
   `<a href="#slug">Brand</a>`, keeping the surrounding `<strong>`
   (added by the [first-row-bold rule](../global/first-row-bold.md) for the
   header row, and present in the brand column by convention) intact.
   Don't link brand names that appear in other cells of the table
   (descriptions, "Stands out for", etc.) — only the brand column.

Do **not** auto-link brand mentions elsewhere in the article body — only in
these two anchored summary surfaces.

## How to wrap

Place the `<a>` immediately around the brand text, **inside** the existing
`<strong>` if there is one:

```html
<strong><a href="#sender">Sender</a></strong>
```

Match the brand text exactly as it appears (case-sensitive, including
characters like the capital H in "MailHog"). Don't expand abbreviations or
add tooltips.

The [link targets and rel rule](../global/link-attributes.md) exempts
in-page anchors (`href` starting with `#`), so these links stay as plain
`<a href="#slug">…</a>` without `target` or `rel`.

## Constraints

- If a brand name is already linked to its `#slug`, leave it alone — don't
  double-wrap.
- If a brand name in the TL;DR or comparison table is already linked to a
  different URL (e.g. an external vendor page), replace that link with the
  `#slug` anchor link — the in-post jump takes precedence.
- The brand → slug map is derived from H3 headings present in this post.
  Don't fabricate slugs for tools that have no H3.

## Examples

TL;DR item — before:

```html
<li><strong>Transactional + marketing in one platform — Sender.</strong> Native transactional SMTP alongside marketing automation and custom events triggering, on a free tier of 15,000 emails/month.</li>
```

After (Sender has an H3 with `anchor="sender"`):

```html
<li><strong>Transactional + marketing in one platform — <a href="#sender">Sender</a>.</strong> Native transactional SMTP alongside marketing automation and custom events triggering, on a free tier of 15,000 emails/month.</li>
```

Comparison table row — before:

```html
<tr><td class="has-text-align-center" data-align="center"><strong>Sender</strong></td><td class="has-text-align-center" data-align="center">Transactional + marketing</td>…</tr>
```

After:

```html
<tr><td class="has-text-align-center" data-align="center"><strong><a href="#sender">Sender</a></strong></td><td class="has-text-align-center" data-align="center">Transactional + marketing</td>…</tr>
```

TL;DR item for a brand without an H3 — leave alone:

```html
<li><strong>Hosted manual QA inspection — Mailtrap (kept).</strong> As an email sandbox tool, Mailtrap delivers clean inbox UI …</li>
```
