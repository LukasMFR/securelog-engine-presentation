# Notes d’orateur — SecureLog Engine (20 slides)

Ces notes accompagnent la présentation HTML et servent de points de parole, pas de texte à lire mot pour mot.

## 01 — SECURE LOG ENGINE .

Présentez le nom du projet et le cadre ING3. Expliquez dès le départ qu’il s’agit d’un moteur Python en ligne de commande, pas d’un service SIEM complet. Les trois noms du groupe figurent sur la couverture.

## 02 — Beaucoup de logs. Peu de signal.

Le cas d’usage retenu est une salle informatique fictive, avec des journaux d’authentification et réseau. Le problème est de rendre les événements lisibles et détectables sans ouvrir manuellement chaque ligne.

## 03 — Pas besoin d’un serveur H24.

Précision essentielle : la commande par défaut fait une analyse finie ; en entrée standard, le programme peut rester actif et émettre les alertes au fil de l’eau. Il n’y a pas de démon ni de serveur H24. Ne pas confondre traitement d’un flux et collecte automatique.

## 04 — Du log brut à l’alerte.

Montrez les cinq étapes du pipeline. Ingestion et transformations utilisent des itérateurs ; les statistiques et les règles s’appliquent à chaque log valide. Les sorties sont publiées à la fin d’une analyse réussie.

## 05 — Un rôle précis par module.

Le code est organisé en package src/ avec un point d’entrée main.py. Le notebook n’est pas le moteur : il documente et illustre les résultats. Cet isolement facilite tests et maintenance.

## 06 — Lire une ligne. Pas un fichier entier.

Ce n’est pas un chargement massif en mémoire. Les générateurs suspendent leur exécution entre deux lignes. Le code affiché est un extrait volontairement abrégé du fichier ingestion.py ; il faut le présenter comme tel.

## 07 — Deux formats en entrée. Une structure en sortie.

Le parseur accepte une ligne séparée par | ou un objet JSON, vérifie les types et crée un dictionnaire Python. Les métadonnées de connexion (IP, port, protocole) deviennent un tuple immuable. Les données de test contiennent trois erreurs intentionnelles.

## 08 — Nettoyer et router les données.

C’est une consigne explicite du cours. Le pipeline emploie filter(), lambda, map() et match-case. Distinguez les lignes nettoyées de celles qui sont rejetées pour erreur de validation.

## 09 — Des événements aux métriques.

Ces chiffres proviennent de l’exécution réelle des données fournies, pas d’un exemple inventé. Le niveau ERROR n’est pas nécessairement une attaque ; les règles de détection sont distinctes du comptage par niveau.

## 10 — Comparer les adresses IP.

La blacklist comporte trois IP dont deux apparaissent dans les logs valides. On obtient donc 4 IP actives non blacklistées, 2 présentes dans les deux ensembles et 1 blacklistée non observée. Le moteur calcule les quatre opérations demandées.

## 11 — Trois règles. Un moteur.

Expliquez args : les fonctions de règles ; kwargs : log et configuration. Le moteur teste chaque fonction, émet une alerte si elle retourne un résultat. Un log critique issu d’une IP blacklistée peut produire deux alertes.

## 12 — À la 5ᵉ erreur, une alerte.

Les exemples admin, root et bob sont ceux des fichiers fournis. Un succès annule immédiatement la série du couple. Le code utilise délibérément while, for, break, continue et else pour respecter le cahier des charges.

## 13 — Voir le signal apparaître.

Sur cette slide, n’utilisez pas la souris : → / ↓ (ou Espace) révèle une ligne de log à chaque pression ; ← / ↑ revient en arrière. Après huit lignes, → passe à la slide 14. Depuis la slide 14, ← revient sur la démonstration entièrement révélée. La touche R la réinitialise. Les huit lignes proviennent des données fictives du dépôt. Les alertes BLACKLIST et BRUTE_FORCE sont illustrées en HTML ; aucun Python ne s’exécute dans le navigateur.

## 14 — 22 logs. 15 alertes.

Il s’agit des résultats effectivement obtenus avec auth.log et network.log. Trois lignes volontairement mal formées sont rejetées ; trois lignes vides ou commentaires sont ignorées séparément. Les 15 alertes représentent les détections, pas 15 attaques indépendantes.

## 15 — Les résultats deviennent exploitables.

Le résumé est en JSON, les alertes en JSON Lines et l’audit documente l’état du processus. Le moteur prépare ses résultats dans un dossier temporaire et tente de restaurer les sorties antérieures si la publication échoue.

## 16 — Le moteur a été mis à l’épreuve.

Ne confondez pas les 45 tests avec les 48 scénarios d’échec inclus dans l’un de ces tests. Les versions Python listées sont celles déclarées par le rapport. La suite couvre notamment les sorties lors d’erreurs et la CLI.

## 17 — 100 fois plus de logs. Un pic presque stable.

Ces chiffres viennent du tableau de benchmark sauvegardé dans le notebook. Ils mesurent les allocations Python via tracemalloc, pas toute la RAM du processus. La stabilité concerne un scénario avec une seule identité : ne pas la généraliser à toutes les charges.

## 18 — Les cas difficiles ont guidé la robustesse.

Il s’agit des difficultés techniques que le projet gère réellement, telles que le montrent le code et les tests. Il ne faut pas les présenter comme des anecdotes personnelles dont nous n’avons pas de preuve.

## 19 — Un bon moteur. Pas encore un SIEM.

Admettez clairement les limites actuelles. Les principales améliorations sont des hypothèses de feuille de route et non des fonctionnalités réalisées. Le cahier des charges n’impose pas de serveur permanent ni d’interface graphique.

## 20 — Des logs bruts. Des signaux utiles.

Terminez sur les compétences Python démontrées et la différence entre ce moteur pédagogique et une plateforme de surveillance complète. Les livrables demandés sont bien ceux de l’énoncé. Invitez aux questions.
