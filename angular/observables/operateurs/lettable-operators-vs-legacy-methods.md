# Lettable Operators vs Legacy Methods

Les opérateurs de type fonction sont appelés "**lettable operators**". Ils ont été introduit avec la version RxJS 5.5.

Avant les "lettable operators", les opérateurs étaient de méthodes de la classe `Observable` qu'il fallait ajouter par "polyfill" pour éviter de surcharger le code généré par l'application.

```typescript
import 'rxjs/add/operators/map';

data$.map(value => value * 2);
```

L'approche à base de méthodes et de "polyfill" a été retiré depuis RxJS 6.0 _\(sorti quasi simultanément que Angular 6\)_.

Si vous avez du "code legacy", vous pouvez utiliser le module **`rxjs-compat`** [https://github.com/ReactiveX/rxjs/tree/master/compat](https://github.com/ReactiveX/rxjs/tree/master/compat) en attendant de procéder à la migration. 

