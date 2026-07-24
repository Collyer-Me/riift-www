# Theme deploy checklist

**Rule:** Git owns code. The live Shopify theme owns content.

Content files are listed in `.shopifyignore` and must not be pushed over live from a stale local copy.

## Day-to-day (safe)

1. Create a feature branch and make code changes (`.liquid`, CSS/JS, assets, schemas).
2. Preview on a **development theme** (not the published theme):

   ```bash
   shopify theme dev
   ```

   Or push to an unpublished theme:

   ```bash
   shopify theme push --development
   ```

3. Review on the preview URL.
4. Merge to `main` when ready.

## Deploy code to live

1. Confirm you are deploying **code only** (`.shopifyignore` is in place).
2. Pull latest content from live first if you need a fresh local mirror:

   ```bash
   shopify theme pull --live
   ```

3. Push to the live theme (content files stay ignored):

   ```bash
   shopify theme push --live
   ```

   Prefer selecting the live theme interactively if you are unsure:

   ```bash
   shopify theme push
   ```

4. Hard-refresh the storefront and spot-check header, homepage, and a product page.

## When you intentionally need to ship a JSON template / section group

1. `shopify theme pull --live` so local content matches Shopify.
2. Make your template/section-group change on that fresh copy.
3. Push **only** that file, for example:

   ```bash
   shopify theme push --live --only templates/page.about.json
   ```

4. Do not push a full theme including stale `settings_data.json`.

## Never do this

- Connect the **published** theme to GitHub bidirectional sync for daily work.
- `shopify theme push --live` of the whole theme without `.shopifyignore`.
- Commit/push old `config/settings_data.json` or template JSON over live after someone edited in admin.
