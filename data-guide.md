---
title: Data guide
nav_order: 10
permalink: /data-guide/
---

# Data guide

AERUN shows running data: geometry, terrains, size guides, technologies. A theme creates no data in the Shopify admin, so the path is:

1. **Create the definitions**: metaobjects first, then metafields (*Settings → Custom data*).
2. **Fill in and publish the values**: for a metaobject entry, set the status to *Active* and turn on storefront access.
3. **Connect** the theme blocks. The templates included with the theme already read these keys.

Every key is in the `aerun` namespace. Blocks that show a single value (Linked products, Product text, the images of Annotated image and Drop and stack) accept a dynamic source, so they can be connected to an existing field instead. The other blocks combine several fields (unit, basis, scale, family) and read `aerun.*` directly: spec table and summary, profile bars, fit advice, technologies, proofs, nutrition facts, product card, comparison and filters.

Rules:

- An empty field is hidden. In the comparison, it shows "Not specified", never zero.
- Measurements keep the unit you enter. The theme adds the conversion in brackets; a theme setting turns it off.
- For the weight, also fill in its basis (shoe, pair) and the reference size.
- The 1 to 5 scales are editorial: each level has a name, and the scale is the same for every model.
- Adding a field creates no filter. Filters are set up in the Search & Discovery app.
- Store no health or biometric data.

Alternate templates (`product.shoe`, `page.compare`, `page.glossary`…) appear in the *Theme template* field of the admin only when AERUN is the published theme.

This guide is also available [in French]({{ '/fr/data-guide/' | relative_url }}).

## Metaobjects

Create them first: the reference metafields depend on them. For each one, turn on storefront access and translations.

### AERUN discipline (`aerun_discipline`)

How a product is used. Its family drives the product card and the comparison rows.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `family` | Family | `single_line_text_field` | choices: `shoe`, `apparel`, `hydration`, `nutrition`, `accessory`; required; Comparison family: shoe (footwear), apparel (clothing), hydration, nutrition, accessory. |

### AERUN terrain (`aerun_terrain`)

Terrain recommended by the brand (RUN-02). Can be used as a filter in Search & Discovery.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `icon` | Icon | `single_line_text_field` | choices: `road`, `track`, `trail`, `technical`; required; Icon key, independent of language. |
| `description` | Description | `multi_line_text_field` |  |

### AERUN usage (`aerun_usage`)

Training or racing usage (RUN-01, RUN-17). Can be used as a filter in Search & Discovery.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `description` | Description | `multi_line_text_field` |  |

### AERUN qualitative criterion (`aerun_criterion`)

Editorial scale with five named levels (SPC-05, SPC-06). The handle must match the product metafield key: cushioning, stability, flexibility or responsiveness.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `levels` | Levels (5, lowest to highest) | `list.single_line_text_field` | 5 values; required |
| `definition` | Definition | `multi_line_text_field` |  |
| `method` | Method and source | `multi_line_text_field` | How the level is assigned. Never a claimed scientific score. |

### AERUN technology (`aerun_technology`)

Material or technology shared across products (SPC-07, MED-09).

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `benefit` | Stated benefit | `multi_line_text_field` | required |
| `image` | Image | `file_reference` |  |
| `source` | Source | `single_line_text_field` |  |
| `link` | Link | `url` |  |

### AERUN proof (`aerun_proof`)

Supporting document: technical sheet, test, certificate (SPC-12).

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `organization` | Organization | `single_line_text_field` |  |
| `scope` | Scope | `multi_line_text_field` |  |
| `date` | Date | `date` |  |
| `file` | File | `file_reference` |  |
| `link` | Link | `url` |  |

### AERUN glossary term (`aerun_glossary_term`)

Definition of a technical term (SPC-08, CHO-07). The handle of a term that explains a spec is that spec's key, with hyphens instead of underscores (drop, stack, plate, lug-depth…).

Display name: `term`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `term` | Term | `single_line_text_field` | required |
| `definition` | Definition | `multi_line_text_field` | required |
| `image` | Illustration | `file_reference` |  |

### AERUN FAQ (`aerun_faq`)

Reusable question and answer: landing page FAQ, help center, product page (HOM-19, FRM-04, TRU-10, PIN-11). The category groups and filters the questions.

Display name: `question`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `question` | Question | `single_line_text_field` | required |
| `answer` | Answer | `multi_line_text_field` | required |
| `category` | Category | `single_line_text_field` | Free text, for example Shipping, Returns, Sizing, Care. |

### AERUN person (`aerun_person`)

Athlete, ambassador, author or expert (EDT-09, EDT-17, TRU-08, FRM-09). Each person has their own page: /pages/people/<handle>.

Display name: `name`.

Web pages: turn on *Web pages* (Online Store) with the URL prefix `people`. The theme renders them with `templates/metaobject/aerun_person.json`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `role` | Role | `single_line_text_field` | Ambassador, coach, editor… |
| `discipline` | Discipline | `single_line_text_field` |  |
| `portrait` | Portrait | `file_reference` |  |
| `bio` | Biography | `multi_line_text_field` |  |
| `quote` | Quote | `multi_line_text_field` |  |
| `credentials` | Credentials | `multi_line_text_field` | Real, verifiable credentials only. |
| `link` | Link | `url` | Website, social network or external page. |

### AERUN store location (`aerun_store`)

Store or retailer listed by the brand (FRM-10, FRM-11, TPL-27). Separate from Shopify local pickup.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `city` | City | `single_line_text_field` | required |
| `country` | Country | `single_line_text_field` | required |
| `address` | Address | `multi_line_text_field` | required |
| `hours` | Opening hours | `multi_line_text_field` |  |
| `phone` | Phone | `single_line_text_field` |  |
| `email` | Email | `single_line_text_field` |  |
| `image` | Photo | `file_reference` |  |
| `directions` | Directions link | `url` | Optional. Otherwise, a Google Maps search on the address. |

### AERUN event (`aerun_event`)

Community run, race or meetup (FRM-13, TPL-26). Each event has its own page: /pages/event/<handle>. Registration happens on an external service.

Display name: `name`.

Web pages: turn on *Web pages* (Online Store) with the URL prefix `event`. The theme renders them with `templates/metaobject/aerun_event.json`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `starts_at` | Start date and time | `date_time` | required |
| `timezone` | Time zone | `single_line_text_field` | Shown next to the time, for example Europe/Paris. |
| `location` | Location | `single_line_text_field` | required |
| `description` | Description | `multi_line_text_field` |  |
| `program` | Program | `multi_line_text_field` |  |
| `registration` | Registration link | `url` |  |
| `image` | Image | `file_reference` |  |

### AERUN testimonial (`aerun_testimonial`)

Feedback selected and published by the brand (TRU-02). Never presented as a verified purchase review.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `quote` | Quote | `multi_line_text_field` | required |
| `name` | Name | `single_line_text_field` | required |
| `role` | Role or discipline | `single_line_text_field` |  |
| `portrait` | Portrait | `file_reference` |  |
| `product` | Related product | `product_reference` |  |
| `source` | Source | `single_line_text_field` | Where the testimonial was collected, for example Interview, March 2026. |

### AERUN size guide (`aerun_size_guide`)

Size table for a brand or a model (RUN-14, RUN-15, SPT-01, SPT-08). Never a universal conversion.

Display name: `name`.

| Field | Name | Type | Details |
| --- | --- | --- | --- |
| `name` | Name | `single_line_text_field` | required |
| `category` | Category | `single_line_text_field` | choices: `shoe`, `apparel`, `socks`; required |
| `table` | Table | `json` | format `size_table`; required |
| `method` | How to measure | `rich_text_field` |  |
| `image` | Measurement illustration | `file_reference` |  |
| `note` | Note | `multi_line_text_field` |  |

## Metafields

### Identity

Used by: Product card, use and terrain profile, filters, comparison.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.discipline` | Product | Discipline | `metaobject_reference` | metaobject `aerun_discipline`; Replaces the type on the product card; its family selects the comparison rows. |
| `aerun.terrain` | Product | Terrains | `list.metaobject_reference` | metaobject `aerun_terrain`; Search & Discovery filter; The first terrain is the main terrain. |
| `aerun.usage` | Product | Usages | `list.metaobject_reference` | metaobject `aerun_usage`; Search & Discovery filter; The first usage is the main usage. |
| `aerun.distance` | Product | Distances | `list.single_line_text_field` | choices: `short`, `medium`, `long`, `ultra`; Distances stated by the brand, with no individual guarantee. |

### Geometry and weight

Used by: Drop and stack block, spec table, comparison.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.drop` | Product | Drop | `dimension` | unit mm; Takes precedence over the heel stack − forefoot stack calculation. |
| `aerun.stack_heel` | Product | Heel stack | `dimension` | unit mm |
| `aerun.stack_forefoot` | Product | Forefoot stack | `dimension` | unit mm |
| `aerun.weight` | Product | Weight | `weight` | unit g; Reference weight; specify the basis (shoe or pair) and the size. |
| `aerun.weight_basis` | Product | Weight basis | `single_line_text_field` | choices: `shoe`, `pair`, `item`; shoe: one shoe; pair: the pair; item: the item. |
| `aerun.reference_size` | Product | Reference size | `single_line_text_field` | Size at which weight and geometry were measured, for example "42 EU". |
| `aerun.measurement_note` | Product | Measurement conditions | `multi_line_text_field` | Protocol, date and source of the measurements. |
| `aerun.weight` | Variant | Variant weight | `weight` | unit g; Weight specific to the size. Otherwise, the product weight is shown with its reference size. |

### Feel (1 to 5 scales)

Used by: Profile bars block. Each value is read on the scale of the criterion with the same handle.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.cushioning` | Product | Cushioning (1 to 5) | `number_integer` | 1 to 5 |
| `aerun.stability` | Product | Stability (1 to 5) | `number_integer` | 1 to 5 |
| `aerun.flexibility` | Product | Flexibility (1 to 5) | `number_integer` | 1 to 5 |
| `aerun.responsiveness` | Product | Responsiveness (1 to 5) | `number_integer` | 1 to 5 |

### Construction

Used by: Spec table (construction group) and the trail product page.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.plate` | Product | Plate | `single_line_text_field` | choices: `none`, `nylon`, `tpu`, `carbon`, `other` |
| `aerun.plate_note` | Product | Plate construction | `multi_line_text_field` |  |
| `aerun.stability_note` | Product | Stability construction | `multi_line_text_field` | Base, geometry, reinforcements. No gait correction is promised. |
| `aerun.midsole` | Product | Midsole | `multi_line_text_field` |  |
| `aerun.outsole` | Product | Outsole | `multi_line_text_field` |  |
| `aerun.lug_depth` | Product | Lug depth | `dimension` | unit mm |
| `aerun.upper` | Product | Upper | `multi_line_text_field` |  |
| `aerun.weather` | Product | Weather protection | `multi_line_text_field` | Membrane, water repellency or waterproofing claimed, with its source. |
| `aerun.trail_protection` | Product | Trail protection | `multi_line_text_field` | Rock plate, protective plate, drainage, hold. |

### Fit and sizing

Used by: Fit advice block and the size guide link of the variant picker.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.fit` | Product | Fit | `single_line_text_field` | choices: `snug`, `true`, `roomy`; snug: runs small; true: true to size; roomy: runs large. |
| `aerun.fit_note` | Product | Fit profile | `multi_line_text_field` | Forefoot, heel, width, volume; source of the advice. |
| `aerun.size_guide` | Product | Size guide | `metaobject_reference` | metaobject `aerun_size_guide`; Takes precedence over the page chosen in the Variant picker block. |

### Shared references

Used by: Technologies and Proofs blocks.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.technologies` | Product | Technologies | `list.metaobject_reference` | metaobject `aerun_technology` |
| `aerun.proofs` | Product | Proofs | `list.metaobject_reference` | metaobject `aerun_proof` |
| `aerun.faq` | Product | Product FAQ | `list.metaobject_reference` | metaobject `aerun_faq`; Questions specific to the model, shown by the FAQ section. |

### Linked products

Used by: Linked products blocks (versions, rotation, compatible products).

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.siblings` | Product | Sibling products | `list.product_reference` | Other colors or versions sold as separate product pages. Include the product itself to set the order. |
| `aerun.sibling_label` | Product | Sibling label | `single_line_text_field` | Short name shown under the thumbnail: color or version. |
| `aerun.rotation` | Product | Recommended rotation | `list.product_reference` | Shoes to pair with it, hand-picked. |
| `aerun.compatible_products` | Product | Compatibility | `list.product_reference` | Tested pairings. No compatibility is inferred. |

### Apparel

Used by: Apparel product template: fit, model, materials, pockets, care.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.composition` | Product | Composition | `multi_line_text_field` |  |
| `aerun.apparel_fit` | Product | Fit | `single_line_text_field` | choices: `slim`, `regular`, `relaxed` |
| `aerun.model_height` | Product | Model height | `dimension` | unit cm |
| `aerun.model_size_worn` | Product | Size worn | `single_line_text_field` |  |
| `aerun.model_note` | Product | Model note | `single_line_text_field` | Relevant measurement, for example chest circumference. |
| `aerun.ventilation` | Product | Ventilation | `multi_line_text_field` |  |
| `aerun.protection` | Product | Protection | `multi_line_text_field` | Wind resistance, insulation, water repellency: stated properties and conditions. |
| `aerun.upf` | Product | UPF rating | `number_integer` | 1 to 100 |
| `aerun.pockets` | Product | Pockets and storage | `multi_line_text_field` |  |
| `aerun.reflective` | Product | Reflective details | `multi_line_text_field` | Never presented as a safety certification. |
| `aerun.care` | Product | Care | `multi_line_text_field` |  |

### Hydration and accessories

Used by: Hydration product template: capacity, dimensions, contents, instructions.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.capacity` | Product | Capacity | `volume` | unit ml |
| `aerun.dimensions` | Product | Dimensions | `single_line_text_field` | Manufacturer dimensions with their unit, for example "22 × 7 cm". |
| `aerun.included` | Product | What's in the box | `list.single_line_text_field` |  |
| `aerun.instructions` | Product | Instructions for use | `rich_text_field` | Instructions already validated by the manufacturer. No custom calculation. |

### Nutrition

Used by: Nutrition product template: format, nutrition facts, ingredients.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.net_weight` | Product | Net weight | `weight` | unit g |
| `aerun.serving_size` | Product | Serving size | `weight` | unit g |
| `aerun.servings` | Product | Number of servings | `number_integer` | at least 1 |
| `aerun.nutrition_table` | Product | Nutrition table | `json` | format `nutrition_table` |
| `aerun.ingredients` | Product | Ingredients | `multi_line_text_field` |  |
| `aerun.allergens` | Product | Allergens | `multi_line_text_field` |  |

### Merchandising

Used by: New badge of product cards, Linked products blocks (alternatives, bundle).

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.release_date` | Product | Release date | `date` | Editorial release date. Drives the New badge (otherwise: publication date). |
| `aerun.alternatives` | Product | Range alternatives | `list.product_reference` | Other tiers of the range, with the price and spec differences. |
| `aerun.bundle_components` | Product | Bundle contents | `list.product_reference` | Products in a bundle created with a bundles app, if the app does not already expose them. |

### Product information

Used by: Benefits, Care symbols and Manuals blocks of the product page.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.benefits` | Product | Benefits | `list.single_line_text_field` | Three to five short sentences, in display order. |
| `aerun.care_symbols` | Product | Care symbols | `list.single_line_text_field` | choices: `wash-30`, `wash-40`, `hand-wash`, `no-tumble-dry`, `no-bleach`, `iron-low`, `no-iron`, `no-dry-clean`, `dry-flat` |
| `aerun.manuals` | Product | Manuals and documents | `list.file_reference` | PDF manual or technical sheet. The file's alt text is used as the title. |

### Articles

Used by: Article template: author, featured products, related articles, reading time.

| Key | On | Name | Type | Details |
| --- | --- | --- | --- | --- |
| `aerun.author` | Article | Author | `metaobject_reference` | metaobject `aerun_person`; Replaces the Shopify author name with their profile (portrait, biography, link). |
| `aerun.products` | Article | Featured products | `list.product_reference` |  |
| `aerun.related_articles` | Article | Related articles | `list.single_line_text_field` | Handles in the form blog/article, for example news/choisir-sa-chaussure-de-trail. Otherwise: articles with the same tag, then the most recent. |
| `aerun.reading_time` | Article | Reading time (minutes) | `number_integer` | 1 to 120; Replaces the calculated estimate (words ÷ 200). |

## JSON formats

### `size_table`

Size table: column headers, then one row per size. Cells are text, to keep the exact source value.

```json
{
  "columns": [
    "EU",
    "US",
    "UK",
    "cm"
  ],
  "rows": [
    [
      "42",
      "8.5",
      "8",
      "26.5"
    ]
  ]
}
```

### `nutrition_table`

Manufacturer nutrition table: reference columns (per 100 g, per serving), then one row per nutrient with its unit.

```json
{
  "columns": [
    "Pour 100 g",
    "Par portion (40 g)"
  ],
  "rows": [
    {
      "label": "Énergie",
      "unit": "kcal",
      "values": [
        "260",
        "104"
      ]
    },
    {
      "label": "Glucides",
      "unit": "g",
      "values": [
        "64",
        "25.6"
      ]
    }
  ]
}
```

## Search & Discovery filters

In the Search & Discovery app, add these product metafield filters. The theme shows the terrain pictogram next to its name.

- `aerun.terrain` (Terrains)
- `aerun.usage` (Usages)
- Width is a variant option ("Width"): use the native option filter.
