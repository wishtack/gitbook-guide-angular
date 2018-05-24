# Compatibilité de Librairies

ECMAScript6 a introduit les "promises" dont nous verrons le fonctionnement plus tard.  
L'idée globale des "promises" est de retourner de façon synchrone un "handler" sur lequel l'appelant s'inscrit \(grâce à la méthode `then`\) pour être informé quand le traitement asynchrone est terminé.

```typescript
const promise = ...;

promise.then(data => {
    console.log(`Woohoo! Data is finally ready: ${data}`);
});
```

En attendant ECMAScript 6, de nombreuses librairies ont dû produire leur propre implémentation des `Promise` : [kriskowal's q](https://github.com/kriskowal/q) / [AngularJS](https://docs.angularjs.org/api/ng/service/$q) / [jQuery](https://api.jquery.com/jquery.deferred/) / [Bluebird](http://bluebirdjs.com/docs/getting-started.html)...

Le problème des promises est qu'elles ont tendance à se propager dans les applications et à travers les librairies.

Dans une rigueur extrême de langage statiquement typé sans Duck Typing, il faudrait que toutes ces librairies s'accordent à implémenter une interface universelle commune... C'est une utopie dans un univers où la durée de vie moyenne d'une librairie est de quelques mois.

Modulo quelques variations, toutes les implémentations de "promises" implémentent au moins la méthode `then` \(et très souvent `catch`\).  
Grâce au Duck Typing, l'interface suivante est compatible avec toutes les implémentations :

```typescript
interface Thenable {
    then(callback): Thenable;
}
```



