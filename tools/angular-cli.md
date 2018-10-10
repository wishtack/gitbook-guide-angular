# Angular CLI

{% embed url="https://cli.angular.io/" caption="Angular CLI" %}

Angular CLI est un outil permettant de créer, construire, générer et tester vos applications et librairies Angular.

Sans Angular CLI, la création et la construction d'une application Angular nécessite l'utilisation et la maîtrise de nombreux outils : typescript, webpack, karma, protractor, istanbul etc...

> Exemple du package.json d'un boilerplate _\(squelette de projet\)_ avant Angular CLI  [https://github.com/gdi2290/angular-starter/blob/master/package.json](https://github.com/gdi2290/angular-starter/blob/master/package.json)

## Installation

La commande suivante installe le module `@angular/cli`.

{% hint style="info" %}
Les modules Angular officiels sont préfixés par `@angular/`.

Il s'agit d'un "scope" NPM. Cela nous garantit que seuls les administrateurs du groupe "angular" peuvent déployer des modules dans ce "scope" _\(avec idéalement deux facteurs d'authentification\)_.  
[https://docs.npmjs.com/misc/scope](https://docs.npmjs.com/misc/scope)  
[https://docs.npmjs.com/getting-started/using-two-factor-authentication](https://docs.npmjs.com/getting-started/using-two-factor-authentication)
{% endhint %}

```bash
yarn global add @angular/cli
```

L'installation de ce module mettra à votre disposition la commande `ng` qui vous permettra plus tard de créer votre application Angular.

## Documentation

La documentation d'Angular CLI est disponible sous forme de wiki [https://github.com/angular/angular-cli/wiki](https://github.com/angular/angular-cli/wiki)

## Schematics

La génération et mise à jour automatique du code fournie par Angular CLI se base sur l'outil Schematics qui permet également de définir nos propres "schematics". Ces "schematics" peuvent être vues comme des "recettes" qui pourront être utilisées en ligne de commande pour générer du code, le corriger ou le mettre à jour afin de respecter les derniers "breaking changes" ou "guidelines" du framework ou d'une librairie.

[https://blog.angular.io/schematics-an-introduction-dc1dfbc2a2b2](https://blog.angular.io/schematics-an-introduction-dc1dfbc2a2b2)

