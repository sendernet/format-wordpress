---
name: format-wordpress
description: Format WordPress Gutenberg post markup. Handles two workflows — (1) new post: user pastes raw Gutenberg code to format; (2) post update: user sends either a markdown export from Google Docs or WP HTML with new content, and Claude merges it with the most recent Gutenberg output in the conversation. Global rules always apply; roundup rules apply automatically when the post reviews/compares tools or software; educational rules apply when the post is a how-to guide or tutorial. Triggers on /format-wordpress or when the user provides Gutenberg/WP block code and asks to format or update it.
---

# Format Gutenberg

This skill handles two workflows:

## Workflow 1 — New post

The user provides raw WordPress Gutenberg post markup (block HTML with `<!-- wp:... -->`
comment delimiters). Apply all relevant formatting rules and return the full updated
markup in a single fenced code block.

## Workflow 2 — Post update

The user provides **two inputs**:
1. The existing live Gutenberg code of the post (already formatted).
2. A Google Docs copy-paste of the updated post content — plain text or lightly
   formatted, containing additions, removals, and/or rewritten sections compared
   to the live version.

**How to merge:**
- Treat the existing Gutenberg code as the authoritative source of structure and
  formatting for all content that hasn't changed.
- Diff the Google Docs content against the existing post to identify what is new,
  removed, or rewritten.
- For **removed** content: delete the corresponding Gutenberg blocks entirely.
- For **unchanged** content: keep the existing Gutenberg blocks exactly as-is —
  do not reformat or touch them.
- For **new or rewritten** content: produce correct Gutenberg markup for it and
  apply all relevant formatting rules to those blocks only.
- Preserve all custom block attributes, anchors, classes, and IDs on unchanged blocks.
- The Google Docs content is inconsistent — it sometimes includes banners, disclosure
  blocks, methodology blocks, pros/cons blocks, and other special blocks, and sometimes
  skips them entirely. Do not treat missing blocks in Google Docs as a signal to remove
  them from the Gutenberg output. Focus on **text content changes** (new paragraphs,
  rewritten sections, added/removed tools, updated copy) and ignore the presence or
  absence of structural/banner blocks in the Google Docs paste.
- If the scope of changes is unclear, ask the user to clarify before proceeding.

In both workflows, apply the formatting rules below. Do not change anything the rules don't cover.

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

## Educational rules

Apply only when the user has said the post is an educational post. Skip the
whole section otherwise.

*(No educational-specific rules yet — see `rules/educational/`.)*

## Output

- Return the complete formatted markup, not just the changed parts.
- Use **one single fenced code block** for the entire post. Never split the output across multiple code blocks regardless of post length.
- Do not put any text, commentary, headings, or "continuing from…" notes between code blocks. The entire formatted post must appear as one unbroken fenced block.
- Don't add commentary inside the code block.
