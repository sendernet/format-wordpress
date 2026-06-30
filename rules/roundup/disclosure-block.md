# Disclosure block


Insert this disclosure block, on its own line, immediately after the intro
(i.e. directly before the first heading block, `<!-- wp:heading -->`):

```html
<!-- wp:block {"ref":28704} /-->
```

- The intro is everything from the start of the post up to the first heading.
- Insert the block once, between the last intro block and the first heading block.
- If a `<!-- wp:block {"ref":28704} /-->` is already present, don't add a second one.
- If the post has no heading, place it after the first paragraph block.

Example — before:

```html
<!-- wp:paragraph -->
<p>
  Choosing the right tool comes down to your workflow. Here are the options.
</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading">The Alternatives</h2>
<!-- /wp:heading -->
```

After:

```html
<!-- wp:paragraph -->
<p>
  Choosing the right tool comes down to your workflow. Here are the options.
</p>
<!-- /wp:paragraph -->

<!-- wp:block {"ref":28704} /-->

<!-- wp:heading -->
<h2 class="wp-block-heading">The Alternatives</h2>
<!-- /wp:heading -->
```
