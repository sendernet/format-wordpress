# Migration banner (Alternative Solutions posts only)

Apply this rule **only to Alternative Solutions posts** — posts that focus on a
specific main tool and compare alternatives to it (e.g. "Best Mailchimp
Alternatives"). Do **not** apply to Software Overview posts, which review tools
without a single main subject.

Insert exactly **one** migration banner per post. If the post already contains a
migration banner block, leave it exactly as-is.

## Identify the main tool

The main tool is the product being migrated away from. Find it from:
1. The post title — "Best **Resend** Alternatives", "Top **Mailchimp** Alternatives", etc.
2. The intro paragraph if the title is ambiguous.

## Where to place it

Scan the post for content that mentions migration, switching, or moving from the
main tool (e.g. "migrate", "switch from", "move from", "import your"). Place the
banner immediately after the paragraph containing that language.

If no such paragraph exists, place it after the first full tool section (after the
first tool's pros/cons or rating card block, before the separator to the next tool).

## The block

Insert this block with `{ToolName}` replaced by the main tool's name:

```html
<!-- wp:html {"metadata":{"categories":[],"patternName":"core/block/29390","name":"Migration Banner"}} -->
<div class="tw:flex tw:flex-col tw:items-center tw:bg-[#3b3962] tw:border-[3px] tw:border-secondary tw:shadow-[8px_8px_0px_0px_#0A083B] tw:rounded-[40px] tw:py-7.5 tw:px-5 tw:md:p-10 tw:md:pb-7.5 tw:my-20 tw:mr-2 tw:first:mt-0 tw:last:mb-0">
                        <div class=" tw:contents tw:md:flex tw:md:mb-5 tw:items-center tw:gap-7.5">
                            <picture class=" tw:mb-5 tw:md:mb-0 tw:w-40 tw:h-32.5 tw:shrink-0">
                                <source srcset="
                            /assets/compressed-images/migration-banner.webp 1x,
                            /assets/compressed-images/migration-banner@2x.webp 2x
                            " type="image/webp">
                                <source srcset="
                            /assets/compressed-images/migration-banner.png 1x,
                            /assets/compressed-images/migration-banner@2x.png 2x
                            " type="image/png">
                                <img loading="lazy" decoding="async" src="/assets/compressed-images/migration-banner.png" srcset="/assets/compressed-images/migration-banner@2x.png 2x" alt="Description" width="160" height="130">
                            </picture>
                            <div class="tw:contents tw:md:block ">
                                <div class="tw:mb-3 tw:text-white tw:text-2xl tw:md:text-3xl tw:leading-tight tw:font-semibold tw:text-center tw:md:text-left">
                                    Ready to move from {ToolName}?
                                </div>
                                <div class="tw:text-white tw:font-base tw:md:text-xl tw:leading-relaxed tw:mb-5 tw:md:mb-0 tw:text-center tw:md:text-left">
                                    Let us handle the heavy lifting. Effortless migration from any provider to Sender with a dedicated
                                    support team.
                                </div>
                            </div>
                        </div>
                        <a href="/migration/?utm_source=blog&utm_medium=alternatives&utm_campaign=migration_banner" class="btn btn-primary btn-rounded tw:mb-3 tw:w-full tw:md:w-auto">Begin
                            Migration</a>
                        <div class="tw:text-white tw:text-center">
                            We're here to assist 24/7,<br class="tw:block tw:sm:hidden"> even on the free plan.
                        </div>
                    </div>
<!-- /wp:html -->
```

Everything in the block is fixed except `{ToolName}` in the heading. Do not change
any URLs, classes, image paths, or other text.
