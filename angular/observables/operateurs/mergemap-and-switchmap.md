# mergeMap & switchMap

`mergeMap` et `switchMap` sont deux opérateurs similaires à l'opérateur [`map`](map.md) mais au lieu de retourner une nouvelle valeur de façon synchrone pour chaque valeur de l'`Observable` d'origine, ces opérateurs permettent de **retourner un `Observable` pour chacune des valeurs de l'`Observable` d'origine**.

Supposons les deux fonctions suivantes qui produisent respectivement un `Observable` indiquant la ville actuelle puis un `Observable` indiquant la température actuelle de la ville donnée en paramètre.

```typescript
import { from, interval, mergeMap, switchMap, zip } from 'rxjs';
import { map } from 'rxjs/operators';

const getCurrentCity = () => {
    /* Produce one value from the array every second. */
    return zip(
        from(['Strasbourg', 'Paris', 'Lyon']),
        interval(1000)
    )
        .pipe(map(([city]) => city));
};

const getTemperature = city => {
    /* Produce the same temperature per city every 500ms. */
    return interval(500)
        .pipe(map(() => 100 / city.length));
};
```

Le code suivant :

```typescript
getCurrrentCity().subscribe(city => console.log(city));
```

... produit donc le résultat :

```bash
# at 1s
Strasbourg 
# at 2s
Paris
# at 3s
Lyon
```

Et le code suivant :

```typescript
getTemperature('Lyon').subscribe(temperature => console.log(temperature));
```

... produit :

```bash
25 # at  400ms
25 # at  800ms
25 # at 1200ms 
# ... same value every 400ms forever.
```

## `mergeMap`

L'opérateur `mergeMap` produit un `Observable` pour chaque valeur de l'`Observable` d'origine. Les `Observable`s ainsi obtenus sont fusionnés \(_merge_\).

```typescript
const currentCityTemperature$ = getCurrentCity()
    .pipe(mergeMap(city => getTemperature(city)));

currentCityTemperature$
    .subscribe(temperature => console.log(temperature));
```

On obtient le résultat suivant :

```bash
# at 1000ms, we're in Strasbourg.
10 # at 1400ms, Strasbourg's temperature.
10 # at 1800ms, Strasbourg's temperature.
# at 2000ms, we're in Paris.
10 # at 2200ms, Strasbourg's temperature. Still?
20 # at 2400ms, Paris' temperature.
10 # at 2600ms, Strasbourg's temperature. Really?
20 # at 2800ms, Paris' temperature.
# at 3000ms, we're in Lyon.
10 # at 3000ms, Strasbourg's temperature.
20 # at 3200ms, Paris' temperature.
25 # at 3400ms, Lyon's temperature.
10 # at 3400ms, Strasbourg's temperature.
20 # at 3600ms, Paris' temperature.
25 # at 3800ms, Lyon's temperature.
10 # at 3800ms, Strasbourg's temperature.
20 # at 4000ms, Paris' temperature.
25 # at 4200ms, Lyon's temperature.
10 # at 4200ms, Strasbourg's temperature.
...
```

Dans cet exemple, on peut constater que ce n'est pas tout à fait l'opérateur qu'il nous faut car quand la valeur de la ville change, nous souhaitons interrompre l'`Observable` de température associé et passer à l'`Observable` de température de la nouvelle ville.

{% hint style="warning" %}
Dans la plupart des cas, nous préférerons utiliser `switchMap` à `mergeMap` pour éviter le comportement décrit ci-dessous.

Toutefois, il existe des "use cases" où il faut absolument préférer `mergeMap` à `switchMap` pour éviter d'annuler certains traitements en cours lorsqu'une nouvelle valeur est produite.
{% endhint %}

## `switchMap`

Contrairement à `mergeMap`, quand l'`Observable` d'origine produit une nouvelle valeur, `switchMap` va `unsubscribe` de l'`Observable` produit par la valeur précédente avant de `subscribe` à l'`Observable` produit par la nouvelle valeur.

Voici le même exemple que le précédent mais avec `switchMap` cette fois-ci.

```typescript
const currentCityTemperature$ = getCurrentCity()
    .pipe(switchMap(city => getTemperature(city)));
​
currentCityTemperature$
    .subscribe(temperature => console.log(temperature));
```

Le résultat devient :

```bash
# at 1000ms, we're in Strasbourg.
10 # at 1400ms, Strasbourg's temperature.
10 # at 1800ms, Strasbourg's temperature.
# at 2000ms, we're in Paris.
20 # at 2400ms, Paris' temperature.
20 # at 2800ms, Paris' temperature.
# at 3000ms, we're in Lyon.
25 # at 3400ms, Lyon's temperature.
25 # at 3800ms, Lyon's temperature.
25 # at 4200ms, Lyon's temperature.
...
```



