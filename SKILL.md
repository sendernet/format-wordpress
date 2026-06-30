---
name: format-wordpress
description: Format WordPress Gutenberg post markup. The user sends either Gutenberg HTML or a Markdown export from Google Docs, and Claude returns fully formatted Gutenberg HTML. Global rules always apply; roundup rules apply automatically when the post reviews/compares tools or software; educational rules apply when the post is a how-to guide or tutorial. Triggers on /format-wordpress or when the user provides Gutenberg/WP block code or markdown and asks to format it.
---

# Format Gutenberg

The user sends either Gutenberg HTML or a Markdown export (from Google Docs). Return
fully formatted Gutenberg HTML in both cases.

**Detect the input type:**
- Input contains `<!-- wp:... -->` block comment delimiters → **Gutenberg HTML**.
  Apply all relevant formatting rules and return the full updated markup.
- Input is plain text or markdown (no `<!-- wp:... -->` delimiters) → **Markdown**.
  Convert the markdown to correct Gutenberg block markup and apply all relevant
  formatting rules.

**Images in markdown input:** wherever an image appears in the markdown, output an
empty `wp:image` block at that position — no inner HTML, just the block comments.
Apply all relevant image formatting rules (attributes, size slug, etc.) but leave
the content blank since the media library ID and URL are not known yet:

```
<!-- wp:image {"sizeSlug":"large","linkDestination":"none"} -->

<!-- /wp:image -->
```

Apply the formatting rules below. Do not change anything the rules don't cover.

Rules are grouped into three categories:

- **Global rules** apply to every post.
- **Roundup rules** apply when the post is an alternatives, software overview, or
  roundup post. Detect this from the content: if the post reviews or compares
  multiple tools/products with individual sections per tool, pros/cons lists, or
  tool feature breakdowns — it's a roundup. Apply roundup rules automatically.
- **Educational rules** apply when the post is a how-to guide, tutorial, or
  explainer. Detect this from the content: if the post teaches a concept or walks
  through steps to accomplish something rather than comparing products — it's
  educational. Apply educational rules automatically.

Detect the post type from the content itself. If the post type is ambiguous and you cannot confidently classify it as roundup or educational, ask the user before applying any non-global rules.

Each rule lives in its own file under `rules/<category>/`. Read the relevant
file(s) before applying a rule — the details (escaping requirements, exact
output shape, examples) are there, not in this index.

## Global rules

Always apply all of:

- [Center all table columns](rules/global/center-table-columns.md)
- [Bold the first row of every table](rules/global/first-row-bold.md)
- [Link targets and rel attributes](rules/global/link-attributes.md)
- [Only Sender pricing links are allowed](rules/global/no-competitor-pricing-links.md)
- [Center images and use full size](rules/global/image-center-full.md)
- [Headings must not be bold](rules/global/headings-not-bold.md)

## Roundup rules (Alternative Solutions / Software Overviews)

Apply only when the user has said the post is an alternatives, software
overview, or roundup. Skip the whole section otherwise.

- [Disclosure block](rules/roundup/disclosure-block.md)
- [Wrap the TL;DR / snapshot section in a wp:group](rules/roundup/tldr-group-wrap.md)
- [Methodology banner](rules/roundup/methodology-banner.md)
- [Tool heading anchors and h-toc class](rules/roundup/tool-heading-anchors.md)
- [Link tool brand names to their tool section](rules/roundup/tool-name-anchor-links.md)
- [Separator between tool sections](rules/roundup/tool-section-separators.md)
- [Convert Pros & Cons lists to the competitor-review block](rules/roundup/pros-cons-block.md)
- [Sender section CTAs](rules/roundup/sender-section-ctas.md)
- [Convert FAQ section to Yoast FAQ block](rules/roundup/faq-yoast-block.md)
- [Constrain images to 800px width](rules/roundup/image-width-800.md) — overrides the global image rule for roundup posts
- [Rating card after first paragraph of each tool section](rules/roundup/rating-card.md) — fetch reviews from the Sender WP REST API and insert the rating card block
- [Migration banner](rules/roundup/migration-banner.md) — **Alternative Solutions posts only**: insert one migration banner near migration-related content, with the main tool's name interpolated

## Educational rules

Apply only when the user has said the post is an educational post. Skip the
whole section otherwise.

*(No educational-specific rules yet — see `rules/educational/`.)*

## Output

- Return the complete formatted markup, not just the changed parts.
- Use **one single fenced code block** for the entire post. Never split the output across multiple code blocks regardless of post length.
- Do not put any text, commentary, headings, or "continuing from…" notes between code blocks. The entire formatted post must appear as one unbroken fenced block.
- Don't add commentary inside the code block.
