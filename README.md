[README.md](https://github.com/user-attachments/files/34407815/README.md)
# L’Extension Café Lounge — site Web

Site vitrine bilingue (français d’abord, anglais complet) pour L’Extension Café Lounge, au 200, rue Bernard Ouest, dans le Mile End à Montréal.

Le site est entièrement statique : HTML, CSS et JavaScript, sans cadriciel ni dépendance. Il s’héberge gratuitement et se charge vite, même sur un vieux téléphone.

## Pages

| Français | Anglais | Contenu |
|---|---|---|
| `/` | `/en/` | Accueil : section d’ouverture, manifeste, journée type, mac & cheese, avis Google, visite et carte |
| `/menu/` | `/en/menu/` | Menu complet, navigation collante par catégories, pictogrammes V / VG / SG |
| `/contact/` | `/en/contact/` | Coordonnées, heures, carte |
| `/confidentialite/` | `/en/privacy/` | Politique de confidentialité (Loi 25) |
| `/404.html` | — | Page introuvable, bilingue |

## Modifier le contenu

**Tout se modifie dans `src/data.mjs`** : coordonnées, horaires, menu et prix, avis.

Lancez ensuite la génération du site (Node 18 ou plus récent, aucune installation nécessaire) :

```bash
node build.mjs
```

Le dossier `dist/` contient alors le site complet, prêt à publier.

Les textes des pages se trouvent dans `src/pages/*.mjs`. Chaque texte y est écrit `L("français", "English")`.

## Mise en ligne sur Cloudflare

Le fichier `wrangler.jsonc` indique à Cloudflare de publier le dossier `dist/`. Sans lui, Cloudflare affiche l’erreur « Could not detect a directory containing static files ».

**Projet relié à GitHub** (Workers & Pages › Créer › Importer un dépôt) :

| Réglage | Valeur |
|---|---|
| Commande de build | `npm run build` |
| Commande de déploiement | `npx wrangler deploy` |
| Répertoire racine | le dossier qui contient `wrangler.jsonc` (laissez vide s’il est à la racine du dépôt) |

**Depuis un ordinateur** (Node 18 ou plus récent) :

```bash
npm run deploy
```

La première fois, Wrangler ouvre le navigateur pour vous connecter à votre compte Cloudflare.

**Sans outil :** dans Workers & Pages › Créer › Pages › Téléverser des fichiers, glissez le **contenu** du dossier `dist/` (pas le dossier du projet).

Une fois le domaine choisi, inscrivez-le dans `site.url` (fichier `src/data.mjs`) et relancez le build. Cette adresse sert aux balises canonical, au hreflang, au sitemap et aux images de partage.

## ⚠ À valider avant la mise en ligne

Le contenu marqué `À VALIDER` dans le code est provisoire :

- [ ] **Horaires** : plausibles, mais inventés (`hours` dans `data.mjs`)
- [ ] **Menu et prix** : réalistes pour le Mile End, mais inventés (seul le mac & cheese vient des avis)
- [ ] **Domaine** (`site.url`) et **courriel public** (`site.email`)
- [ ] **Valeurs** de l’accueil (éthique, communautaire, convivial) : formulation à confirmer
- [ ] **Politique de confidentialité** : nom ou titre du responsable
- [ ] **Logo** : le mot-symbole est une reconstitution en CSS et SVG (police Righteous et fleur vectorielle). Remplacez-le par le logo vectoriel officiel s’il existe.

## Recommandations

1. **Séance photo des plats et des boissons.** Le site n’utilise qu’une photo de la façade. Les sections Menu et Mac & cheese gagneraient à avoir de vraies photos (comptez une demi-journée avec un photographe culinaire). Ajoutez-les dans `src/originals/`, inscrivez-les dans `tools/images.py`, puis lancez `python3 tools/images.py`.
2. **Fiche Google.** Ajoutez l’adresse du site, qui manque actuellement, et les horaires complets.
3. **Instagram.** Mettez le lien du site dans la bio.

## Qualité

- **Performance** (mesurée en 4G lente avec un processeur ralenti ×4) : LCP de 1,3 à 1,8 s, CLS ≤ 0,006 et environ 275 à 395 Ko par page. Sur téléphone, l’ouverture utilise un recadrage dédié plus léger. Les images sont en AVIF ou WebP, avec JPEG de secours, en plusieurs tailles. Les polices sont hébergées localement, et la carte Google ne se charge qu’au clic.
- **Accessibilité** : audit axe-core (WCAG 2.2 AA) sans violation, lien d’évitement, focus visible, navigation au clavier, contrastes AA, respect de `prefers-reduced-motion`. Le site reste complet sans JavaScript.
- **SEO** : balises title et description uniques par page, hreflang fr-CA / en-CA, `sitemap.xml`, données structurées `CafeOrCoffeeShop` (adresse, heures, menu), images Open Graph en français et en anglais.
- **Loi 25** : aucun formulaire, aucun témoin ni outil de suivi, carte chargée seulement à la demande, politique de confidentialité.

## Animations

Le mouvement est volontairement retenu : quelques moments signature, aucun fondu générique au défilement.

- **Titres** : les mots glissent hors d’un masque au chargement.
- **Fleur signature** : elle éclot une fois au chargement.
- **Photo d’ouverture** : elle apparaît en arche ; la pastille « café · lounge » tourne lentement.
- **Manifeste** : les mots s’allument au fil de la lecture.
- **Journée type** (ordinateur) : les cartes passent du gris à leur couleur, de 9 h à 17 h, au fil du défilement.
- **Bandeau** : il défile en continu.
- **Photos** : effet de parallaxe natif, sans JavaScript.
- **Changement de page** : transitions fluides dans Chrome et Edge.

Les animations hors écran sont mises en pause, et tout est désactivé si l’appareil demande moins de mouvement.

## Mobile

- La photo de la façade s’affiche en tête, dans un recadrage dédié.
- Une barre d’actions fixe en bas de l’écran donne accès en un geste au **Menu**, aux **Heures**, à l’**Itinéraire** et à l’**Appel**.
- Les cartes de la journée deviennent un carrousel à faire glisser, et les catégories du menu défilent horizontalement.
- Ce qui fait doublon avec la barre d’actions est masqué sous 760 px : bouton « Nous trouver », boutons Itinéraire / Appeler de la visite, lien Google Maps de la carte, colonnes Heures et Explorer du pied de page.
- Les blocs lourds sont compactés : valeurs et coordonnées en lignes, disque mac & cheese réduit, avis en rangée à faire glisser, rythme vertical resserré (sections de 56 px, en-tête de 62 px).
- L’ouverture utilise une phrase courte sur téléphone, pour que « Voir le menu » apparaisse dès le premier écran (375 et 360 px). Sur la page Menu, l’introduction est masquée pour aller droit aux plats.
- Toutes les règles mobiles sont regroupées dans la section « Passe mobile » de `main.css`.

## Structure

```
build.mjs            Générateur (src → dist)
wrangler.jsonc       Configuration Cloudflare : publie le dossier dist/
src/data.mjs         ← Contenu modifiable (coordonnées, heures, menu…)
src/lib.mjs          Utilitaires : routes, icônes, fleur SVG, formatage
src/layout.mjs       Gabarit commun (en-tête, pied de page, SEO)
src/pages/           Une page par fichier (FR et EN) : accueil, menu, contact, confidentialité, 404
src/assets/          CSS, JS, polices, images optimisées, icônes
src/originals/       Photos sources
src/static/          Fichiers copiés tels quels (manifeste, _headers)
tools/               Images responsives, images de partage, captures
dist/                Site généré, prêt à publier
```
