# Validation de la présentation SecureLog Engine

`GATE: PASS` pour la version à mise en page fluide. Le canevas fixe est
validé par les vérifications ci-dessous.

## Application de design-taste-frontend 2.5.1

Le diaporama présente un outil logiciel au jury de l’ECE. La couverture reprend
le vert terminal du projet et montre un extrait d’alerte produit par le moteur.
Les deux colonnes partagent la grille de la présentation. La typographie système
et le code HTML, CSS et JavaScript intégré permettent l’ouverture hors ligne.

L’ouverture se joue une fois. Le parcours d’une ligne dans le pipeline et la
révélation de la règle au cinquième échec SSH rythment les diapositives 4 et 13.
Le présentateur garde le contrôle des transitions. La préférence de mouvement
réduit désactive les animations.

Les dérogations du contrôle officiel tiennent au format de la soutenance.

- `copy-headline` — Le titre reprend le nom officiel du projet. Le texte
  immédiatement dessous décrit le moteur Python d’analyse de logs.
- `moment-none` — Les deux animations du diaporama sont intégrées au fichier et
  se déclenchent à la navigation ou pendant le rejeu. Le contrôle détecte les
  modules d’animation du kit.
- `container-edges` — La piste de progression de 4 px traverse le bas de
  l’écran. Le contenu de chaque diapositive suit sa grille.
- `container-edges` — La barre de navigation flottante suit le bord droit
  de la fenêtre, avec 6 px de retrait en plus de la zone de sécurité éventuelle.
- `one-section` — Le lien d’accès clavier précède les diapositives dans le
  conteneur principal et reste disponible à chaque étape.

## Vérifications

- Les 20 diapositives et leurs notes sont présentes. Le navigateur affiche une
  seule diapositive à la fois et rend les autres inactives au clavier.
- Dans Chrome, navigation, liens directs par ancre, sommaire, aide, focus,
  notes et rejeu vérifiés. Le sixième événement déclenche `BRUTE_FORCE`, la
  connexion suivante réinitialise la série. Le retour et le redémarrage ont
  été testés.
- Les 20 diapositives tiennent sans défilement à 1440×900, 1280×720,
  1024×768 et 820×1180. La mise en page a également été contrôlée à
  768×1024, 390×844 et 320×640. Les diapositives longues restent
  défilables verticalement sur les écrans étroits.
- Le contrôle de mise en page du skill est passé sur les sept formats.
  Le contrôle de performance à 1440 et 390 px est passé, avec un CLS
  mesuré de 0 et aucune requête en échec.
- Le contrôle étendu relève neuf écarts bruts. Le titre et la barre de
  progression expliquent cinq signalements. Les quatre autres portent sur les
  marges symétriques de 50 px à 820 px et de 38 px à 768 px, conservées
  pour donner davantage de place au texte et au terminal sur tablette.
  Le contrôle à 320×640 ne signale aucun débordement.
- Le contrôle rédactionnel a signalé les bandeaux de chiffres, les étapes
  numérotées et les repères de diapositive. Les chiffres viennent des
  données et des mesures du projet. Les numéros suivent le déroulé oral.
- Une exécution du moteur avec sortie temporaire a confirmé 22 logs
  valides, 3 rejets et 15 alertes. Les 45 tests Python passent sur une
  copie temporaire du dépôt du moteur.

## Retouches de la soutenance

Le marqueur du cinquième échec est centré dans sa rangée. Le total de la
diapositive 16 est espacé et aligné sur la ligne de base. Les cartes d’export
gardent un dégagement au-dessus de la navigation compacte en bas à droite.
La barre est à 6 px des bords droit et inférieur hors zone de sécurité.

Chrome a vérifié les diapositives 3, 12, 13, 15 et 16 aux sept formats de
projection, tablette et mobile indiqués ci-dessus. Les boutons conservent
une cible de 44 px et le bouton Suivante déroule le rejeu.

## Rendu pour la projection

Le chapitre, le compteur et l’aide rejoignent les commandes en bas à droite.
La progression occupe toute la largeur au bas de la fenêtre. Sur téléphone,
la barre passe sur deux rangées pour conserver les cibles de 44 px.

Les 20 diapositives ont été parcourues dans Chrome sur huit formats de 320 px
au plein écran 1920×1080. La surface libérée en haut, les marges latérales
réduites et les blocs techniques agrandis donnent davantage de place au texte,
aux graphiques et au code. La navigation au clavier, l’aide, les notes et les
huit étapes du rejeu ont été vérifiées.

### Composition plein écran

Les groupes de contenu sont centrés verticalement dans l’espace de projection.
Les marges latérales restent symétriques. À 2560×1440, la diapositive 14 passe
de 1600 à 2384 px de largeur utile. L’espace entre les panneaux et les
commandes passe de 491 à 62 px. Les chiffres, le graphique et le terminal
grandissent avec la fenêtre.

Chrome a parcouru les 20 diapositives sur neuf formats. Le contenu tient
sans défilement et reste dégagé des commandes à 2560×1440, 1920×1080,
1440×900, 1280×720 et 1024×768.

### Canevas 16:9 fixe

Chaque diapositive utilise les mêmes dimensions logiques de 1920×1080.
Le navigateur met le canevas entier à l’échelle dans la zone disponible
au-dessus des commandes. Les marges autour du canevas absorbent les
différences de ratio entre fenêtres. Le sommaire, l’aide, la navigation
et la progression conservent leur propre mise en page.

Dans Chrome, les 20 diapositives ont été vérifiées sur neuf tailles de
fenêtre de 320×640 à 2560×1440. Le ratio, les dimensions de composition
et la typographie des titres restent identiques. La navigation au clavier,
l’aide, les notes et les huit étapes du rejeu ont été vérifiées.
Le contrôle de page web sur la hauteur du premier écran est remplacé ici
par le contrôle du canevas et du dégagement au-dessus des commandes.
