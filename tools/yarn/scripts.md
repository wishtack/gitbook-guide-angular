# Scripts

## Comment ?

La section `scripts` du fichier `package.json` permet tout simplement de définir des tâches utilisées par les développeurs ou pour l'automatisation _\(intégration continue etc...\)._

```javascript
{
    "scripts": {
        "deploy": "tools/deploy.sh",
        "hello": "cowsay 👋",
        "start": "webpack-dev-server"
    },
    "devDependencies": {
        "cowsay": "*",
        "webpack-dev-server": "*"
    }
}
```

Il suffit ensuite d'exécuter yarn en lui indiquant le script à exécuter :

```bash
yarn hello
```

et on obtient le résultat suivant :

```bash
yarn run v1.6.0
$ cowsay 👋
 ___
< 👋 >
 ---
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
✨  Done in 0.16s.
```

## Pourquoi ?

### Centraliser et uniformiser

La section `scripts` est utilisée comme point d'entrée commun pour toute l'équipe et les outils d'automatisation afin d'uniformiser les commandes utilisées et de centraliser l'information.

### Utiliser les dépendances locales

En lançant la commande `yarn start`, Yarn va d'abord rechercher la commande `webpack-dev-server` dans le dossier `node_modules` local _\(plus exactement, le dossier `node_modules/.bin`\)_ avant d'utiliser les commandes globalement installées sur la machine.

Le but est d'éviter les dépendances globales et donc l'hétérogénéité des versions.

Les développeurs et outils d'automatisation n'ont donc besoin que de deux prérequis : NodeJS et Yarn.

### Automatiser

Avant et après l'exécution de `yarn install`, Yarn lance respectivement les scripts `preinstall` et `postinstall` qui permettent donc d'enrichir la phase d'installation afin d'installer par exemple les dépendances d'autres langages et frameworks.

Si vous déployez votre application sur Heroku [https://www.heroku.com/](https://www.heroku.com/), Heroku détecte la présence du fichier `package.json`, lance la commande `yarn install` puis la commande `yarn heroku-postbuild` qui vous permet de personnaliser le build de votre application.

## yarn run vs npm run

Pour exécuter une commande avec yarn et lui passer des options, il n'y a pas de surprise :

```bash
yarn hello -s # -s => stoned cow
```

```bash
yarn run v1.6.0
$ cowsay 👋 -s
 ___
< 👋 >
 ---
        \   ^__^
         \  (**)\_______
            (__)\       )\/\
             U  ||----w |
                ||     ||
✨  Done in 0.16s.
```

Cela exécute donc la commande `cowsay 👋 -s`.

En revanche, avec NPM, il faut lancer la commande `run` et malheureusement c'est NPM qui consomme les options et dans ce cas particulier, NPM interprète l'option `-s` qui dans son cas veut dire `--silent`.

```bash
npm run hello -s
```

```bash
 ___
< 👋 >
 ---
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

Pour passer des options, voici la syntaxe à utiliser :

```bash
npm run hello -- -s
```

```bash
> cowsay 👋 "-s"

 ___
< 👋 >
 ---
        \   ^__^
         \  (**)\_______
            (__)\       )\/\
             U  ||----w |
                ||     ||
```

{% hint style="info" %}
Donc pour résumer :

`yarn hello -s` &lt;=&gt; `npm run hello -- -s`

A vous de choisir.
{% endhint %}

{% hint style="danger" %}
Pour remédier à certains de ces problèmes et apporter de nouvelles fonctionnalités, l'équipe NPM a produit une nouvelle commande `npx` que l'on vous recommande d'éviter principalement pour des raisons de sécurité.  
**Le danger de cette commande est qu'elle installe automatiquement le module que vous lui passez en paramètre et l'exécute immédiatement.**

Une typo et vous êtes cuits si vous tombez sur module malveillant.

Exemple :

```bash
npx run hello
```

```bash
Error: Cannot find module './hello'
```

Fiouf ! Nous venons d'installer inconsciemment le module run \([https://yarnpkg.com/en/package/run](https://yarnpkg.com/en/package/run)\) qui a ensuite essayer d'exécuter un fichier hello de notre projet. 😱

> StackOverflow + Social Engineering = Remote Code Execution
{% endhint %}

