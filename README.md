# Supersmashbros - Theme WordPress (FSE + WooCommerce)

Theme WordPress block (Full Site Editing) pour une boutique WooCommerce au style graphique inspire de l'univers jeu video, avec forte personnalisation via `theme.json`, templates HTML de blocs, et styles utilitaires CSS.

## 1. Vue d'ensemble

- **Nom du theme**: Supersmashbros
- **Auteur**: Gabriel Laville--Devaure
- **Version**: 1.0
- **Type**: Block Theme (FSE)
- **Base e-commerce**: WooCommerce (blocs)

Le theme est structure autour de:

- `theme.json` pour les presets globaux (couleurs, typo, espacements, ombres, styles blocs).
- `templates/` pour les pages modeles (produit, panier, checkout, compte, etc.).
- `parts/` pour les morceaux reutilisables (header, footer, mini-cart, add-to-cart options...).
- `styles/*.json` pour les variations de styles de blocs (notamment boutons).
- `style.css` pour les surcharges CSS globales.
- `functions.php` pour l'enqueue des assets et la config editeur.

## 2. Prerequis

- WordPress recent (compatible `theme.json` schema WP 6.9 utilise dans le projet).
- WooCommerce actif (indispensable pour la plupart des templates).
- Un serveur PHP/MySQL standard (LAMP, MAMP, Docker, etc.).

## 3. Installation

1. Copier le dossier du theme dans:
   `wp-content/themes/Supersmashbros`
2. Activer le theme dans **Apparence > Themes**.
3. Installer/activer WooCommerce.
4. Importer/creer vos produits, categories et pages WooCommerce.
5. Configurer menus et templates dans **Apparence > Editeur**.

## 4. Structure du projet

```text
Supersmashbros/
|- functions.php
|- style.css
|- theme.json
|- README.md
|- assets/
|  |- css/
|  |  |- bgpanier.png
|  |- fonts/
|  |- js/
|     |- app.js
|- parts/
|  |- checkout-header.html
|  |- coming-soon-social-links.html
|  |- external-product-add-to-cart-with-options.html
|  |- footer.html
|  |- grouped-product-add-to-cart-with-options.html
|  |- header.html
|  |- headeraccueil.html
|  |- mini-cart.html
|  |- simple-product-add-to-cart-with-options.html
|  |- variable-product-add-to-cart-with-options.html
|- patterns/
|- styles/
|  |- btn_icon.json
|  |- btn_panier.json
|  |- btn_retour.json
|  |- btn_smash.json
|  |- btn_smash_polygon.json
|- templates/
   |- account.html
   |- archive-product.html
   |- coming_soon.html
   |- coming-soon.html
   |- index.html
   |- motion.html
   |- order-confirmation.html
   |- page-cart.html
   |- page-checkout.html
   |- product-search-results.html
   |- single-product.html
   |- taxonomy-product_attribute.html
```

## 5. Fichiers cles

### `functions.php`

- Charge `style.css` et `assets/js/app.js` via `wp_enqueue_*`.
- Active les styles dans l'editeur de blocs (`add_editor_style`).
- Active `useRootPaddingAwareAlignments` dans les settings editeur.

Note importante:

- Les versions des assets utilisent `time()` (cache busting agressif). C'est pratique en dev, mais a remplacer en production par une version stable (ex: `wp_get_theme()->get('Version')` ou hash de fichier) pour beneficier du cache navigateur.

### `theme.json`

Configure:

- Layout global (`contentSize`, `wideSize`).
- Palette personnalisee (noir, bleu, turquoise, jaune-orange, etc.).
- Gradients thematiques (hero produit, diagonales, etc.).
- Echelles de spacing, typo et ombres (`panel`, `button`).
- Styles globaux des headings, paragraphs, boutons et blocs WooCommerce.

### `style.css`

Contient des surcharges visuelles:

- Effets de trame diagonale sur cards/produits.
- Stylage panier/compte WooCommerce.
- Corrections de rendu pour certains blocs (ex: cover).

### `assets/js/app.js`

- Fichier JS principal du theme.
- Actuellement present mais vide.

## 6. Templates et parts WooCommerce

Le theme repose largement sur les blocs WooCommerce:

- Fiche produit personnalisee (`templates/single-product.html`):
  - hero produit (titre, extrait, prix, note, formulaire panier),
  - galerie image,
  - details produit,
  - collection produits similaires.
- Checkout (`templates/page-checkout.html`) avec un header dedie (`parts/checkout-header.html`).
- Templates pour panier, compte client, recherches produits, archives attributs, confirmation commande.
- Parts dediees aux variantes de formulaire d'ajout panier:
  - simple,
  - variable,
  - grouped,
  - external.

## 7. Styles de blocs (`styles/*.json`)

Des variations de style sont fournies, notamment pour `core/button`:

- Bouton Smash (`btn_smash.json`): contour noir, typo condensed uppercase, ombre, fond jaune-orange.
- Autres variantes (`btn_icon`, `btn_panier`, `btn_retour`, `btn_smash_polygon`).

Attention:

- `btn_panier.json` fait reference a `--wp--preset--color--accent` et `--wp--preset--color--white`, qui ne correspondent pas aux slugs principaux definis dans `theme.json` (ex: `blanc`). Verifier ce style si vous constatez des couleurs non appliquees.

## 8. Personnalisation

### Via l'Editeur de site

1. Aller dans **Apparence > Editeur**.
2. Modifier les templates (`templates/*.html`) et template parts (`parts/*.html`).
3. Ajuster navigation, entete, footer et pages WooCommerce depuis l'interface blocs.

### Via `theme.json`

- Ajouter/supprimer des couleurs dans `settings.color.palette`.
- Ajuster les tailles typo dans `settings.typography.fontSizes`.
- Modifier ombres/bordures des boutons dans `styles.blocks.core/button`.

### Via CSS

- Ajouter les surcharges dans `style.css`.
- Garder les regles specifiques WooCommerce regroupees par section (panier, compte, cards, etc.).

## 9. Workflow de developpement recommande

1. Travailler d'abord la structure via blocs (`templates/`, `parts/`).
2. Stabiliser design tokens dans `theme.json`.
3. Finaliser les details avec `style.css`.
4. Ajouter le JS seulement pour les besoins interactifs reels.

Checklist QA minimale:

- Test responsive mobile/tablette/desktop.
- Test parcours e-commerce complet: produit -> panier -> checkout -> confirmation.
- Verification contraste et lisibilite.
- Verification du rendu compte client WooCommerce.

## 10. Bonnes pratiques et maintenance

- Eviter les URLs absolues en dur dans les templates (ex: logos) pour faciliter migration entre environnements.
- Remplacer `time()` en production pour reduire les retelechargements de fichiers statiques.
- Versionner les changements de templates WooCommerce apres chaque mise a jour plugin.
- Documenter chaque nouveau template/part ajoute.

## 11. Depannage rapide

- **Le style ne se met pas a jour**: vider cache navigateur/CDN, puis verifier l'enqueue de `style.css`.
- **Un bloc WooCommerce n'apparait pas**: verifier que WooCommerce est actif et a jour.
- **Couleurs incoherentes sur un bouton**: verifier les slugs de presets dans `styles/*.json` vs `theme.json`.
- **Mise en page casssee dans l'editeur**: verifier `add_editor_style('style.css')` et les presets `theme.json`.

## 12. Evolutions conseillees

- Ajouter une vraie logique JS dans `assets/js/app.js` (menu mobile, filtres produits, micro-interactions).
- Structurer les styles CSS par composants (cards, header, checkout, compte, etc.) pour faciliter la maintenance.
- Ajouter une section "contribution" si le theme devient collaboratif.

## 13. Licence

A definir selon votre contexte de diffusion (projet perso, scolaire, client, open source, etc.).
