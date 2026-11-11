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

À droite, le tiret indique qu’on lit sur l’entrée standard. Un autre programme peut alors nous envoyer des lignes. Tant que la source continue à écrire, le moteur peut émettre les alertes au fil de l’eau avec `--stream-alerts`. C’est le sens de « temps réel » ici. La collecte dépend du programme qu’on lui raccorde.

## 04 · Le trajet d’une ligne de log — Lukas — 0:40

Suivons une ligne d’authentification dans le schéma. L’ingestion la lit et conserve sa provenance. Le parseur en extrait les champs, puis la normalisation donne à la suite du code un format commun.

À partir de cet événement, on met à jour les compteurs et on applique les règles de sécurité. Une règle peut alors produire une alerte. Ce parcours se fait au fil de la lecture. Les fichiers du bilan sont publiés une fois l’analyse terminée avec succès.

## 05 · Les modules du moteur — Lukas — 0:45

L’énoncé imposait un point d’entrée, `main.py`, et du code organisé en package. `main.py` reçoit les options de la commande et orchestre le traitement. Le découpage de `src` permet de tester le parseur avec une seule ligne, indépendamment du reste.

Pour l’environnement, on utilise un `venv`. Il isole l’installation du projet. Le fichier `requirements.txt`, issu d’un `pip freeze` dans cet environnement, ne contient aucune dépendance à installer puisque le moteur utilise la bibliothèque standard. Python 3.10 est le minimum, notamment pour `match-case`. Le notebook sert à présenter les résultats et les mesures.

## 06 · Une lecture ligne par ligne — Lukas — 0:45

L’énoncé demandait explicitement `yield` et `yield from`. Dans le générateur appelé par cet extrait, `yield` fournit une ligne, avec le nom du fichier et son numéro de ligne. Le générateur se met en pause à cet endroit. Quand l’étape suivante demande un nouvel élément, il reprend là où il s’était arrêté. On évite ainsi de construire une grande liste avant de commencer le traitement.

`yield from` délègue cette lecture au générateur du fichier courant. Quand le premier fichier est épuisé, on passe au suivant avec la même interface. Le reste du programme utilise le même itérateur du début à la fin.

## 07 · Un dictionnaire par événement — Lukas — 0:50

Le parseur reconnaît deux formats, une ligne dont les champs sont séparés par des barres verticales et un objet JSON. Dans les deux cas, on vérifie les champs et on construit un dictionnaire Python. Il contient notamment une date, une sévérité, une IP validée, un utilisateur et un type d’événement. Les clés nommées permettent ensuite aux règles d’accéder directement au champ dont elles ont besoin.

Le contenu associé à l’événement, le `payload`, peut être du texte, un objet JSON ou une description de connexion. Pour cette dernière, on utilise le tuple demandé dans l’énoncé. L’IP, le port et le protocole ont un ordre fixe et le tuple est immuable. Sacha va maintenant vous montrer comment on exploite ces événements.

## 08 · Filtrage et normalisation des événements — Sacha — 0:55

Le premier `filter` utilise une `lambda`, une fonction anonyme qui écarte les lignes vides et celles qui commencent par un dièse.

Ensuite, `map` applique la même fonction à chaque ligne restante. Elle parse et normalise la ligne. En mode standard, si une ligne est invalide, cette fonction la compte comme rejetée et renvoie `None`. Le deuxième filtre écarte ces résultats. Les deux opérations travaillent à la demande, comme le générateur.

Enfin, `match-case` reconnaît la structure du contenu. Un texte est nettoyé, un dictionnaire d’erreur donne un message avec son code et un tuple donne une description de la connexion.

## 09 · Compteurs par niveau et utilisateur — Sacha — 0:35

On compte les événements par sévérité et par utilisateur. L’énoncé demandait un `defaultdict`. Avec `defaultdict(int)`, quand un utilisateur apparaît pour la première fois, son compteur part automatiquement de zéro. On peut directement l’incrémenter sans vérifier si la clé existe.

Sur les vingt-deux événements du jeu d’essai, onze sont classés ERROR. Ce graphique décrit la sévérité enregistrée dans le journal. Pour décider si un événement mérite une alerte, on utilise des règles dédiées.

## 10 · Les IP actives et la blacklist — Sacha — 0:45

Le `set` déduplique les adresses, notamment celles qui ont généré des erreurs. À gauche, on voit toutes les IP actives. À droite, la blacklist est un `frozenset`, ce qui empêche de la modifier accidentellement pendant l’analyse.

Les quatre opérations demandées répondent à quatre questions. L’intersection trouve les deux IP qu’on a effectivement observées et qui figurent dans la liste. L’union donne les sept adresses distinctes au total. La différence donne les quatre IP actives hors de la liste. La différence symétrique retient les cinq adresses qui ne figurent que d’un côté.

## 11 · Trois règles dans un même moteur — Sacha — 0:45

L’énoncé demandait que le moteur reçoive ses règles comme des fonctions. Dans l’appel affiché, `*args` récupère ces fonctions. On passe les fonctions elles-mêmes, et c’est `apply_security_rules` qui les appelle. `**kwargs` permet de transmettre les paramètres nommés, comme le log, le seuil, les historiques et la blacklist.

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

**À 7/8, puis 8/8.** Au succès suivant, le compteur du bloc revient à zéro à l’écran. L’adresse, elle, reste sur la blacklist. Bob a son propre compteur. Jeanne va vous montrer ce qu’on obtient sur les deux fichiers complets.

## 14 · Le bilan de l’analyse complète — Jeanne — 0:40

Sur les deux fichiers complets, on obtient vingt-deux événements valides. On compte séparément trois lignes rejetées et trois lignes ignorées, vides ou commentées.

Le moteur produit quinze alertes. Douze viennent de la blacklist, deux de la règle de force brute et une du niveau critique. Plusieurs alertes peuvent provenir d’une seule ligne, comme celle qu’on vient de voir. Et les douze alertes de blacklist correspondent à des événements répétés sur seulement deux adresses présentes dans la liste.

## 15 · Trois fichiers pour examiner les résultats — Jeanne — 0:45

`summary.json` donne la vue d’ensemble, avec les compteurs, la configuration et les rejets. Dans `alerts.jsonl`, chaque alerte tient sur une ligne JSON. Chaque alerte indique le fichier source et le numéro de ligne à examiner.

Pour l’audit, l’énoncé imposait l’introspection avec `globals()` et `locals()`. Ces fonctions donnent accès aux noms de la portée du module et de la portée locale. Notre audit en extrait les noms, les types et, pour certaines variables, le nombre d’éléments. Ici, on voit que la blacklist est un `frozenset` de trois éléments. Cet inventaire permet de contrôler l’état du programme en limitant les données exposées.

## 16 · La suite de tests du moteur — Jeanne — 0:40

Les quarante-cinq tests automatisés couvrent notamment la force brute. Ils vérifient les deux cent cinquante-six suites possibles de huit réussites ou échecs, avec des seuils de un à cinq. Ça couvre des cas comme un succès juste avant le seuil.

La suite vérifie aussi les entrées invalides, les options de la ligne de commande et la conservation des résultats précédents quand la publication échoue.

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
