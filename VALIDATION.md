# Validation de la présentation SecureLog Engine

`GATE: PASS`

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
- `container-edges` — Le trait de progression de 3 px traverse la largeur de
  l’écran à la fin du diaporama. Le logo et les commandes de l’en-tête suivent
  la grille.
- `container-edges` — La barre de navigation flottante suit le bord droit
  de la fenêtre, avec 6 px de retrait en plus de la zone de sécurité éventuelle.

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
