# Unit-Testing

## Configuration

Grâce à [Angular CLI](../../tools/angular-cli.md), **tous les outils** nécessaires à l'implémentation et l'exécution des tests unitaires **sont installés et pré-configurés** dès la création de l'application. Cf. fichier `karma.conf.js`.

## Exécution des tests

La commande **`yarn test`** permet de déclencher la commande [Angular CLI](../../tools/angular-cli.md) **`ng test --watch`**.

{% hint style="info" %}
L'option `--watch` permet de relancer les tests unitaires **à chaque changement dans le code**.

Cela permet de **s'assurer en temps réel** que les développements en cours n'ont **pas d'effets négatifs sur les tests existants** et également de savoir **quand la fonctionnalité est opérationnelle**.

Pour lancer les tests sur un environnement d'**intégration continue**, pensez à ajouter une commande dédiée dans les `scripts` du `package.json` :

```javascript
"scripts": {
    "test": "ng test --watch",
    "test:singlerun": "ng test"
}
```
{% endhint %}

La commande `ng test` lance les tests unitaires en utilisant [**Karma**](https://karma-runner.github.io/2.0/index.html) _\(ex testacular: Cf._ [_https://github.com/karma-runner/karma/issues/376_](https://github.com/karma-runner/karma/issues/376)_\)_.

**Karma est un outil** permettant de **configurer** et d'**interconnecter** simplement les différents éléments nécessaires à la mise en place de tests unitaires frontend : "**watch**" des changements, "**build**" des tests, **exécution** des tests sur les "browsers", "**debug**", "**reporting**", "**code coverage**" etc...

Au lancement des tests :

1. Karma **déclenche un serveur** permettant de communiquer avec les "browsers" _\(généralement sur :_ [_http://localhost:9876_](http://localhost:9876)_\)_,
2. Karma **déclenche les "browsers" demandés** qui se connectent sur le serveur Karma,
3. Karma "**watch**" les changements de **code source**,
4. à chaque changement, Karma **déclenche le "build"** du code source modifié,
5. à chaque build, **Karma émet un événement aux "browsers"** connectés par WebSocket pour les informer du changement et **demander l'exécution des tests**,
6. les tests sont exécutés sur le "browser" et les résultats sont transmis au serveur Karma,
7. Karma **produit les rapports** en fonction des plugins activés : console, HTML, coverage etc...





