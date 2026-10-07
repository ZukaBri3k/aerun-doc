---
title: Products and running data
nav_order: 5
permalink: /products-and-data/
---

# Products and running data

## How it works

AERUN's product page reads running data from **metafields** and **metaobjects** in the `aerun` namespace. A theme cannot create data, so you set it up once:

1. **Create the definitions** in *Settings → Custom data*: metaobjects first (disciplines, terrains, technologies, size guides…), then product and variant metafields. Turn on storefront access for each one.
2. **Fill in the values** on each product, and set each metaobject entry to *Active*.
3. **Use the product templates**: `shoe`, `apparel`, `hydration` and `nutrition` already read these fields. Choose the template in the *Theme template* field of the product.

The [data guide]({{ '/data-guide/' | relative_url }}) lists every field: key, type, unit, choices and which block uses it.

An empty field is simply hidden. You can start with a few fields and add more later.

## Product blocks and their data

| Block | Shows | Main fields |
| --- | --- | --- |
| Spec summary | 3 to 6 key specs as tiles | drop, weight, cushioning, terrain… |
| Spec table | Label and value for the fields you list | any `aerun` field |
| Drop and stack | The sole profile with heel and forefoot stack | `stack_heel`, `stack_forefoot`, `drop` |
| Profile bars | Cushioning, stability, flexibility, responsiveness on a 1 to 5 scale | feel fields and criteria |
| Use profile | Terrains, uses and distances | `terrain`, `usage`, `distance` |
| Fit advice | Runs small or large, fit notes, size guide | `fit`, `fit_note`, `size_guide` |
| Technologies | Technology cards | `technologies` |
| Proofs | Technical sheets and test results | `proofs` |
| Nutrition facts | The maker's nutrition table | `nutrition_table` |
| Care symbols | Care pictograms with their text | `care_symbols`, `care` |
| Manuals | Downloadable documents | `manuals` |
| Benefits | 3 to 5 short benefit lines | `benefits` |
| Linked products | Versions, rotation, compatible products, alternatives, what's in the box | product reference lists |
| Review summary | Rating and review count from your reviews app | `reviews.rating`, `reviews.rating_count` |

Measurements are shown in the unit you entered, with a conversion in brackets. Turn conversions off in *Theme settings → Running data*.

## Product cards

*Theme settings → Product cards* chooses up to three specs shown on every card (*Spec 1*, *Spec 2*, *Spec 3*), the second image on hover, quick add, color swatches and how long the "New" badge stays after a product's release date.

## Filters

Running filters (terrain, use, distance, drop…) are set in the **Search & Discovery** app: add the product metafield filters listed at the end of the [data guide]({{ '/data-guide/' | relative_url }}). The collection page shows them with the native Shopify filters.

## Comparison

1. Create a page with the `compare` template.
2. In *Theme settings → Running data*, turn on *Enable product comparison* and choose that page as the *Comparison page*.

Shoppers add up to four products with the *Compare* button on cards and product pages. The comparison shows the rows that fit the product family (shoes, apparel, hydration, nutrition). A missing value shows "Not specified", never zero.

## Shoe finder

The `shoe-finder` page template holds the *Choice guide* section: a few questions written by you, each answer pointing to a collection or a filtered view. It runs entirely in the theme, with no app.

## Reviews

AERUN does not collect reviews. Install a reviews app, then add its app block in the *Reviews* section of the product templates. The *Review summary* block near the title links to it.
