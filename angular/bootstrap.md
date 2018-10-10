# Bootstrap

## Prérequis

Installation des outils suivants :

{% page-ref page="../tools/git.md" %}

{% page-ref page="../tools/nodejs.md" %}

{% page-ref page="../tools/yarn/" %}

{% page-ref page="../tools/angular-cli.md" %}

{% page-ref page="../tools/intellij-webstorm-vscode/" %}

{% page-ref page="../tools/chrome.md" %}

## Création de l'application Angular

Pour créer une application Angular, nous utiliserons la commande Angular CLI : `ng`.

Il faut d'abord indiquer à Angular CLI que nous utiliserons Yarn plutôt que NPM.

```bash
ng config cli.packageManager yarn --global
```

{% hint style="danger" %}
En cas de problème avec cette commande, vous pouvez ignorer cette étape puis ajouter l'option `--skip-install` afin d'éviter l'installation avec npm puis installer les dépendances manuellement avec `yarn`.  
`ng new book-shop --prefix wt --skip-install && cd book-shop && yarn install`
{% endhint %}

La commande ci-dessous va créer un projet Angular _\(avec une application\)_ dans le dossier `book-shop`.

```bash
ng new book-shop --prefix wt
```

{% hint style="info" %}
L'option --prefix permet d'indiquer le préfixe que nous utiliserons pour nos composants Angular \(entre autres\) afin de les reconnaître rapidement dans le code HTML \(e.g.: &lt;wt-book-list&gt;\).

Autrement, la valeur par défaut utilisée par Angular CLI est app  et notre composant se nommerait alors &lt;app-book-list&gt;. Ici, l'acronyme wt fait référence à Wishtack.
{% endhint %}

La commande `ng new` :

1. Crée le squelette du projet dans le dossier `book-shop` _\(`package.json`, `.gitignore`, code source, configurations etc...\)_
2. Installe implicitement à l'aide de la commande `yarn install` toutes les dépendances indiquées par défaut dans le fichier `package.json` _\(sauf si l'on ajoute l'option `--skip-install`\)_. 
3. Initialise un "repository" git et ajoute le fichier `yarn.lock` généré par l'étape précédente.

{% hint style="info" %}
Vous obtenez donc une application opérationnelle dont tous les fichiers ont été "commit" proprement.  
Il vous suffit alors d'ajouter un "remote" pour partager votre code source avec les copains.  
[https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)  
[https://help.github.com/articles/adding-a-remote/](https://help.github.com/articles/adding-a-remote/)
{% endhint %}



{% hint style="info" %}
Le module `@angular/cli` installé globalement n'est utile que pour la création de l'application.  
Pour la suite, le module `@angular/cli` sera installé localement dans chaque projet.

Une fois le projet initialisé, les développeurs n'ont plus besoin d'installer le module `@angular/cli` globalement.
{% endhint %}

## Développement

Il suffit maintenant d'ouvrir le projet sur votre IDE puis de lancer la commande suivante :

```bash
yarn start
```

... ou ajouter l'option `--open` pour ouvrir une fenêtre de votre navigateur sur l'application : [http://localhost:4200](http://localhost:4200)

```bash
yarn start --open
```

Cette commande est un "alias" pour `ng serve` qui lui-même lance un serveur de développement à l'aide de `webpack-dev-server` qui s'occupe principalement des tâches suivantes :

* **Watch et build avec webpack** : à chaque fois que vous modifiez le code source de votre application, webpack va détecter les changements et "rebuild" l'application.
* **Livereload** : à chaque "rebuild", webpack-dev-server communique avec vos fenêtres de navigation ouvertes sur l'application \(par websocket\) pour rafraîchir le contenu. C'est ainsi qu'à chaque changement, vous pouvez en observer le résultat quasi-immédiatement sur vos navigateurs.

 Vous pouvez également lancer l'application _\(ou n'importe quel script yarn\)_ directement depuis l'IDE.

![](../.gitbook/assets/intellij-yarn-start.gif)

Et c'est parti :

![](../.gitbook/assets/livereload.gif)

## Mise à jour

La commande update d'Angular CLI permet :

* de mettre à jour les dépendances de votre projet,
* d'adapter sa structure aux nouvelles version d'Angular CLI _\(e.g. : renommage et restructuration du fichier `.angular-cli.json` =&gt; `angular.json`\)_.
* à partir d'Angular 6, d'adapter votre code quand la mise à jour d'une dépendance le nécessite _\(e.g. : adaptations en fonction des mises à jour d'Angular Material\)_. 

```bash
yarn ng update
```

{% hint style="warning" %}
Nous utilisons la commande `yarn ng update` au lieu de `ng update` afin d'utiliser la version Angular CLI locale à votre projet plutôt que la version installée globalement sur votre environnement _\(si Angular CLI est installé globalement\)_. 
{% endhint %}

Cf. [https://update.angular.io](https://update.angular.io)

