# Only Sender pricing links are allowed

Pricing pages of other vendors should not be linked from the post. Sender is
the only brand whose pricing page may carry a hyperlink — every other
vendor's pricing URL should appear as plain text instead.

## What to detect

An `<a href="…">…</a>` whose `href` points to a pricing-style page on a
**non-Sender** domain. Treat a URL as a pricing page when its path contains
any of these segments (case-insensitive):

- `/pricing`
- `/pricing/`
- `/plans`
- `/price`

If the domain (or any subdomain) is `sender.net`, leave the link alone — the
[link targets and rel rule](link-attributes.md) handles its `target`/`rel`.

## What to do

Replace the `<a>…</a>` with just its inner text. Keep the surrounding
punctuation, whitespace, and any inline formatting (e.g. `<strong>`,
`<em>`) intact. Don't reword the visible text.

If only part of a sentence's link is the pricing URL (i.e. the entire
sentence isn't the link), only that one anchor is unlinked — neighboring
links to non-pricing pages are not affected by this rule.

## Constraints

- **Don't unlink non-pricing pages**, even on competitor domains. A link to
  `brevo.com/blog/x` or `mailgun.com/features` stays as a link (the global
  link rule applies its `target`/`rel`).
- **Don't add a link** to a vendor's pricing if one is missing — this rule
  only removes existing links, never creates them.
- If the pricing link points to Sender (`sender.net` or any `*.sender.net`
  subdomain, or an internal path), keep it linked.

## Example

Before:

```html
<!-- wp:paragraph -->
<p>Brevo charges from <a href="https://www.brevo.com/pricing/">$25/month</a> and Postmark from <a href="https://postmarkapp.com/pricing">$15/month</a>, while <a href="https://www.sender.net/pricing/">Sender</a> starts free.</p>
<!-- /wp:paragraph -->
```

After:

```html
<!-- wp:paragraph -->
<p>Brevo charges from $25/month and Postmark from $15/month, while <a href="https://www.sender.net/pricing/">Sender</a> starts free.</p>
<!-- /wp:paragraph -->
```
