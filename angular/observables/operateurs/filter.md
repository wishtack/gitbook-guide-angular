# filter

L'opérateur `filter` permet de ne garder que les éléments pour lesquels la fonction `predicate` retourne `true`.

```typescript
import { from } from 'rxjs';
import { filter } from 'rxjs/operators';
​
const getTemperature = city => 100 / city.length;
​
/* Create an Observable from an array : Strasbourg => Nice => Lyon. */
const city$ = from(['Strasbourg', 'Nice', 'Lyon']);
​
const hotCity$ = city$
    .pipe(filter(city => getTemperature(city) > 20));
​
hotCity$.subscribe(city => console.log(city));
```

Résultat :

```bash
Nice
Lyon
```

