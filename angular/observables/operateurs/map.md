# map

L'opérateur `map` permet de créer un nouvel `Observable` à partir de l'`Observable` d'origine en transformant simplement chacune de ses valeurs.

```typescript
import { from } from 'rxjs';
import { map } from 'rxjs/operators';

const getTemperature = city => 100 / city.length;

/* Create an Observable from an array : Strasbourg => Nice => Lyon. */
const city$ = from(['Strasbourg', 'Nice', 'Lyon']);

const temperature$ = city$
    .pipe(map(city => getTemperature(city)));

temperature$.subscribe(temperature => console.log(temperature));
```

Résultat :

```bash
10
25
25
```



