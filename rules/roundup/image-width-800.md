# Constrain images to 800px width

For roundup posts, every `<!-- wp:image ... -->` block should be centered at
full size **and** capped at 800px wide. This rule **replaces** the global
[Center images and use full size](../global/image-center-full.md) rule for
roundup posts — apply this one instead, not both.

Apply all of the following to each image block:

1. **JSON attributes** — in the block's opening `<!-- wp:image {...} -->`
   comment, set/merge these keys (preserve any other existing keys such as
   `id`, `linkDestination`):
   - `"sizeSlug":"full"` (replace any other `sizeSlug` value)
   - `"align":"center"` (add if missing)
   - `"width":"800px"` (add if missing; replace any prior value)
   - Remove `"scale":"cover"` if it was added by the global rule.
2. **Figure classes** — the `<figure>` must carry
   `wp-block-image aligncenter size-full is-resized`. Replace any existing
   `size-*` class with `size-full`, and add `aligncenter` and `is-resized` if
   not already present. Don't drop unrelated classes if present.
3. **`<img>` style** — set `style="width:800px"` (merge into an existing
   `style` attribute if present; replace any prior `width` value, and remove
   `object-fit` if it was set by the global rule).
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
<!-- wp:image {"id":30941,"width":"800px","sizeSlug":"full","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-full is-resized"><img src="http://localhost:10018/wp-content/uploads/2026/06/Screenshot-2026-06-15-at-09.21.44.png" alt="" class="wp-image-30941" style="width:800px"/></figure>
<!-- /wp:image -->
```
