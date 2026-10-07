---
title: Troubleshooting
nav_order: 8
permalink: /troubleshooting/
---

# Troubleshooting

### A product block shows nothing

- **Cause**: the metafield is empty, its definition has no storefront access, or the metaobject entry it points to is not *Active*.
- **Fix**: fill in the field on the product; in *Settings → Custom data*, open the definition and turn on storefront access; set the metaobject entry to *Active*. In the theme editor, a running block without data shows a notice instead of staying blank.

### A template is missing from the *Theme template* field

- **Cause**: Shopify lists a theme's alternate templates only when the theme is published.
- **Fix**: publish AERUN, then choose the template on the product or page.

### Running filters don't appear on collections

- **Cause**: filters are not created by the theme.
- **Fix**: in the Search & Discovery app, add the product metafield filters listed in the [data guide]({{ '/data-guide/' | relative_url }}), and check that *Enable filtering* is on in the collection section.

### The comparison page is empty

- **Cause**: the comparison is off or has no page.
- **Fix**: create a page with the `compare` template, then turn on *Enable product comparison* and choose that page in *Theme settings → Running data*.

### The FAQ section is empty

- **Cause**: no FAQ entry matches the section's category or handles, or the entries are drafts.
- **Fix**: check the *Category* and *Handles* settings, and set the FAQ entries to *Active*.

### Colors or fonts changed after choosing another style

- **Cause**: *Theme styles* replaces the theme settings with the preset's values.
- **Fix**: this is expected. Your sections and content are kept. Adjust colors in *Theme settings → Colors*.

### Text is hard to read on a section

- **Cause**: the section's color scheme doesn't fit its image, or a scheme color was changed.
- **Fix**: choose another *Color scheme* for the section, raise the image overlay of the Hero, or restore the scheme's text color.

### The free shipping bar doesn't show

- **Cause**: no threshold is set for the shopper's currency.
- **Fix**: add the currency to *Theme settings → Cart → Free shipping thresholds*, for example `EUR:100, CHF:110`.

### Translations are missing in French

- **Cause**: your content (products, pages, metaobject entries) is not translated yet.
- **Fix**: translate it with the Translate & Adapt app. The theme's own texts are already translated.

Still stuck? Contact us from the [support page]({{ '/support/' | relative_url }}).
