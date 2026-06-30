# Bold the first row of every table

For every `<table>` in the markup, the cells in the first row act as the
header — wrap their content in `<strong>` so the row reads as bold.

- The "first row" is the first `<tr>` inside the table (whether under
  `<thead>` or `<tbody>`).
- Apply to every cell in that row, whether it's a `<td>` or a `<th>`.
- Wrap only the inner content of the cell in `<strong>...</strong>`. Don't
  add `<strong>` around the cell tag itself.
- If a cell's content is already wrapped in `<strong>`, leave it alone —
  don't double-wrap.
- Don't bold rows other than the first one. Rows 2+ keep their original
  formatting.

This rule composes with [Center all table columns](center-table-columns.md):
the first row's cells get both `<strong>` content and the centering
class/attribute.

Example — before:

```html
<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td>Setting</td><td>Value</td></tr><tr><td>Host</td><td>localhost</td></tr></tbody></table></figure>
<!-- /wp:table -->
```

After (with centering applied as well):

```html
<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><td class="has-text-align-center" data-align="center"><strong>Setting</strong></td><td class="has-text-align-center" data-align="center"><strong>Value</strong></td></tr><tr><td class="has-text-align-center" data-align="center">Host</td><td class="has-text-align-center" data-align="center">localhost</td></tr></tbody></table></figure>
<!-- /wp:table -->
```
