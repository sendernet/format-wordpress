# Insert rating card after first paragraph of each tool section

For every tool section in a roundup post, fetch the matching review from the
Sender WP REST API and insert a `wp:competitor-review-blocks/rating-card` block
immediately after the first `<!-- wp:paragraph -->` block in that section.

## Step 1 — Fetch reviews

Fetch all pages before generating any output:

```
https://www.sender.net/wp-json/wp/v2/reviews?per_page=100&page=1&_fields=id,slug,title,review_data&include_hidden=true
https://www.sender.net/wp-json/wp/v2/reviews?per_page=100&page=2&_fields=id,slug,title,review_data&include_hidden=true
```

Keep fetching pages until a page returns an empty array or an error.

The data lives at these paths in each review object:

| Field | Path |
|---|---|
| Review ID | `id` |
| Slug | `slug` |
| Brand name (for matching) | `review_data.pros_cons.brand_name` → fallback `title.rendered` |
| Brand logo URL | `review_data.pros_cons.brand_logo` |
| Competitor website | `review_data.pros_cons.competitor_website_url` |
| Overall rating value | `review_data.pros_cons.overall_rating.value` |
| Platform ratings array | `review_data.pros_cons.overall_rating.external_ratings` |

## Step 2 — Match tool sections to reviews

For each tool section (identified by its H2/H3 heading), find the review whose
`review_data.pros_cons.brand_name` matches the tool heading text. Matching is
case-insensitive. If no review matches, skip inserting a rating card for that section.

## Step 3 — Build the block

Insert the block immediately after the first `<!-- wp:paragraph -->` in the tool
section. If the section already contains a
`<!-- wp:competitor-review-blocks/rating-card -->`, leave it as-is.

### Link logic

Derive these four fields from the review before building the block:

| Field | Rule |
|---|---|
| `link` | `review_data.pros_cons.competitor_website_url` |
| `linkText` | `"Visit Sender"` if `slug === "sender"`, else `"Visit website"` |
| `isNofollow` | `false` if `slug === "sender"`, else `true` |
| `target` | always `"_blank"` |

### JSON comment

```
<!-- wp:competitor-review-blocks/rating-card {"card":{"reviewId":"{id_string}","reviewData":{"id":{id_number},"slug":"{slug}","review_data":{"pros_cons":{"brand_logo":"{brand_logo}","brand_name":"{brand_name}","competitor_website_url":"{competitor_website_url}","overall_rating":{"value":"{overall_value}","external_ratings":[{external_ratings_json}]}}}},"link":"{link}","linkText":"{linkText}","target":"_blank","isNofollow":{isNofollow}}} -->
```

- `{id_string}` — review ID as a JSON string (e.g. `"23555"`)
- `{id_number}` — review ID as a JSON number (e.g. `23555`)
- `{slug}` — WP post slug
- `{brand_logo}` — from `review_data.pros_cons.brand_logo`
- `{brand_name}` — from `review_data.pros_cons.brand_name`
- `{competitor_website_url}` — from `review_data.pros_cons.competitor_website_url`
- `{overall_value}` — string from `review_data.pros_cons.overall_rating.value`
- `{external_ratings_json}` — the full `external_ratings` array serialized as JSON,
  including all entries even if `rating` or `url` are empty strings
- `{link}` / `{linkText}` / `{isNofollow}` — derived from the link logic table above

### Rendered HTML

Emit this structure (all on one line in the file):

```html
<div class="wp-block-competitor-review-blocks-rating-card competitor-review-rating-card"><div class="competitor-review-rating-card-logo"><img src="{brand_logo}" alt="{brand_name} logo"/></div><div class="competitor-review-rating-card-rating"><div class="competitor-review-rating-card-rating-overall-container"><div class="competitor-review-rating-card-rating-header">Overall rating:</div><div class="competitor-review-rating-card-rating-overall"><div><div>{overall_value}</div><div> /5</div></div>{star_svg}</div></div><div class="competitor-review-rating-card-rating-list">{platform_items}</div><div class="competitor-review-rating-card-rating-link"><a href="{link}" target="_blank" rel="{rel}" class="btn btn-lg nacked-btn">{linkText}<span class="btn-icon">{arrow_svg}{arrow_svg}</span></a></div></div></div>
```

**Star SVG** (use verbatim, appears after overall value and inside each platform item):
```html
<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" class="competitor-review-rating-card-rating-star"><path fill="#F75B11" d="M9.05 1.194c.298-.925 1.601-.925 1.9 0l1.676 5.177a1 1 0 0 0 .95.694h5.422c.969 0 1.371 1.245.588 1.816l-4.386 3.2c-.35.256-.497.709-.363 1.123l1.675 5.177c.3.925-.755 1.694-1.538 1.123l-4.386-3.2a1 1 0 0 0-1.176 0l-4.386 3.2c-.783.571-1.837-.198-1.538-1.123l1.675-5.177A1.01 1.01 0 0 0 4.8 12.08l-4.386-3.2c-.783-.571-.38-1.816.588-1.816h5.421a1 1 0 0 0 .95-.694z"></path></svg>
```

**Arrow SVG** (appears twice inside `btn-icon` span):
```html
<svg class="svg-icon" viewBox="0 0 16 14" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M9.3 0L7.9 1.4L12.2 5.7H0V7.7H12.2L7.9 12L9.3 13.4L16 6.7L9.3 0Z" fill="currentColor"></path></svg>
```

**Rating display:** if `overall_rating.value` is `"N/A"`, render `"0"`; if the value
is an integer (e.g. `"4"`), render it as `"4.0"`.

**`rel` attribute on the link:** `rel="nofollow"` when `isNofollow` is true;
`rel=""` (empty string) when false.

**Platform items** — one per entry in `external_ratings`, including entries with empty
`rating`/`url`. Capitalize only the first letter of `platform` (`g2` → `G2`,
`trustpilot` → `Trustpilot`, `capterra` → `Capterra`):

```html
<div class="competitor-review-rating-card-rating-list-item"><div>{PlatformLabel}:</div><div><span>{rating}</span>{star_svg}</div></div>
```

Close the block with `<!-- /wp:competitor-review-blocks/rating-card -->`.

## Example output (Sender, review ID 23555)

```html
<!-- wp:paragraph -->
<p>Sender is an email marketing platform…</p>
<!-- /wp:paragraph -->

<!-- wp:competitor-review-blocks/rating-card {"card":{"reviewId":"23555","reviewData":{"id":23555,"slug":"sender","review_data":{"pros_cons":{"brand_logo":"https://www.sender.net/wp-content/uploads/2025/05/Sender-review.png","brand_name":"Sender","competitor_website_url":"https://www.sender.net","overall_rating":{"value":"4.8","external_ratings":[{"platform":"g2","rating":"4.8","url":"https://www.g2.com/products/sender-net/reviews/","new_tab":true,"nofollow":true},{"platform":"trustpilot","rating":"4.8","url":"https://www.trustpilot.com/review/sender.net","new_tab":true,"nofollow":true},{"platform":"capterra","rating":"4.7","url":"https://www.capterra.com/p/139770/SENDER/","new_tab":true,"nofollow":true}]}}}},"link":"https://www.sender.net","linkText":"Visit Sender","target":"_blank","isNofollow":false}} -->
<div class="wp-block-competitor-review-blocks-rating-card competitor-review-rating-card"><div class="competitor-review-rating-card-logo"><img src="https://www.sender.net/wp-content/uploads/2025/05/Sender-review.png" alt="Sender logo"/></div><div class="competitor-review-rating-card-rating"><div class="competitor-review-rating-card-rating-overall-container"><div class="competitor-review-rating-card-rating-header">Overall rating:</div><div class="competitor-review-rating-card-rating-overall"><div><div>4.8</div><div> /5</div></div><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" class="competitor-review-rating-card-rating-star"><path fill="#F75B11" d="M9.05 1.194c.298-.925 1.601-.925 1.9 0l1.676 5.177a1 1 0 0 0 .95.694h5.422c.969 0 1.371 1.245.588 1.816l-4.386 3.2c-.35.256-.497.709-.363 1.123l1.675 5.177c.3.925-.755 1.694-1.538 1.123l-4.386-3.2a1 1 0 0 0-1.176 0l-4.386 3.2c-.783.571-1.837-.198-1.538-1.123l1.675-5.177A1.01 1.01 0 0 0 4.8 12.08l-4.386-3.2c-.783-.571-.38-1.816.588-1.816h5.421a1 1 0 0 0 .95-.694z"></path></svg></div></div><div class="competitor-review-rating-card-rating-list"><div class="competitor-review-rating-card-rating-list-item"><div>G2:</div><div><span>4.8</span><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" class="competitor-review-rating-card-rating-star"><path fill="#F75B11" d="M9.05 1.194c.298-.925 1.601-.925 1.9 0l1.676 5.177a1 1 0 0 0 .95.694h5.422c.969 0 1.371 1.245.588 1.816l-4.386 3.2c-.35.256-.497.709-.363 1.123l1.675 5.177c.3.925-.755 1.694-1.538 1.123l-4.386-3.2a1 1 0 0 0-1.176 0l-4.386 3.2c-.783.571-1.837-.198-1.538-1.123l1.675-5.177A1.01 1.01 0 0 0 4.8 12.08l-4.386-3.2c-.783-.571-.38-1.816.588-1.816h5.421a1 1 0 0 0 .95-.694z"></path></svg></div></div><div class="competitor-review-rating-card-rating-list-item"><div>Trustpilot:</div><div><span>4.8</span><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" class="competitor-review-rating-card-rating-star"><path fill="#F75B11" d="M9.05 1.194c.298-.925 1.601-.925 1.9 0l1.676 5.177a1 1 0 0 0 .95.694h5.422c.969 0 1.371 1.245.588 1.816l-4.386 3.2c-.35.256-.497.709-.363 1.123l1.675 5.177c.3.925-.755 1.694-1.538 1.123l-4.386-3.2a1 1 0 0 0-1.176 0l-4.386 3.2c-.783.571-1.837-.198-1.538-1.123l1.675-5.177A1.01 1.01 0 0 0 4.8 12.08l-4.386-3.2c-.783-.571-.38-1.816.588-1.816h5.421a1 1 0 0 0 .95-.694z"></path></svg></div></div><div class="competitor-review-rating-card-rating-list-item"><div>Capterra:</div><div><span>4.7</span><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20" class="competitor-review-rating-card-rating-star"><path fill="#F75B11" d="M9.05 1.194c.298-.925 1.601-.925 1.9 0l1.676 5.177a1 1 0 0 0 .95.694h5.422c.969 0 1.371 1.245.588 1.816l-4.386 3.2c-.35.256-.497.709-.363 1.123l1.675 5.177c.3.925-.755 1.694-1.538 1.123l-4.386-3.2a1 1 0 0 0-1.176 0l-4.386 3.2c-.783.571-1.837-.198-1.538-1.123l1.675-5.177A1.01 1.01 0 0 0 4.8 12.08l-4.386-3.2c-.783-.571-.38-1.816.588-1.816h5.421a1 1 0 0 0 .95-.694z"></path></svg></div></div></div><div class="competitor-review-rating-card-rating-link"><a href="https://www.sender.net" target="_blank" rel="" class="btn btn-lg nacked-btn">Visit Sender<span class="btn-icon"><svg class="svg-icon" viewBox="0 0 16 14" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M9.3 0L7.9 1.4L12.2 5.7H0V7.7H12.2L7.9 12L9.3 13.4L16 6.7L9.3 0Z" fill="currentColor"></path></svg><svg class="svg-icon" viewBox="0 0 16 14" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M9.3 0L7.9 1.4L12.2 5.7H0V7.7H12.2L7.9 12L9.3 13.4L16 6.7L9.3 0Z" fill="currentColor"></path></svg></span></a></div></div></div>
<!-- /wp:competitor-review-blocks/rating-card -->
```
