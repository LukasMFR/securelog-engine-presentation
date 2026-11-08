# Notes d’orateur · SecureLog Engine (20 slides)

Ces notes accompagnent la présentation HTML et servent de points de parole, pas de texte à lire mot pour mot.

## 01 · SecureLog Engine

Présentez le projet ING3. Prenez l’alerte de force brute affichée en couverture comme fil conducteur et annoncez le rejeu de la série d’échecs SSH.

## 02 · Repérer une attaque dans les logs

Le cas d’usage retenu est une salle informatique fictive, avec des journaux d’authentification et réseau. Le problème est de rendre les événements lisibles et détectables sans ouvrir manuellement chaque ligne.

## 03 · Deux façons de lancer le moteur

Précision essentielle : la commande par défaut fait une analyse finie ; en entrée standard, le programme peut rester actif et émettre les alertes au fil de l’eau. Il n’y a pas de démon ni de serveur H24. Ne pas confondre traitement d’un flux et collecte automatique.

## 04 · Le trajet d’une ligne de log

Montrez les cinq étapes du pipeline. Ingestion et transformations utilisent des itérateurs ; les statistiques et les règles s’appliquent à chaque log valide. Les sorties sont publiées à la fin d’une analyse réussie.

## 05 · Les modules du moteur

Le code est organisé en package src/ avec un point d’entrée main.py. Le notebook n’est pas le moteur : il documente et illustre les résultats. Cet isolement facilite tests et maintenance.

## 06 · Une lecture ligne par ligne

Ce n’est pas un chargement massif en mémoire. Les générateurs suspendent leur exécution entre deux lignes. Le code affiché est un extrait volontairement abrégé du fichier ingestion.py ; il faut le présenter comme tel.

## 07 · Un dictionnaire par événement

Le parseur accepte une ligne séparée par | ou un objet JSON, vérifie les types et crée un dictionnaire Python. Le champ payload peut être vide dans le format à six champs et peut être omis dans le JSON. Il peut contenir du texte, un objet JSON ou une connexion. Les métadonnées de connexion (IP, port, protocole) deviennent un tuple immuable. Les données de test contiennent trois erreurs intentionnelles.

## 08 · Filtrage et normalisation des événements

C’est une consigne explicite du cours. Le pipeline emploie filter(), lambda, map() et match-case. Distinguez les lignes nettoyées de celles qui sont rejetées pour erreur de validation.

## 09 · Compteurs par niveau et utilisateur

Ces chiffres proviennent de l’exécution réelle des données fournies, pas d’un exemple inventé. Le niveau ERROR n’est pas nécessairement une attaque ; les règles de détection sont distinctes du comptage par niveau.

## 10 · Les IP actives et la blacklist

La blacklist comporte trois IP dont deux apparaissent dans les logs valides. On obtient donc 4 IP actives non blacklistées, 2 présentes dans les deux ensembles et 1 blacklistée non observée. Le moteur calcule les quatre opérations demandées.

## 11 · Trois règles dans un même moteur

Expliquez args : les fonctions de règles ; kwargs : log et configuration. Le moteur teste chaque fonction, émet une alerte si elle retourne un résultat. Un log critique issu d’une IP blacklistée peut produire deux alertes.

## 12 · Le cinquième échec déclenche l’alerte

Les exemples admin, root et bob sont ceux des fichiers fournis. Un succès annule les échecs encore en attente, comme pour bob. Dix échecs consécutifs donnent deux alertes au seuil par défaut. Le code utilise délibérément while, for, break, continue et else pour respecter le cahier des charges.

## 13 · Une alerte au cinquième échec SSH

Révélez une ligne avec → ou le bouton. Montrez le bloc de cinq échecs d’admin et l’alerte BRUTE_FORCE. Le moteur clôt le bloc dès l’alerte. La connexion réussie qui suit déclenche encore BLACKLIST. ← remonte d’un événement. Après le huitième événement, → ouvre le bilan de l’analyse complète. Depuis le bilan, ← revient sur le rejeu terminé. R ou « Recommencer » remet le rejeu à zéro. Le navigateur rejoue les résultats enregistrés sur les données fictives du dépôt.

## 14 · Le bilan de l’analyse complète

Il s’agit des résultats effectivement obtenus avec auth.log et network.log. Trois lignes volontairement mal formées sont rejetées ; trois lignes vides ou commentaires sont ignorées séparément. Les 15 alertes représentent les détections, pas 15 attaques indépendantes.

## 15 · Trois fichiers pour examiner les résultats

Chaque alerte occupe une ligne dans le fichier réel. La diapositive la met en forme et abrège le chemin source. L’audit enregistre des métadonnées sur les variables. Le moteur prépare ses résultats dans un dossier temporaire et tente de restaurer les sorties antérieures si la publication échoue.

## 16 · La suite de tests du moteur

Ne confondez pas les 45 tests avec les 48 scénarios d’échec inclus dans l’un de ces tests. Les versions Python listées sont celles déclarées par le rapport. La suite couvre notamment les sorties lors d’erreurs et la CLI.

## 17 · Le pic mémoire reste proche de 68 Ko

Ces chiffres viennent du tableau de benchmark sauvegardé dans le notebook. Ils mesurent les allocations Python via tracemalloc, pas toute la RAM du processus. La stabilité concerne un scénario avec une seule identité : ne pas la généraliser à toutes les charges.

## 18 · Les cas limites traités par le moteur

Il s’agit des difficultés techniques que le projet gère réellement, telles que le montrent le code et les tests. Il ne faut pas les présenter comme des anecdotes personnelles dont nous n’avons pas de preuve.

## 19 · Limites du moteur et pistes d’évolution

Admettez clairement les limites actuelles. Les principales améliorations sont des hypothèses de feuille de route et non des fonctionnalités réalisées. Le cahier des charges n’impose pas de serveur permanent ni d’interface graphique.

## 20 · Ce que révèle un fichier de logs.

Terminez sur les compétences Python démontrées et la différence entre ce moteur pédagogique et une plateforme de surveillance complète. Invitez aux questions.
