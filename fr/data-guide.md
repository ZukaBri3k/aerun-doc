---
title: Guide des données (français)
nav_order: 12
permalink: /fr/data-guide/
nav_exclude: true
---
# Guide des données AERUN

Le thème AERUN affiche des données running : géométrie, terrains, guides de tailles, technologies. Un thème ne crée aucune donnée dans l’admin. Le parcours est le suivant :

1. **Créer les définitions** : metaobjects d’abord, puis metafields (Paramètres → Données personnalisées).
2. **Renseigner et publier les valeurs** : pour un metaobject, statut « Actif » et accès Storefront activé.
3. **Connecter** les blocs du thème. Les templates livrés lisent déjà ces clés.

Toutes les clés sont dans le namespace `aerun`. Ce n’est pas une obligation pour les blocs à valeur unique : Produits liés, Texte produit et les images d’Image annotée et de Drop et stack acceptent une source dynamique, qu’on peut rebrancher sur un champ existant. Les autres blocs combinent plusieurs champs (unité, base, échelle, famille) et lisent `aerun.*` directement : tableau et résumé de specs, profils, conseil de taille, technologies, preuves, nutrition, carte produit, comparaison et facettes.

Règles :

- Un champ vide est masqué. En comparaison, il s’affiche « Non renseigné », et jamais zéro.
- Les mesures gardent l’unité saisie. Le thème ajoute la conversion entre parenthèses, et un réglage du thème permet de la couper.
- Pour le poids, renseigner aussi la base (chaussure, paire) et la taille de référence.
- Les échelles 1 à 5 sont éditoriales : chaque niveau porte un nom, et l’échelle est la même pour tous les modèles.
- Ajouter un champ ne crée pas de filtre. Les filtres se configurent dans l’app Search & Discovery.
- Ne stocker aucune donnée de santé ou biométrique.

Sur la boutique de démonstration, `node docs/demo/setup-aerun.mjs --store <boutique>.myshopify.com` crée les définitions, les metaobjects d’exemple et les valeurs des produits de démo. C’est un outil de démonstration, pas une installation du thème. Il faut d’abord autoriser la CLI avec les droits sur les metaobjects, sans quoi Shopify refuse chaque type comme « réservé à une autre application » :

```sh
shopify store auth --store <boutique>.myshopify.com --scopes read_products,write_products,read_metaobject_definitions,write_metaobject_definitions,read_metaobjects,write_metaobjects
```

Les modèles du thème (`product.shoe`, `page.compare`, `page.glossary`…) n’apparaissent dans le champ « Modèle de thème » de l’admin que si AERUN est le thème publié. Pour tester AERUN en thème de développement ou non publié, on attribue le modèle par l’API (`templateSuffix` du produit ou de la page) ; le script de démo le fait pour les produits de démo.

## Metaobjects

Les créer en premier : les metafields de référence en dépendent. Pour chacun, activer l’accès Storefront et la traduction.

### Discipline AERUN (`aerun_discipline`)

Pratique d'un produit. Sa famille pilote la carte produit et les lignes de comparaison.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `family` | Famille | `single_line_text_field` | choix : `shoe`, `apparel`, `hydration`, `nutrition`, `accessory` ; obligatoire ; Famille de comparaison : shoe (chaussures), apparel (textile), hydration, nutrition, accessory. |

Exemples : `road`, `trail`, `recovery`, `apparel`, `hydration`, `nutrition`, `accessory`.

### Terrain AERUN (`aerun_terrain`)

Terrain conseillé par la marque (RUN-02). Filtrable dans Search & Discovery.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `icon` | Pictogramme | `single_line_text_field` | choix : `road`, `track`, `trail`, `technical` ; obligatoire ; Clé du pictogramme, indépendante de la langue. |
| `description` | Description | `multi_line_text_field` |  |

Exemples : `road`, `track`, `trail`, `technical`.

### Usage AERUN (`aerun_usage`)

Usage d'entraînement ou de course (RUN-01, RUN-17). Filtrable dans Search & Discovery.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `description` | Description | `multi_line_text_field` |  |

Exemples : `daily`, `tempo`, `race`, `long-run`.

### Critère qualitatif AERUN (`aerun_criterion`)

Échelle éditoriale à cinq niveaux nommés (SPC-05, SPC-06). Le handle doit être la clé du metafield produit : cushioning, stability, flexibility ou responsiveness.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `levels` | Niveaux (5, du plus bas au plus haut) | `list.single_line_text_field` | 5 valeurs ; obligatoire |
| `definition` | Définition | `multi_line_text_field` |  |
| `method` | Méthode et source | `multi_line_text_field` | Comment le niveau est attribué. Jamais un score scientifique prétendu. |

Exemples : `cushioning`, `stability`, `flexibility`, `responsiveness`.

### Technologie AERUN (`aerun_technology`)

Matériau ou technologie partagée entre produits (SPC-07, MED-09).

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `benefit` | Bénéfice déclaré | `multi_line_text_field` | obligatoire |
| `image` | Image | `file_reference` |  |
| `source` | Source | `single_line_text_field` |  |
| `link` | Lien | `url` |  |

Exemples : `peba-foam`, `carbon-plate`.

### Preuve AERUN (`aerun_proof`)

Document justificatif : fiche technique, essai, certificat (SPC-12).

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `organization` | Organisme | `single_line_text_field` |  |
| `scope` | Périmètre | `multi_line_text_field` |  |
| `date` | Date | `date` |  |
| `file` | Fichier | `file_reference` |  |
| `link` | Lien | `url` |  |

Exemples : `tempo-01-tech-sheet`.

### Terme du glossaire AERUN (`aerun_glossary_term`)

Définition d'un terme technique (SPC-08, CHO-07). Le handle d'un terme qui explique une spec est la clé de cette spec, avec des tirets à la place des soulignés (drop, stack, plate, lug-depth…).

Nom d’affichage : `term`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `term` | Terme | `single_line_text_field` | obligatoire |
| `definition` | Définition | `multi_line_text_field` | obligatoire |
| `image` | Illustration | `file_reference` |  |

Exemples : `drop`, `stack`, `plate`, `lug-depth`.

### Question fréquente AERUN (`aerun_faq`)

Question et réponse réutilisables : FAQ de landing, centre d'aide, fiche produit (HOM-19, FRM-04, TRU-10, PIN-11). La catégorie regroupe et filtre les questions.

Nom d’affichage : `question`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `question` | Question | `single_line_text_field` | obligatoire |
| `answer` | Réponse | `multi_line_text_field` | obligatoire |
| `category` | Catégorie | `single_line_text_field` | Texte libre, par exemple Livraison, Retours, Tailles, Entretien. |

Exemples : `shipping-times`, `returns`, `choosing-size`, `shoe-care`.

### Personne AERUN (`aerun_person`)

Athlète, ambassadeur, auteur ou expert (EDT-09, EDT-17, TRU-08, FRM-09). Chaque personne a sa page : /pages/people/<handle>.

Nom d’affichage : `name`.

Page web : activer « Pages web » (Online Store) avec le préfixe d’URL `people`. Le thème la rend avec `templates/metaobject/aerun_person.json`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `role` | Rôle | `single_line_text_field` | Ambassadeur, coach, rédactrice… |
| `discipline` | Discipline | `single_line_text_field` |  |
| `portrait` | Portrait | `file_reference` |  |
| `bio` | Biographie | `multi_line_text_field` |  |
| `quote` | Citation | `multi_line_text_field` |  |
| `credentials` | Qualifications | `multi_line_text_field` | Uniquement des qualifications réelles et vérifiables. |
| `link` | Lien | `url` | Site, réseau social ou page externe. |

Exemples : `lea-martin`, `hugo-bernard`.

### Point de vente AERUN (`aerun_store`)

Boutique ou revendeur renseigné par la marque (FRM-10, FRM-11, TPL-27). Distinct du retrait en magasin de Shopify.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `city` | Ville | `single_line_text_field` | obligatoire |
| `country` | Pays | `single_line_text_field` | obligatoire |
| `address` | Adresse | `multi_line_text_field` | obligatoire |
| `hours` | Horaires | `multi_line_text_field` |  |
| `phone` | Téléphone | `single_line_text_field` |  |
| `email` | E-mail | `single_line_text_field` |  |
| `image` | Photo | `file_reference` |  |
| `directions` | Lien d'itinéraire | `url` | Facultatif. Sinon, une recherche Google Maps sur l'adresse. |

Exemples : `aerun-paris`, `aerun-lyon`, `trail-shop-annecy`.

### Événement AERUN (`aerun_event`)

Sortie, course ou rendez-vous de la communauté (FRM-13, TPL-26). Chaque événement a sa page : /pages/event/<handle>. L'inscription se fait sur un service externe.

Nom d’affichage : `name`.

Page web : activer « Pages web » (Online Store) avec le préfixe d’URL `event`. Le thème la rend avec `templates/metaobject/aerun_event.json`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `starts_at` | Début | `date_time` | obligatoire |
| `timezone` | Fuseau horaire | `single_line_text_field` | Affiché à côté de l'heure, par exemple Europe/Paris. |
| `location` | Lieu | `single_line_text_field` | obligatoire |
| `description` | Description | `multi_line_text_field` |  |
| `program` | Programme | `multi_line_text_field` |  |
| `registration` | Lien d'inscription | `url` |  |
| `image` | Image | `file_reference` |  |

Exemples : `thursday-club-run`, `ridge-trail`.

### Témoignage AERUN (`aerun_testimonial`)

Retour choisi et publié par la marque (TRU-02). Jamais présenté comme un avis d'achat vérifié.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `quote` | Citation | `multi_line_text_field` | obligatoire |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `role` | Rôle ou discipline | `single_line_text_field` |  |
| `portrait` | Portrait | `file_reference` |  |
| `product` | Produit concerné | `product_reference` |  |
| `source` | Source | `single_line_text_field` | Où le témoignage a été recueilli, par exemple Entretien, mars 2026. |

Exemples : `camille`, `yanis`, `ines`.

### Guide de tailles AERUN (`aerun_size_guide`)

Table de tailles d'une marque ou d'un modèle (RUN-14, RUN-15, SPT-01, SPT-08). Jamais de conversion universelle.

Nom d’affichage : `name`.

| Champ | Nom | Type | Détails |
| --- | --- | --- | --- |
| `name` | Nom | `single_line_text_field` | obligatoire |
| `category` | Catégorie | `single_line_text_field` | choix : `shoe`, `apparel`, `socks` ; obligatoire |
| `table` | Tableau | `json` | forme `size_table` ; obligatoire |
| `method` | Comment mesurer | `rich_text_field` |  |
| `image` | Illustration de mesure | `file_reference` |  |
| `note` | Note | `multi_line_text_field` |  |

Exemples : `aerun-shoes`, `aerun-apparel`.

## Metafields

### Identité

Utilisé par : Carte produit, profil d’usage et de terrain, facettes, comparaison.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.discipline` | Produit | Discipline | `metaobject_reference` | metaobject `aerun_discipline` ; Remplace le type sur la carte produit ; sa famille choisit les lignes de comparaison. | CRD-08, CHO-04 |
| `aerun.terrain` | Produit | Terrains | `list.metaobject_reference` | metaobject `aerun_terrain` ; filtre Search & Discovery ; Le premier terrain est le terrain principal. | RUN-02, COL-18 |
| `aerun.usage` | Produit | Usages | `list.metaobject_reference` | metaobject `aerun_usage` ; filtre Search & Discovery ; Le premier usage est l'usage principal. | RUN-01, COL-18 |
| `aerun.distance` | Produit | Distances | `list.single_line_text_field` | choix : `short`, `medium`, `long`, `ultra` ; Distances annoncées par la marque, sans garantie individuelle. | RUN-03 |

### Géométrie et poids

Utilisé par : Bloc Drop et stack, tableau de specs, comparaison.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.drop` | Produit | Drop | `dimension` | unité mm ; Prime sur le calcul stack talon − stack avant-pied. | RUN-04, SPC-01 |
| `aerun.stack_heel` | Produit | Stack talon | `dimension` | unité mm | RUN-04 |
| `aerun.stack_forefoot` | Produit | Stack avant-pied | `dimension` | unité mm | RUN-04 |
| `aerun.weight` | Produit | Poids | `weight` | unité g ; Poids de référence ; préciser la base (chaussure ou paire) et la taille. | RUN-05, SPC-04 |
| `aerun.weight_basis` | Produit | Base du poids | `single_line_text_field` | choix : `shoe`, `pair`, `item` ; shoe : une chaussure ; pair : la paire ; item : l'article. | RUN-05 |
| `aerun.reference_size` | Produit | Taille de référence | `single_line_text_field` | Taille à laquelle poids et géométrie ont été mesurés, par exemple « 42 EU ». | RUN-05, SPC-10 |
| `aerun.measurement_note` | Produit | Conditions de mesure | `multi_line_text_field` | Protocole, date et source des mesures. | SPC-10 |
| `aerun.weight` | Variante | Poids de la variante | `weight` | unité g ; Poids propre à la taille. Sinon, le poids produit s'affiche avec sa taille de référence. | SPC-11 |

### Sensation (échelles 1 à 5)

Utilisé par : Bloc Profil produit. Chaque valeur est lue sur l’échelle du critère qui porte le même handle.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.cushioning` | Produit | Amorti (1 à 5) | `number_integer` | de 1 à 5 | RUN-06, SPC-06 |
| `aerun.stability` | Produit | Stabilité (1 à 5) | `number_integer` | de 1 à 5 | RUN-07, SPC-06 |
| `aerun.flexibility` | Produit | Souplesse (1 à 5) | `number_integer` | de 1 à 5 | SPC-06 |
| `aerun.responsiveness` | Produit | Dynamisme (1 à 5) | `number_integer` | de 1 à 5 | SPC-06 |

### Construction

Utilisé par : Tableau de specs (groupe Construction) et fiche trail.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.plate` | Produit | Plaque | `single_line_text_field` | choix : `none`, `nylon`, `tpu`, `carbon`, `other` | RUN-08 |
| `aerun.plate_note` | Produit | Construction de la plaque | `multi_line_text_field` |  | RUN-08 |
| `aerun.stability_note` | Produit | Construction de stabilité | `multi_line_text_field` | Base, géométrie, renforts. Aucune correction de foulée promise. | RUN-07 |
| `aerun.midsole` | Produit | Semelle intermédiaire | `multi_line_text_field` |  | RUN-09 |
| `aerun.outsole` | Produit | Semelle externe | `multi_line_text_field` |  | RUN-10 |
| `aerun.lug_depth` | Produit | Profondeur des crampons | `dimension` | unité mm | RUN-10 |
| `aerun.upper` | Produit | Tige | `multi_line_text_field` |  | RUN-11 |
| `aerun.weather` | Produit | Protection météo | `multi_line_text_field` | Membrane, déperlance ou imperméabilité revendiquée, avec sa source. | RUN-12 |
| `aerun.trail_protection` | Produit | Protection trail | `multi_line_text_field` | Pare-pierres, plaque de protection, drainage, maintien. | RUN-18 |

### Chaussant et tailles

Utilisé par : Bloc Conseil de taille et lien Guide des tailles du sélecteur de variante.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.fit` | Produit | Chaussant | `single_line_text_field` | choix : `snug`, `true`, `roomy` ; snug : taille petit ; true : conforme ; roomy : taille grand. | RUN-16 |
| `aerun.fit_note` | Produit | Profil de chaussant | `multi_line_text_field` | Avant-pied, talon, largeur, volume ; source du conseil. | RUN-13, RUN-16 |
| `aerun.size_guide` | Produit | Guide de tailles | `metaobject_reference` | metaobject `aerun_size_guide` ; Prime sur la page choisie dans le bloc Sélecteur de variante. | RUN-14, SPT-01, SPT-08 |

### Références partagées

Utilisé par : Blocs Technologies et Preuves.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.technologies` | Produit | Technologies | `list.metaobject_reference` | metaobject `aerun_technology` | SPC-07, RUN-09, MED-09 |
| `aerun.proofs` | Produit | Preuves | `list.metaobject_reference` | metaobject `aerun_proof` | SPC-12 |
| `aerun.faq` | Produit | FAQ produit | `list.metaobject_reference` | metaobject `aerun_faq` ; Questions propres au modèle, affichées par la section FAQ. | PIN-11 |

### Produits liés

Utilisé par : Blocs Produits frères et Produits associés.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.siblings` | Produit | Produits frères | `list.product_reference` | Autres coloris ou versions vendus en fiches séparées. Inclure le produit lui-même pour fixer l'ordre. | PDP-19 |
| `aerun.sibling_label` | Produit | Libellé frère | `single_line_text_field` | Nom court affiché sous la miniature : coloris ou version. | PDP-19 |
| `aerun.rotation` | Produit | Rotation conseillée | `list.product_reference` | Chaussures à associer, choisies à la main. | RUN-17 |
| `aerun.compatible_products` | Produit | Compatibilités | `list.product_reference` | Associations testées. Aucune compatibilité déduite. | SPC-14, SPT-10 |

### Textile

Utilisé par : Template produit textile : coupe, mannequin, matière, poches, entretien.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.composition` | Produit | Composition | `multi_line_text_field` |  | SPT-04 |
| `aerun.apparel_fit` | Produit | Coupe | `single_line_text_field` | choix : `slim`, `regular`, `relaxed` | SPT-03 |
| `aerun.model_height` | Produit | Taille du mannequin | `dimension` | unité cm | SPT-02 |
| `aerun.model_size_worn` | Produit | Taille portée | `single_line_text_field` |  | SPT-02 |
| `aerun.model_note` | Produit | Note mannequin | `single_line_text_field` | Mensuration pertinente, par exemple tour de poitrine. | SPT-02 |
| `aerun.ventilation` | Produit | Ventilation | `multi_line_text_field` |  | SPT-04 |
| `aerun.protection` | Produit | Protection | `multi_line_text_field` | Coupe-vent, isolation, déperlance : propriétés annoncées et conditions. | SPT-05 |
| `aerun.upf` | Produit | Indice UPF | `number_integer` | de 1 à 100 | SPT-05 |
| `aerun.pockets` | Produit | Poches et rangement | `multi_line_text_field` |  | SPT-06 |
| `aerun.reflective` | Produit | Détails réfléchissants | `multi_line_text_field` | Jamais présenté comme une certification de sécurité. | SPT-07 |
| `aerun.care` | Produit | Entretien | `multi_line_text_field` |  | SPT-04, RUN-12 |

### Hydratation et accessoires

Utilisé par : Template produit hydratation : capacité, dimensions, contenu, notice.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.capacity` | Produit | Capacité | `volume` | unité ml | SPT-09 |
| `aerun.dimensions` | Produit | Dimensions | `single_line_text_field` | Dimensions du fabricant avec leur unité, par exemple « 22 × 7 cm ». | SPT-09, SPT-10 |
| `aerun.included` | Produit | Contenu de la boîte | `list.single_line_text_field` |  | SPT-09 |
| `aerun.instructions` | Produit | Notice d'utilisation | `rich_text_field` | Instructions déjà validées par le fabricant. Aucun calcul personnalisé. | SPT-13 |

### Nutrition

Utilisé par : Template produit nutrition : format, tableau nutritionnel, ingrédients.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.net_weight` | Produit | Poids net | `weight` | unité g | SPT-12 |
| `aerun.serving_size` | Produit | Portion | `weight` | unité g | SPT-11, SPT-12 |
| `aerun.servings` | Produit | Nombre de portions | `number_integer` | minimum 1 | SPT-12 |
| `aerun.nutrition_table` | Produit | Tableau nutritionnel | `json` | forme `nutrition_table` | SPT-11 |
| `aerun.ingredients` | Produit | Ingrédients | `multi_line_text_field` |  | SPT-11 |
| `aerun.allergens` | Produit | Allergènes | `multi_line_text_field` |  | SPT-11 |

### Merchandising

Utilisé par : Badge Nouveau des cartes, blocs Produits liés (alternatives, bundle).

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.release_date` | Produit | Date de sortie | `date` | Date éditoriale de sortie. Pilote le badge Nouveau (sinon : date de publication). | MCH-04 |
| `aerun.alternatives` | Produit | Alternatives de gamme | `list.product_reference` | Autres niveaux de la gamme, avec la différence de prix et de specs. | MCH-06 |
| `aerun.bundle_components` | Produit | Contenu du bundle | `list.product_reference` | Produits d'un bundle créé avec une app de bundles, si l'app ne les expose pas déjà. | MCH-13 |

### Informations produit

Utilisé par : Blocs Bénéfices, Entretien, Documents de la fiche produit.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.benefits` | Produit | Bénéfices | `list.single_line_text_field` | Trois à cinq phrases courtes, dans l'ordre d'affichage. | PIN-02 |
| `aerun.care_symbols` | Produit | Symboles d'entretien | `list.single_line_text_field` | choix : `wash-30`, `wash-40`, `hand-wash`, `no-tumble-dry`, `no-bleach`, `iron-low`, `no-iron`, `no-dry-clean`, `dry-flat` | PIN-06 |
| `aerun.manuals` | Produit | Notices et documents | `list.file_reference` | PDF de notice ou de fiche technique. Le texte alternatif du fichier sert de titre. | PIN-08 |

### Articles

Utilisé par : Template article : auteur, produits cités, articles associés, temps de lecture.

| Clé | Sur | Nom | Type | Détails | IDs |
| --- | --- | --- | --- | --- | --- |
| `aerun.author` | Article | Auteur | `metaobject_reference` | metaobject `aerun_person` ; Remplace le nom Shopify de l'auteur par sa fiche (portrait, biographie, lien). | EDT-09 |
| `aerun.products` | Article | Produits cités | `list.product_reference` |  | EDT-11, EDT-18 |
| `aerun.related_articles` | Article | Articles associés | `list.single_line_text_field` | Handles de la forme blog/article, par exemple news/choisir-sa-chaussure-de-trail. Sinon : articles du même tag, puis les plus récents. | EDT-13 |
| `aerun.reading_time` | Article | Temps de lecture (minutes) | `number_integer` | de 1 à 120 ; Remplace l'estimation calculée (mots ÷ 200). | EDT-08 |

## Formats JSON

### `size_table`

Tableau de tailles : en-têtes de colonnes, puis une ligne par taille. Les cellules sont du texte, pour garder la valeur source exacte.

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

Tableau nutritionnel du fabricant : colonnes de référence (pour 100 g, par portion), puis une ligne par nutriment avec son unité.

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

## Filtres Search & Discovery

Dans l’app Search & Discovery, ajouter ces filtres de metafields produit. Les facettes du thème affichent le pictogramme du terrain à côté de son nom.

- `aerun.terrain` (Terrains)
- `aerun.usage` (Usages)
- La largeur est une option de variante (« Width », « Largeur ») : utiliser le filtre d’option natif.
