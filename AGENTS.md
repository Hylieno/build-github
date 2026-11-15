# Build GitHub

## Fonctionnement automatique

Un fichier source joint, un dossier accessible ou une URL vers du code source
constitue à lui seul une demande complète de traitement. Si l’utilisateur
indique un nom de dépôt, utilise ce nom. Sinon, dérive un nom clair du projet.
Ne demande aucune confirmation lorsque ces informations suffisent.

Une URL GitHub fournie dans le message désigne toujours le dépôt source à
importer. Un texte comme `Repo : Nom`, `Dépôt : Nom` ou
`Nom du dépôt : Nom` désigne toujours le nom du nouveau dépôt de destination
sous `Hylieno`. Ne l’interprète jamais comme un nom de branche. Pour le dépôt
source, utilise automatiquement sa branche par défaut. Ne demande quelle
branche utiliser que si l’utilisateur mentionne explicitement une branche ou
si le dépôt source ne possède aucune branche par défaut exploitable.

N’adopte pas le comportement général d’assistance sur un dépôt qui consiste à
demander s’il faut le réviser, l’exécuter ou le modifier. Dans cet environnement,
la tâche implicite et permanente est de l’importer, le compiler et publier les
résultats. Commence le travail immédiatement.

Exécute automatiquement l’ensemble du processus : récupérer et extraire la
source, créer le dépôt privé, importer les fichiers, vérifier les fichiers importés selon la section ci-dessous, analyser le système de
compilation, configurer GitHub Actions, compiler, corriger les erreurs de
configuration, relancer les workflows et publier une release avec les
résultats de compilation.

La cible par défaut est Windows x64 (Si un build PGO existe, il faut le 
compiler également, en supplémentaire). Utilise une autre plateforme, architecture,
configuration ou variante seulement lorsque l’utilisateur le demande ou
lorsque le projet ne prend pas en charge Windows x64. Si Windows x64 est
impossible, vérifie les cibles réellement prises en charge et explique le
blocage ou utilise la cible explicitement demandée.

## Règle de sécurité absolue

Ne supprime jamais un dépôt GitHub, quelles que soient les circonstances.
N’utilise aucune commande, API, interface ou automatisation capable de
supprimer un dépôt. Cette interdiction s’applique aussi aux dépôts temporaires,
aux dépôts de test, aux dépôts créés par erreur et aux dépôts devenus inutiles.
Si la suppression d’un dépôt semble nécessaire, explique pourquoi et demande
à l’utilisateur de l’effectuer lui-même manuellement sur GitHub.

## Vérification du code importé avant compilation

Avant le premier push, garde les workflows d’origine hors de `.github/workflows/`
et conserve-les comme fichiers de référence inactifs. L’import ne doit déclencher
aucune exécution automatique.

Après l’import dans le dépôt privé, relève le SHA du commit importé et fais
une revue statique de son contenu, sans exécuter le code, installer les
dépendances ni déclencher GitHub Actions. Dresse l’inventaire des fichiers
suivis, y compris les fichiers cachés, scripts, liens symboliques, sous-modules
et binaires. Si un fichier ou un sous-module demeure inaccessible, indique-le
dans le bilan et détermine s’il intervient dans le build ou la distribution.

Examine en priorité les workflows d’origine, scripts d’installation et de
compilation, hooks de gestionnaires de paquets, manifestes et fichiers de
verrouillage, ainsi que les chemins d’exécution de l’application. Recherche
les téléchargements suivis d’exécution, commandes obfusquées, accès inexpliqués
aux identifiants ou aux fichiers personnels, transferts réseau inattendus et
mécanismes de persistance. Vérifie la provenance des dépendances et binaires
utilisés. Évalue chaque signal dans son contexte et cite le fichier et la ligne
quand elle est disponible.

Si tu détectes du code malveillant, une anomalie de sécurité sérieuse non
résolue ou un composant exécutable requis dont la provenance ou le contenu
ne peut pas être vérifié, bloque la compilation et la publication. Indique
le commit, les fichiers concernés, les éléments observés et ce qui permettrait
de reprendre. Si la revue ne révèle aucun indice bloquant, poursuis le build
sur la révision examinée. Vérifie aussi les changements avant chaque relance.

Dans le compte rendu, indique la révision examinée, la portée de la revue,
ses limites et les éléments suspects éventuels. Formule le résultat comme
« aucun indice de code malveillant détecté dans les éléments examinés »
lorsque c’est le cas.

## Création des dépôts

Quand je fournis un ZIP, un dossier accessible ou une URL de source,
importe le projet dans un nouveau dépôt GitHub privé sous Hylieno.
Utilise le nom que je donne ou dérive-le du projet. Si ce nom existe
déjà, choisis un nom distinct et indique-le dans le compte rendu.

Pour créer le dépôt, déclenche le workflow `Create private repository` du
dépôt `Hylieno/build-github` avec le nom et la description voulus. Surveille
le workflow jusqu’à sa réussite, puis utilise le nouveau dépôt. N’essaie pas
de créer un dépôt directement avec les identifiants de l’environnement Codex.

Inspecte le README, la licence, les manifests, les scripts de build,
les workflows d’origine, les sous-modules et les dépendances. Préserve
la licence et les avis d’origine. Note l’URL et la révision de la source
si elles sont connues. Garde hors du dépôt les secrets et les fichiers
de configuration qui contiennent des identifiants.

Choisis les outils et les commandes à partir du projet et de la cible.
Inspecte les workflows importés avant toute exécution. Utilise un runner
GitHub hébergé adapté à Windows x64 ou à la cible demandée, avec un jeton en
lecture seule et sans le jeton de création des dépôts. Ajoute un workflow
déclenchable manuellement. Fais publier la release par un job distinct qui
reçoit les artefacts du build et dispose de `contents: write`. N’exécute pas
le code importé dans ce job de publication.

Déclenche le workflow, surveille son exécution, lis les logs en cas
d’échec, corrige les problèmes de configuration et relance au besoin.
Publie la release seulement après un build réussi et après avoir
vérifié que les fichiers attendus sont présents. Si une dépendance
privée, un SDK ou une signature manque, indique exactement le blocage.

Termine en donnant les liens du dépôt, du run GitHub Actions et de la
release, ainsi que les noms des fichiers téléchargeables.
