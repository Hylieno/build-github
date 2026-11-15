# Build GitHub

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

Choisis le système, l’architecture, les outils et les commandes à partir
du projet et de la cible demandée. Inspecte les workflows importés avant
toute exécution. Utilise un runner GitHub hébergé pour le build,
avec un jeton en lecture seule et sans le jeton de création des dépôts.
Ajoute un workflow déclenchable manuellement. Fais publier la release
par un job distinct qui reçoit les artefacts du build et dispose de
`contents: write`. N’exécute pas le code importé dans ce job de publication.

Déclenche le workflow, surveille son exécution, lis les logs en cas
d’échec, corrige les problèmes de configuration et relance au besoin.
Publie la release seulement après un build réussi et après avoir
vérifié que les fichiers attendus sont présents. Si une dépendance
privée, un SDK ou une signature manque, indique exactement le blocage.

Termine en donnant les liens du dépôt, du run GitHub Actions et de la
release, ainsi que les noms des fichiers téléchargeables.
