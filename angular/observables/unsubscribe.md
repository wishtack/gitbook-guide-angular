# Unsubscribe ⚠️

La méthode `subscribe` retourne un objet de type `Subscription`.

```typescript
import { interval } from 'rxjs';

const data$ = interval(1000);

const subscription = data$.subscribe({
    next: value => console.log(value),
    error: error => console.error(error),
    complete: () => console.log('DONE!')
});
```

Cet objet sert principalement à se désinscrire d'un `Observable` via sa méthode `unsubscribe`.

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

Dans certains cas, nous préférons l'utilisation du "pipe" `async` que nous verrons plus tard ou alors des "helpers" tels que **Rx-Scavenger**.

## Rx-Scavenger

[https://medium.com/@wishtack/rx-scavenger-the-rxjs-garbage-collector-b050099a00ea](https://medium.com/@wishtack/rx-scavenger-the-rxjs-garbage-collector-b050099a00ea)

{% embed url="https://medium.com/@wishtack/rx-scavenger-the-rxjs-garbage-collector-b050099a00ea" caption="" %}

[Rx-Scavenger](https://github.com/wishtack/wishtack-steroids/tree/master/packages/rx-scavenger) est un "Garbage Collector" de Subscription RxJS pour Angular.

[https://github.com/wishtack/wishtack-steroids/tree/master/packages/rx-scavenger](https://github.com/wishtack/wishtack-steroids/tree/master/packages/rx-scavenger)

Il permet d'unsubscribe automatiquement au remplacement d'une subscription ou à la destruction d'un composant.

```bash
yarn add @wishtack/rx-scavenger
```

```typescript
import { Scavenger } from '@wishtack/rx-scavenger';

@Component({
    ...
})
export class WeatherComponent implements OnDestroy, OnInit {

    private _scavenger = new Scavenger(this);

    constructor(private _weatherStation: WeatherStation) {
    }

    ngOnInit() {

        this._weatherStation
            .getWeather('Lyon')
            .pipe(
                this._scavenger.collect()        
            )
            .subscribe(weather => {
                ...
            });

    }

    ngOnDestroy() {
    }

}
```

