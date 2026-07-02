# Strip the H1

The H1 is the post title, which WordPress renders from the post's title field —
it is never part of the post body markup. If the input (markdown or Gutenberg
HTML) contains an H1, remove it entirely from the output.

- Markdown input: drop any `# Heading` line (single leading `#`). It is almost
  always the first line of a Google Docs export.
- Gutenberg HTML input: drop the whole heading block containing an `<h1>`,
  including its `<!-- wp:heading {"level":1} -->` / `<!-- /wp:heading -->`
  delimiters.
- Remove the H1 only — do not demote it to an H2 or move its text elsewhere.
- Do not touch H2–H6 headings; they stay as the post's section structure.
- If there are somehow multiple H1s, remove all of them.

Example — markdown input:

```markdown
# 10 Best Mailchimp Alternatives in 2026

Looking for a Mailchimp alternative? ...

## Why look for an alternative?
```

Output starts directly with the intro paragraph block — no heading block for
"10 Best Mailchimp Alternatives in 2026":

```html
<!-- wp:paragraph -->
<p>Looking for a Mailchimp alternative? ...</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Why look for an alternative?</h2>
<!-- /wp:heading -->
```
