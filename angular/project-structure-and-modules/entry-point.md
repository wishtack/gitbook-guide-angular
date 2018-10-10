# Entry Point

Le point d'entrée d'une application Angular est situé par défaut dans le fichier `src/main.ts` _\(configurable dans le fichier_ `angular.json`_\)_.

Grâce à Webpack, Angular suit les imports de fichiers à partir de ce point d'entrée pour construire les "bundles" JavaScript qui seront chargés dans le fichier `index.html` produit par le build.

`yarn build` =&gt; `ng build` =&gt; `webpack` =&gt; `dist/*`

{% embed url="https://webpack.js.org/" %}

![Webpack](../../.gitbook/assets/webpack.png)

