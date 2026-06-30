# Center images and use full size

For every `<!-- wp:image ... -->` block in the markup, force the image to be
center-aligned at full size.

Apply all four changes to each image block:

1. **JSON attributes** — in the block's opening `<!-- wp:image {...} -->`
   comment, set/merge these keys (preserve any other existing keys such as
   `id`, `linkDestination`):
   - `"sizeSlug":"full"` (replace any other `sizeSlug` value)
   - `"align":"center"` (add if missing)
   - `"scale":"cover"` (add if missing)
2. **Figure classes** — the `<figure>` must carry exactly
   `wp-block-image aligncenter size-full`. Replace any existing
   `size-*` class with `size-full` and add `aligncenter` if not already there.
   Don't drop unrelated classes if present.
3. **`<img>` style** — add `style="object-fit:cover"` to the `<img>` (merge
   into an existing `style` attribute if present; replace any prior
   `object-fit` value).
4. **`src` URL** — strip the WordPress size suffix (`-WIDTHxHEIGHT` right
   before the file extension) from the `src` so it points to the full-size
   original. Example: `.../Screenshot-1024x701.png` →
   `.../Screenshot.png`. If the URL has no suffix, leave it alone.

If an image block already matches all of the above, don't change anything.

Example — before:

```html
<!-- wp:image {"id":30941,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="http://localhost:10018/wp-content/uploads/2026/06/Screenshot-2026-06-15-at-09.21.44-1024x701.png" alt="" class="wp-image-30941"/></figure>
<!-- /wp:image -->
```

After:

```html
<!-- wp:image {"id":30941,"scale":"cover","sizeSlug":"full","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-full"><img src="http://localhost:10018/wp-content/uploads/2026/06/Screenshot-2026-06-15-at-09.21.44.png" alt="" class="wp-image-30941" style="object-fit:cover"/></figure>
<!-- /wp:image -->
```
