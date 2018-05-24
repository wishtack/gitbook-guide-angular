# Entry Point

Le point d'entrée d'une application Angular est situé par défaut dans le fichier `src/main.ts` _\(configurable dans le fichier_ `angular.json`_\)_.

Grâce à Webpack, Angular suit les imports de fichiers à partir de ce point d'entrée pour construire les "bundles" JavaScript qui seront chargés dans le fichier `index.html` produit par le build.

`yarn build` =&gt; `ng build` =&gt; `webpack` =&gt; `dist/*`

{% embed data="{\"url\":\"https://webpack.js.org/\",\"type\":\"link\",\"title\":\"webpack\",\"description\":\"webpack is a module bundler. Its main purpose is to bundle JavaScript files for usage in a browser, yet it is also capable of transforming, bundling, or packaging just about any resource or asset.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://webpack.js.org/assets/favicon.ico\",\"aspectRatio\":0}}" %}

![Webpack](../../.gitbook/assets/webpack.png)

