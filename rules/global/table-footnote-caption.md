# Table Footnote Caption

If a table has cells containing asterisk markers (`*`, `**`, `***`, etc.) AND the
paragraph immediately following the `<!-- /wp:table -->` closing comment is a
footnote-style explanation that starts with those same markers — move that paragraph
**inside** the `<figure>` element as a `<figcaption>`, then delete the standalone
paragraph block.

## Detection

A footnote paragraph qualifies when **both** conditions are true:

1. It immediately follows a `<!-- /wp:table -->` comment (no other blocks between
   them).
2. Its visible text starts with one or more `*` characters (the literal asterisk,
   not bold markup), e.g. `* One block equals…` or `** Deliverability rates…`.

The paragraph is typically wrapped in `<em>` tags and may contain `<br>` line
breaks separating multiple footnote entries. Accept any combination of inner markup
as long as the text content starts with `*`.

## Output shape

Add a `<figcaption class="wp-element-caption">` as the last child of `<figure
class="wp-block-table">`, preserving the inner HTML of the `<p>` verbatim:

```html
<!-- wp:table -->
<figure class="wp-block-table"><table …>…</table><figcaption class="wp-element-caption"><em>* One block equals 25,000 email sends + users have to pay for their Mailchimp plan separately.<br>** Deliverability rates from EmailToolTester (2026). Sender's rate based on internal testing. Results vary by domain reputation and sending practices.</em></figcaption></figure>
<!-- /wp:table -->
```

Remove the original `<!-- wp:paragraph -->…<!-- /wp:paragraph -->` block entirely.

## Example — before

```html
<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody>…</tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><em>* One block equals 25,000 email sends + users have to pay for their Mailchimp plan separately.<br>** Deliverability rates from EmailToolTester (2026). Sender's rate based on internal testing. Results vary by domain reputation and sending practices.</em></p>
<!-- /wp:paragraph -->
```

## Example — after

```html
<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody>…</tbody></table><figcaption class="wp-element-caption"><em>* One block equals 25,000 email sends + users have to pay for their Mailchimp plan separately.<br>** Deliverability rates from EmailToolTester (2026). Sender's rate based on internal testing. Results vary by domain reputation and sending practices.</em></figcaption></figure>
<!-- /wp:table -->
```

## Notes

- Copy the inner HTML of `<p>` directly into `<figcaption>` — do not strip or alter
  the `<em>`, `<br>`, or any other tags.
- If the table already has a `<figcaption>`, do not add another one.
- Apply this rule even when the table itself does not use asterisks in its cells —
  the paragraph alone qualifies if it starts with `*`.
