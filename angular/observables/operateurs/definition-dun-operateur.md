# Définition d'un Opérateur

RxJS fournit nativement plus d'une centaine d'opérateurs.

Un opérateur permet de définir un `Observable` à partir d'un autre en y **appliquant quelques transformations**.

Les opérateurs sont des fonctions que l'on peut appliquer à un `Observable` via la méthode `pipe`.

```typescript
import { interval } from 'rxjs';
import { map } from 'rxjs/operators';

const value$ = interval(1000);
const double$ = value$.pipe(map(value => value * 2));
```

