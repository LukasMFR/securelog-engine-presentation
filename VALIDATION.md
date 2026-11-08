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

- `copy-headline`. Le titre reprend le nom officiel du projet. Le texte
  immédiatement dessous décrit le moteur Python d’analyse de logs.
- `moment-none`. Les deux animations du diaporama sont intégrées au fichier et
  se déclenchent à la navigation ou pendant le rejeu. Le contrôle détecte les
  modules d’animation du kit.
- `container-edges`. La piste de progression de 4 px traverse le bas de
  l’écran. Le contenu de chaque diapositive suit sa grille.
- `container-edges`. La barre de navigation flottante suit le bord droit
  de la fenêtre, avec 6 px de retrait en plus de la zone de sécurité éventuelle.
- `one-section`. Le lien d’accès clavier précède les diapositives dans le
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
Le navigateur dimensionne le canevas par le zoom de mise en page CSS,
sans transformation de mise à l’échelle permanente. Un écran 16:9 est
rempli bord à bord. Sur un autre ratio, les marges reprennent le fond de la
diapositive active. Le sommaire, l’aide, la navigation et la progression
conservent leur propre mise en page.

Dans Chrome, les 20 diapositives ont été vérifiées sur neuf tailles de
fenêtre de 320×640 à 2560×1440. Le ratio et la composition sont conservés,
avec les arrondis de rendu du navigateur selon l’échelle. La navigation au clavier,
l’aide, les notes et les huit étapes du rejeu ont été vérifiées.
Le contrôle de page web sur la hauteur du premier écran est remplacé ici
par le contrôle du canevas et du dégagement au-dessus des commandes.

Les indices, les compteurs et les chiffres des graphiques ont été repris
pour la projection, en suivant l’espacement typographique de la présentation
Larry Fink. Le focus programmatique du diaporama ne dessine plus de contour
autour des titres. Dans Chrome, le test du raccourci F vérifie l’appel à
l’API de plein écran et l’absence de contour avec cet appel simulé.

### Dégagement et transitions de projection

La couverture réunit le titre sur une ligne, l’introduction et les noms à
gauche, face à l’extrait de terminal. Sur la diapositive 16, la commande
de test tient sur une seule ligne et sa note reste dans la même colonne.
Une zone libre sépare le contenu des commandes, tandis que le fond continue
jusqu’à la progression.

Les 20 diapositives ont été parcourues dans Chrome sur onze formats, de
320×640 à 2560×1440. Le contrôle vérifie les limites du contenu, le
dégagement au-dessus du menu et les sauts de ligne des deux éléments
signalés à 1920×1080 et 2560×1440.

La démonstration a été mesurée à chacun de ses neuf états, de 0/8 à 8/8,
sur ces mêmes formats. Le panneau reste à la même hauteur et les 15 lignes
sont visibles à la dernière étape. Le panneau s’arrête à 153 px du menu
en 1920×1080 et à 227 px en 2560×1440.

La couverture entre par étapes. Le déplacement des diapositives suit le
sens de la navigation et les barres des graphiques se déploient à leur
apparition. Les deux modes de mouvement ont été contrôlés dans Chrome.

### Dernières retouches

La couverture et la conclusion utilisent les commandes communes du
diaporama pour la navigation. Les cinq étapes du pipeline occupent une
rangée centrée de 330 px dans le canevas de projection. Les repères des
modules et des pistes d’évolution ont été agrandis.

Les panneaux de résultats ont été contrôlés sur les onze formats. Leur
bord inférieur se trouve à 155 px du menu en 1920×1080 et à 50 px en
1024×576. Le rejeu conserve ses 15 lignes visibles à l’étape 8/8 sur ces
mêmes formats.
