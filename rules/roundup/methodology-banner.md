# Methodology banner

Roundup posts that include a section explaining the review methodology — e.g.
"How We Evaluated X", "How We Tested", "Our Methodology", "Testing
Methodology", "How We Picked" — should end that section with the methodology
banner block:

```html
<!-- wp:block {"ref":28276} /-->
```

## Where to insert

Place the banner on its own line as the **last block of the methodology
section**, i.e. immediately after the section's final block and immediately
before:

- the next `<!-- wp:heading -->` H2 block (the start of the next section), or
- the end of the post if no further H2 follows.

Keep one blank line of separation above and below the banner so it sits as a
distinct block.

## Constraints

- **At most one banner per post.** If `<!-- wp:block {"ref":28276} /-->`
  already appears anywhere in the markup, do not add another.
- **Insert only when confident** the section is genuinely about the review
  methodology. The H2 text should clearly signal it (contains words like
  "evaluated", "tested", "methodology", "how we picked", "our process"). If
  the wording is ambiguous and you can't tell, skip this rule.
- **Don't confuse with a results/comparison section.** A section titled
  "Comparison", "Verdict", "Recommendations", "Top Picks" is not a
  methodology section — skip those.

## Example

Before (only the tail of the methodology section is shown):

```html
<!-- wp:paragraph -->
<p>To learn more about our in-house testing methodology, head to this article, which breaks down the process we follow to deliver accurate, unbiased reviews.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading">The Alternatives</h2>
<!-- /wp:heading -->
```

After:

```html
<!-- wp:paragraph -->
<p>To learn more about our in-house testing methodology, head to this article, which breaks down the process we follow to deliver accurate, unbiased reviews.</p>
<!-- /wp:paragraph -->

<!-- wp:block {"ref":28276} /-->

<!-- wp:heading -->
<h2 class="wp-block-heading">The Alternatives</h2>
<!-- /wp:heading -->
```
