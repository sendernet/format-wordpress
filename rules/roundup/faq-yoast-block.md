# Convert FAQ section to Yoast FAQ block


When the post contains an FAQ section — an `<!-- wp:heading -->` H2 whose text
**starts with or contains** `FAQ`, `FAQs`, or `Frequently Asked Questions`
(case-insensitive match, e.g. "FAQ about Klaviyo Alternatives" qualifies) —
replace the question/answer sequence that follows with a single
`<!-- wp:yoast/faq-block ... -->` block. Keep the H2 heading unchanged above it.
Drop any trailing empty paragraph (`<p></p>`) inside the FAQ section.

## Detecting questions and answers

Questions may appear as **either**:

- `<!-- wp:heading {"level":3} -->` blocks (H3 headings), **or**
- `<!-- wp:paragraph -->` blocks whose inner text **ends with a `?`**

Both forms are treated identically — each is a question node that starts a new
Q&A pair.

Answers are **one or more** blocks that follow a question node and are NOT
themselves a question node. Answer blocks may be:

- `<!-- wp:paragraph -->` blocks (the answer text)
- `<!-- wp:list -->` blocks (convert list items to text — see below)

If a question has multiple answer blocks, join them in order:
- Two adjacent paragraph blocks → join with `<br><br>`
- A list block → extract each `<li>` inner text as a line, join the lines with
  `<br><br>`, then join to any surrounding paragraphs with `<br><br>`

## Building the block

For each question/answer pair:

- **Question text** — the inner text of the H3 or the paragraph.
- **Answer text** — assembled from the answer blocks per the joining rules above.
- **`id`** — `faq-question-<timestamp>`, where `<timestamp>` is a unique number
  incremented per question so each id is unique.
- The `question` and `jsonQuestion` fields are identical; the `answer` and
  `jsonAnswer` fields are identical. `images` is always `[]`.

**Escaping inside the JSON attributes** (the `{...}` blob inside
`<!-- wp:yoast/faq-block {...} -->`): the JSON must be strictly valid. Apply
all four rules below to both `question`/`answer` and `jsonQuestion`/`jsonAnswer`:

1. **Angle brackets** — unicode-escape so the tags don't conflict with the HTML
   comment parser:
   - `<` → `<` (**never** `&lt;` — HTML entities inside JSON render as literal text)
   - `>` → `>` (**never** `&gt;`)
2. **Double-quote characters inside HTML attribute values** — any `"` that
   appears inside an HTML tag (e.g. `href="…"`, `target="_blank"`) must be
   escaped as `\"` in the JSON string. Example: an anchor becomes
   `<a href=\"https://example.com\" target=\"_blank\" rel=\"noopener\">text</a>`.
3. **Literal newline / carriage-return characters** — strip them. A raw newline
   inside a JSON string is a hard syntax error. If "Gmail" or any other word
   sits on its own line in the source (e.g. from a Google Docs export), fold it
   inline with the surrounding sentence and remove the surrounding whitespace.
4. **Double-hyphen sequences** — `--` (two consecutive hyphens) would close the
   HTML comment early; escape as `--`.

Inline tags like `<strong>`, `<em>`, `<br>` follow
rule 1 + 2 above. A `<br><br>` join between paragraphs is
the correct JSON representation of the paragraph break.

The visible HTML rendered below the comment (the `<div class="schema-faq...">`)
keeps real tags — `<strong>`, `<br>`, etc. — only the JSON inside the comment
is unicode-escaped.

After the comment, emit the rendered HTML block exactly in this shape:

```html
<div class="schema-faq wp-block-yoast-faq-block">
  <div class="schema-faq-section" id="faq-question-<id>">
    <strong class="schema-faq-question">Question text</strong>
    <p class="schema-faq-answer">Answer text with real <br><br> between joined blocks</p>
  </div>
  <!-- repeat per question -->
</div>
<!-- /wp:yoast/faq-block -->
```

## Constraints

- If the section already uses `<!-- wp:competitor-review-blocks/pros-and-cons -->`
  or `<!-- wp:yoast/faq-block -->`, leave it alone.
- If no question/answer pairs are found after the FAQ H2, skip the rule.

## Example

Before:

```html
<!-- wp:heading -->
<h2 class="wp-block-heading">FAQs about Mailtrap Alternatives</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Does Mailpit persist messages between Docker container restarts?</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Not by default. Mailpit holds captured messages in memory. For persistence, pass the --db-file flag with a path mounted from your host.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
```

After:

```html
<!-- wp:heading -->
<h2 class="wp-block-heading">FAQs about Mailtrap Alternatives</h2>
<!-- /wp:heading -->

<!-- wp:yoast/faq-block {"questions":[{"id":"faq-question-1779278186168","question":"Does Mailpit persist messages between Docker container restarts?","answer":"Not by default. Mailpit holds captured messages in memory. For persistence, pass the --db-file flag with a path mounted from your host.","jsonQuestion":"Does Mailpit persist messages between Docker container restarts?","jsonAnswer":"Not by default. Mailpit holds captured messages in memory. For persistence, pass the --db-file flag with a path mounted from your host.","images":[]}]} -->
<div class="schema-faq wp-block-yoast-faq-block"><div class="schema-faq-section" id="faq-question-1779278186168"><strong class="schema-faq-question">Does Mailpit persist messages between Docker container restarts?</strong> <p class="schema-faq-answer">Not by default. Mailpit holds captured messages in memory. For persistence, pass the --db-file flag with a path mounted from your host.</p> </div> </div>
<!-- /wp:yoast/faq-block -->
```
