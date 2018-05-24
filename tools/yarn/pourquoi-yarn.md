# Pourquoi Yarn ?

## Stabilité

En cas d'anomalie _\(interruption lors de l'installation par exemple\)_, vos dépendances seront toujours dans un état stable et il suffit de relancer une commande pour reprendre l'installation.

Avec NPM, il arrivait souvent qu'une interruption corrompe les dépendances. Il fallait alors supprimer toutes les dépendances et relancer l'installation... parfois après quelques heures de diagnostic.

## Sécurité

Malheureusement, les modules publiés sur NPM registry \([https://www.npmjs.com/](https://www.npmjs.com/)\) ne sont pas signés.

Yarn intègre une solution "best effort" qui consiste à retenir les "checksum" des modules téléchargés. Ainsi, si jamais le contenu du module est modifié par malveillance ou par erreur, Yarn pourra détecter l'anomalie... à condition d'avoir récupéré ce module avec la même version précédemment.

## Résilience et mode "offline"

Si une dépendance est déjà disponible dans le cache de votre machine, elle sera utilisée par Yarn sans déclencher aucune connexion vers l'extérieur.

En cas d'erreur réseau, Yarn réessaie plusieurs fois tout en continuant l'installation des autres dépendances.

## Déterminisme

Un ancien problème très commun avec NPM est que les versions des dépendances indirectes _\(ou sous-dépendances\)_ ne sont pas maitrisées. A deux moments différents de la journée, deux développeurs pouvaient récupérer le même code source, installer les dépendances et obtenir des résultats différents.

Pour remédier à ce problème, à chaque installation de dépendances, Yarn génère un fichier `yarn.lock`  _\(une sorte de snapshot de toute votre arborescence de dépendances\)_ qu'il faut "commit" pour partager avec les autres développeurs et les outils. Lorsqu'on exécute la commande `yarn` ou `yarn install`, Yarn installe les dépendances directes et indirectes en se basant sur les informations du fichier `yarn.lock`.

Pour mettre à jour les dépendances, il suffit de lancer la commande `yarn upgrade` et le fichier `yarn.lock` sera mis à jour avec les dernières dépendances.

Avant l'apparition de Yarn, NPM proposait la commande `npm shrinkwrap` \([https://docs.npmjs.com/cli/shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap)\) mais le résultat n'était pas déterministe.

NPM génère désormais _\(depuis la version 5\)_ un fichier `package-lock.json` équivalent au `yarn.lock`.

## Performance

Grâce à la parallélisation des téléchargements, le pool de connexions, le caching etc... Yarn est capable d'installer très rapidement l'ensemble des dépendances _\(bien plus vite qu'NPM...\)_.

