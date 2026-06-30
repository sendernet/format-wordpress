# Link targets and rel attributes


For every `<a>` tag that has an `href`:

- **Exemption: in-page anchors.** If the `href` starts with `#` (an in-page
  jump to a heading on the same page, e.g. `href="#sender"`), skip this rule
  entirely — don't add `target` or `rel`. Opening an in-page anchor in a new
  tab makes no sense and breaks the jump.
- Always open in a new tab: set `target="_blank"`.
- Set `rel` based on where the link points:
  - **Sender links** — the `href` is relative/internal, or points to `sender.net`
    or any `*.sender.net` subdomain → **dofollow**: `rel="noopener"`. The `rel`
    must NOT contain `nofollow` (remove it if present).
  - **Non-Sender links** — the `href` points to any other domain → **nofollow**:
    `rel="noopener nofollow"`.
- Merge into existing `target`/`rel` attributes; don't create duplicate attributes.
  Preserve any other existing `rel` tokens (e.g. `sponsored`, `ugc`); only the
  `nofollow` token is added or removed per the rule above.

Example — before:

```html
<!-- wp:paragraph -->
<p>
  Try <a href="https://www.sender.net/pricing/">Sender pricing</a> or read this
  <a href="https://example.com/guide">external guide</a>.
</p>
<!-- /wp:paragraph -->
```

After:

```html
<!-- wp:paragraph -->
<p>
  Try
  <a
    href="https://www.sender.net/pricing/"
    target="_blank"
    rel="noopener"
    >Sender pricing</a
  >
  or read this
  <a
    href="https://example.com/guide"
    target="_blank"
    rel="noopener nofollow"
    >external guide</a
  >.
</p>
<!-- /wp:paragraph -->
```
