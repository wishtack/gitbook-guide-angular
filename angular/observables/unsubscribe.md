# Unsubscribe ⚠️

La méthode `subscribe` retourne un objet de type `Subscription`. 

```typescript
import { interval } from 'rxjs';
​
const data$ = interval(1000);

const subscription = data$.subscribe({
    next: value => console.log(value),
    error: error => console.error(error),
    complete: () => console.log('DONE!')
});
```

Cet  objet sert principalement à se désinscrire d'un `Observable`  via sa méthode `unsubscribe`.

```typescript
subscription.unsubscribe();
```

La méthode `unsubscribe` va donc :

* **désinscrire les "callbacks"** : `next`, `error` et  `complete`;
* **détruire l'`Observable`** _\(c'est à dire interrompre les traitements effectués par l'`Observable`\)_ si celui-ci est de type "**cold**" ;
* éventuellement **libérer la mémoire** car en désinscrivant les "callbacks", le "garbage collector" libérera la mémoire occupée par les objets référencés dans les "callbacks" _\(s'ils ne sont plus référencés ailleurs\)_.

{% hint style="danger" %}
Il est **important d'`unsubscribe`** les `Observable`s dès qu'il n'y en a plus besoin afin d'éviter les **fuites mémoire**, la **consommation inutile de CPU** et les **effets de bord** associés à ces `Observable`s "zombies".
{% endhint %}

L'`unsubscribe` se fait généralement :

* lors de la destruction d'un composant via le "lifecycle hook" **`ngOnDestroy`**,
* ou lorsqu'un `Observable` doit en remplacer un autre _\(e.g. : lancement d'une nouvelle recherche par un utilisateur ou rafraichissement du contenu\)_.

Dans certains cas, nous préférons l'utilisation du "pipe" `async` que nous verrons plus tard ou alors des "helpers" tels que celui-ci : [https://github.com/wishtack/wt-angular-auth-demo/blob/master/src/app/helpers/subscription-garbage-collector.ts](https://github.com/wishtack/wt-angular-auth-demo/blob/master/src/app/helpers/subscription-garbage-collector.ts)



