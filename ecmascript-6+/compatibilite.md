# Compatibilité

Comment faire tant que toutes les fonctionnalités ne sont pas disponibles sur nos navigateurs cibles ?

​[https://kangax.github.io/compat-table/es6/](https://kangax.github.io/compat-table/es6/)

{% embed data="{\"url\":\"https://kangax.github.io/compat-table/es6/\",\"type\":\"link\",\"title\":\"ECMAScript 6 compatibility table\",\"icon\":{\"type\":\"icon\",\"url\":\"https://kangax.github.io/compat-table/favicon.ico\",\"aspectRatio\":0}}" %}

## Transpilation

ECMAScript 5.1 est à l'ECMAScript moderne ce que l'assembleur est au C++.

Nous avons donc besoin de transpilateurs :

* Babel: [https://babeljs.io/](https://babeljs.io/)
* TypeScript: [https://www.typescriptlang.org/](https://www.typescriptlang.org/)

Mais cela n'est pas suffisant car la transpilation ne fait que convertir les nouvelles fonctionnalités syntaxiques.

## Polyfills

Nous avons besoin de compenser l'absence de certains objects, fonctions ou méthodes `customElements`, `fetch`, `Array.filter` sur certains navigateurs.

Pour remédier à cette limitation, nous utiliserons des librairies de polyfills. Ces librairies détectent l'absence des fonctionnalités et décident dans ce cas de les compenser avec une implémentation JavaScript.

Exemple :

```javascript
if (Array.prototype.first == null) {
    Array.prototype.first = function () {
        return this[0];
    };
}

const valueList = [1, 2, 3];

console.log(valueList.first()); // 1
```

L'une des librairies de polyfills les plus populaires est **core-js** [https://github.com/zloirock/core-js](https://github.com/zloirock/core-js).

Ou encore : [https://polyfill.io](https://polyfill.io/v2/docs/).

