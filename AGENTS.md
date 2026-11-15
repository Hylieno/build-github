# Build GitHub

## Fonctionnement automatique

Un fichier source joint, un dossier accessible ou une URL vers du code source
constitue à lui seul une demande complète de traitement. Si l’utilisateur
indique un nom de dépôt, utilise ce nom. Sinon, dérive un nom clair du projet.
Ne demande aucune confirmation lorsque ces informations suffisent.

Exécute automatiquement l’ensemble du processus : récupérer et extraire la
source, créer le dépôt privé, importer les fichiers, analyser le système de
compilation, configurer GitHub Actions, compiler, corriger les erreurs de
configuration, relancer les workflows et publier une release avec les
résultats de compilation.

La cible par défaut est Windows x64. Utilise une autre plateforme, architecture,
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
