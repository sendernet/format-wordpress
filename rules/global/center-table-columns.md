# Center all table columns


For every table cell — every `<td>` and `<th>` in the markup — make the content
horizontally centered:

- Add the class `has-text-align-center` to the cell.
  - Merge it into an existing `class="..."` attribute; don't create a duplicate
    `class` attribute.
  - If the cell already has a different `has-text-align-*` class, replace it with
    `has-text-align-center`.
- Add `data-align="center"` to the cell (replace any existing `data-align`). This
  keeps the core/table block valid when reopened in the editor.

Example — before:

```html
<!-- wp:table -->
<figure class="wp-block-table">
  <table>
    <tbody>
      <tr>
        <td>A</td>
        <th class="lead">B</th>
      </tr>
    </tbody>
  </table>
</figure>
<!-- /wp:table -->
```

After:

```html
<!-- wp:table -->
<figure class="wp-block-table">
  <table>
    <tbody>
      <tr>
        <td
          class="has-text-align-center"
          data-align="center"
        >
          A
        </td>
        <th
          class="lead has-text-align-center"
          data-align="center"
        >
          B
        </th>
      </tr>
    </tbody>
  </table>
</figure>
<!-- /wp:table -->
```
