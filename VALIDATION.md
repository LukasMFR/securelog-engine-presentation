# Validation de la présentation SecureLog Engine

`GATE: OPEN 1 (rounds used up)`

## Application de design-taste-frontend 2.5.1

Le diaporama présente un outil logiciel au jury de l’ECE. La couverture reprend
le vert terminal du projet et montre un extrait d’alerte produit par le moteur.
Les deux colonnes partagent la grille de la présentation. La typographie système
et le code HTML, CSS et JavaScript intégré permettent l’ouverture hors ligne.

L’ouverture se joue une fois. Le parcours d’une ligne dans le pipeline et la
révélation de la règle au cinquième échec SSH rythment les diapositives 4 et 13.
Le présentateur garde le contrôle des transitions. La préférence de mouvement
réduit désactive les animations.

Le contrôle officiel a accepté deux dérogations liées au format de la soutenance.

- `copy-headline` — Le titre reprend le nom officiel du projet. Le texte
  immédiatement dessous décrit le moteur Python d’analyse de logs.
- `moment-none` — Les deux animations du diaporama sont intégrées au fichier et
  se déclenchent à la navigation ou pendant le rejeu. Le contrôle détecte les
  modules d’animation du kit.
Le seul écart resté ouvert est `container-edges` sur la barre de progression.
Cette ligne de 3 px part du bord de l’écran et traverse sa largeur à la fin
du diaporama. Le logo et les commandes suivent la grille.

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
