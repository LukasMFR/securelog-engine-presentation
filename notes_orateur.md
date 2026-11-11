# SecureLog Engine · Script de soutenance

**15 minutes chronométrées, rejeu compris.** Les 5 minutes restantes sont réservées à la suite de la présentation. Les temps par slide sont des cibles de répétition, gestes et transitions compris. Les lignes « À faire » indiquent des gestes et restent silencieuses.

- **Lukas · slides 01 à 07 · 00:00–05:00** — Du problème au dictionnaire Python.
- **Sacha · slides 08 à 13 · 05:00–10:00** — Transformations, règles et rejeu SSH.
- **Jeanne · slides 14 à 20 · 10:00–15:00** — Résultats, validation et perspectives.

## 01 · SecureLog Engine — Lukas — 0:40

Bonjour à toutes et à tous. Je suis Lukas, et je vais vous présenter SecureLog Engine avec Sacha et Jeanne.

Sur la droite, vous voyez une alerte produite par notre moteur Python. Le compte admin vient d’enregistrer son cinquième échec de connexion SSH. L’alerte indique le compte, l’adresse IP et la ligne du journal où revenir vérifier l’événement.

Je vais suivre le trajet d’un log. Sacha expliquera la détection, puis Jeanne présentera les résultats et leur validation.

## 02 · Repérer une attaque dans les logs — Lukas — 0:40

On a imaginé une salle informatique qui produit deux types de journaux, l’un pour les authentifications, l’autre pour le réseau. Quelqu’un qui examine ces journaux peut chercher des échecs de connexion, une IP connue ou un événement critique.

À la main, la recherche devient difficile quand plusieurs comptes et adresses se mélangent dans des milliers de lignes de formats différents. L’énoncé nous demandait donc d’automatiser ce travail avec une application en ligne de commande capable de traiter des fichiers volumineux sans les charger entièrement en RAM.

## 03 · Deux façons de lancer le moteur — Lukas — 0:40

La commande affichée à gauche, `python main.py`, lit les deux fichiers fournis. L’analyse s’arrête à la fin des fichiers et le moteur publie les résultats.

À droite, le tiret indique qu’on lit sur l’entrée standard. Ici, `cat` simule le flux avec les mêmes fichiers. Avec `--stream-alerts`, chaque alerte sort dès sa détection, en JSON. Le bilan va sur la sortie d’erreur à la fin du flux, ce qui permet à un autre programme de lire la sortie standard comme du JSON ligne par ligne.

## 04 · Le trajet d’une ligne de log — Lukas — 0:40

Suivons une ligne d’authentification dans le schéma. L’ingestion la lit et conserve sa provenance. Le filtre écarte les lignes vides et les commentaires. Le parseur extrait les champs et la normalisation donne à la suite du code un format commun.

À partir de cet événement, on met à jour les compteurs et on applique les règles de sécurité. Une règle peut alors produire une alerte. Ce parcours se fait au fil de la lecture. Les fichiers du bilan sont publiés une fois l’analyse terminée avec succès.

## 05 · Les modules du moteur — Lukas — 0:45

L’énoncé imposait un point d’entrée, `main.py`, et du code organisé en package. `main.py` reçoit les options de la commande et orchestre le traitement. Le découpage de `src` permet de tester le parseur avec une seule ligne, indépendamment du reste.

Pour l’environnement, on utilise un `venv`. Il isole l’installation du projet. Le fichier `requirements.txt` est vide parce que `pip freeze` ne trouve aucune dépendance tierce. Le moteur utilise la bibliothèque standard et demande Python 3.10 au minimum pour `match-case`. Le notebook `rapport.ipynb` documente les résultats et les mesures.

## 06 · Une lecture ligne par ligne — Lukas — 0:45

L’énoncé demandait explicitement `yield` et `yield from`. Dans le générateur appelé par cet extrait, `yield` fournit une ligne, avec le nom du fichier et son numéro de ligne. Le générateur se met en pause à cet endroit. Quand l’étape suivante demande un nouvel élément, il reprend là où il s’était arrêté. On évite ainsi de construire une grande liste avant de commencer le traitement.

`yield from` délègue cette lecture au générateur du fichier courant. Quand le premier fichier est épuisé, on passe au suivant avec la même interface. Le reste du programme utilise le même itérateur du début à la fin.

## 07 · Un dictionnaire par événement — Lukas — 0:50

Le parseur reconnaît deux formats, une ligne dont les champs sont séparés par des barres verticales et un objet JSON. Dans les deux cas, on vérifie les champs et on construit un dictionnaire Python. On convertit l’horodatage en `datetime`, on valide l’IP source et on conserve l’utilisateur et le type d’événement. La sévérité prend l’une des quatre valeurs de l’énoncé, d’INFO à CRITICAL.

Le `payload` peut être du texte, un objet JSON ou une liste décrivant une connexion. On valide cette liste puis on la convertit en tuple immuable, avec une IP, un port entier et un protocole. On garde ces trois champs ensemble dans un ordre fixe. Sacha va maintenant vous montrer comment on exploite ces événements.

## 08 · Filtrage et normalisation des événements — Sacha — 0:55

Le premier `filter` utilise une `lambda`, une fonction anonyme qui écarte les lignes vides et celles qui commencent par un dièse.

Ensuite, `map` applique la même fonction de parsing et de normalisation à chaque ligne. Par exemple, le parseur enlève les espaces autour du niveau et met `error` en majuscules pour que le compteur retrouve la même catégorie. En mode standard, une ligne invalide donne `None`, que `filter(None, …)` écarte. Tout se fait à la demande.

Avec `match-case`, on reconnaît la forme du contenu et on extrait directement ce qui nous intéresse. On nettoie un texte brut. Pour un dictionnaire d’erreur ou un tuple de connexion, on insère le code ou les champs de connexion dans un message avec une f-string.

## 09 · Compteurs par niveau et utilisateur — Sacha — 0:35

On compte les événements par sévérité et par utilisateur. L’énoncé demandait un `defaultdict`. Avec `defaultdict(int)`, quand un utilisateur apparaît pour la première fois, son compteur part automatiquement de zéro. On peut directement l’incrémenter sans vérifier si la clé existe.

Sur les vingt-deux événements du jeu d’essai, onze sont classés ERROR. Ce graphique décrit la sévérité enregistrée dans le journal. Pour décider si un événement mérite une alerte, on utilise des règles dédiées.

## 10 · Les IP actives et la blacklist — Sacha — 0:45

On tient deux ensembles `set`, l’un pour les IP actives et l’autre pour les IP qui ont émis un événement ERROR ou CRITICAL. Une IP répétée compte une seule fois. À gauche, on voit l’ensemble des IP actives. À droite, la blacklist est un `frozenset`, ce qui empêche de la modifier accidentellement pendant l’analyse.

Les quatre opérations demandées répondent à quatre questions. L’intersection trouve les deux IP qu’on a effectivement observées et qui figurent dans la liste. L’union donne les sept adresses distinctes au total. La différence donne les quatre IP actives hors de la liste. La différence symétrique retient les cinq adresses qui ne figurent que d’un côté.

## 11 · Trois règles dans un même moteur — Sacha — 0:45

En Python, une fonction peut être passée comme une valeur. C’est ce que demande l’énoncé. `apply_security_rules` reçoit les fonctions dans `*args` et les appelle une par une. `**kwargs` reçoit le log et les paramètres nommés, dont le seuil, réglable avec `--threshold`.

On peut donc ajouter une règle dans l’appel en gardant la même boucle d’exécution. Aujourd’hui, on cherche les séries d’échecs, les adresses de la blacklist et les événements critiques. Chaque fonction prend sa propre décision. Par exemple, un événement critique provenant d’une IP sur liste noire peut déclencher deux alertes.

## 12 · Le cinquième échec déclenche l’alerte — Sacha — 0:55

On suit les tentatives par couple IP et utilisateur. Un échec sur le compte de Bob ne prolonge donc pas la série d’admin. Par défaut, cinq échecs consécutifs pour un même couple forment un bloc et produisent une alerte. Sur la ligne de Bob, la connexion réussie coupe la série, donc ses deux groupes d’échecs ne s’additionnent pas.

Le sujet imposait aussi les deux boucles et leurs instructions de contrôle. Le `while` lance la vérification quand le seuil est atteint ou qu’un succès arrive. Le `for` retire les tentatives une à une. Un échec fait `continue`. Un succès fait `break`. Le `else` du `for` s’exécute seulement si la boucle termine sans `break`. C’est là qu’on émet l’alerte et que le bloc est consommé.

## 13 · Une alerte au cinquième échec SSH — Sacha — 1:05

> À faire — Suivre les gestes dans l’ordre. → une fois pour Alice, une fois pour le premier échec d’admin, quatre fois jusqu’à 6/8, puis une fois pour le succès d’admin et une fois pour Bob. À 8/8, → ouvre le bilan pour Jeanne. R relance le rejeu si besoin.

Voici les huit premiers événements valides du journal d’authentification. Ce rejeu reprend les résultats du moteur Python.

**À 1/8.** Alice se connecte normalement. Le compteur d’alertes reste à zéro.

**À 2/8.** Voici le premier échec d’admin. Son adresse est sur la blacklist, donc elle déclenche déjà une alerte. Le compteur d’échecs du bloc, lui, indique un.

**À 6/8.** À la cinquième tentative échouée d’admin, le moteur signale une force brute. La même ligne déclenche aussi BLACKLIST.

**À 7/8, puis 8/8.** Le rejeu remet le compteur à zéro à l’événement suivant. Le moteur avait déjà consommé le bloc au moment de l’alerte. La connexion réussie déclenche encore BLACKLIST, puis Bob démarre sa propre série. Jeanne va vous montrer le bilan des deux fichiers.

## 14 · Le bilan de l’analyse complète — Jeanne — 0:40

Sur les deux fichiers complets, on obtient vingt-deux événements valides. On compte séparément trois lignes rejetées et trois lignes ignorées, vides ou commentées.

Le moteur produit quinze alertes. Douze viennent de la blacklist, deux de la règle de force brute et une du niveau critique. Plusieurs alertes peuvent provenir d’une seule ligne, comme celle qu’on vient de voir. Et les douze alertes de blacklist correspondent à des événements répétés sur seulement deux adresses présentes dans la liste.

## 15 · Trois fichiers pour examiner les résultats — Jeanne — 0:45

`summary.json` donne la vue d’ensemble, avec les compteurs, la configuration et les rejets. Dans `alerts.jsonl`, chaque alerte tient sur une ligne JSON. Chaque alerte indique le fichier source et le numéro de ligne à examiner.

Dans `audit.json`, on utilise l’introspection demandée. `globals()` donne accès à la portée du module et `locals()` à celle de la fonction. On en extrait les noms, les types et parfois le nombre d’éléments. Pour le débogage, on peut ainsi vérifier que la blacklist chargée est bien un `frozenset` de trois éléments. Le rapport exporte seulement ces métadonnées.

## 16 · La suite de tests du moteur — Jeanne — 0:40

Les quarante-cinq tests automatisés couvrent notamment la force brute. Ils vérifient les deux cent cinquante-six suites possibles de huit réussites ou échecs, avec des seuils de un à cinq. Ça couvre des cas comme un succès juste avant le seuil.

On teste aussi les entrées invalides, la ligne de commande et la restauration des résultats après un échec de publication. Les annotations de types décrivent les données attendues et le rapport documente le contrôle du style PEP 8.

## 17 · Le pic mémoire reste proche de 68 Ko — Jeanne — 0:45

Pour vérifier la contrainte mémoire, on a mesuré le pic des allocations suivies par `tracemalloc`. Dans le graphique, on passe de dix mille à un million de lignes avec le même couple IP et utilisateur. Le temps augmente, et le pic mesuré reste proche de soixante-huit kilo-octets. Cette mesure porte sur les allocations suivies par Python pendant l’essai.

Avec dix mille couples distincts, le pic monte à environ dix-sept mégaoctets. Les générateurs évitent de conserver toutes les lignes, mais les compteurs, les ensembles et les historiques en cours prennent de la place.

## 18 · Les cas limites traités par le moteur — Jeanne — 0:40

Face à une entrée mal formée, le moteur indique la source et la ligne, puis poursuit l’analyse en mode standard. Avec `--strict`, il s’arrête sur l’erreur. C’est pratique quand on cherche à vérifier la qualité d’une source avant de s’appuyer sur ses résultats.

Autre cas, l’écriture des fichiers de sortie. Le moteur les prépare dans un dossier temporaire avant de les publier. Si une erreur survient pendant le remplacement, il tente de restaurer les anciennes sorties. Ça évite autant que possible de laisser un bilan composé de fichiers issus de deux analyses différentes.

## 19 · Limites du moteur et pistes d’évolution — Jeanne — 0:45

Si on voulait utiliser le moteur pour surveiller une vraie salle informatique, je commencerais par la règle de force brute. Aujourd’hui, elle suit l’ordre d’arrivée et compte les échecs consécutifs, quel que soit le temps qui les sépare. Cinq échecs dans une minute et cinq dans une journée peuvent donc déclencher le même signal.

Une évolution serait de compter les échecs dans une fenêtre glissante, avec des horodatages ramenés en UTC et une politique pour les événements qui arrivent en retard. Ensuite, on pourrait raccorder une source syslog et tester cette règle sur plusieurs machines. On pourrait alors mesurer combien d’alertes sont réellement utiles et ajuster le seuil selon le contexte.

## 20 · Ce que révèle un fichier de logs — Jeanne — 0:45

Revenons à l’alerte sur le compte admin. On peut retrouver la ligne qui l’a déclenchée, expliquer pourquoi la règle a réagi et vérifier son comportement sur d’autres séries de connexions.

Ce qui nous a plu dans le projet, c’est de voir comment des choix Python très concrets finissent par changer ce qu’on est capable de repérer dans un journal. Comme on s’oriente vers la cybersécurité, on aimerait prolonger l’essai sur des journaux de labo, avec une fenêtre de temps, pour voir si les alertes aident vraiment à décider quoi examiner en premier.

Merci pour votre attention.
