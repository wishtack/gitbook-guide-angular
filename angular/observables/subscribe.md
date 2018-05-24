# Subscribe

Dans l'exemple ci-dessous nous allons créer un observable à l'aide de la fonction `interval` qui produit une **valeur auto-incrémentée de façon régulière**. 

```typescript
import { interval } from 'rxjs';

const data$ = interval(1000);
```

Tant qu'on ne s'inscrit pas à l'"observable", il ne se passe rien car cet observable est "lazy“.

{% hint style="success" %}
La convention de nommage est de suffixer les "observables" avec `$`.
{% endhint %}

## `Observable.subscribe`

Le point commun entre tous les "observables" est la méthode `subscribe` qui permet de **souscrire à un `Observable`** et être **notifié des nouvelles valeurs**, des **erreurs** ou de la **fin du "stream"**.

```typescript
import { interval } from 'rxjs';
​
const data$ = interval(1000);

data$.subscribe(value => console.log(value));
```

![R&#xE9;sultat du \`subscribe\`](../../.gitbook/assets/observable-subscribe.gif)

Nous pouvons ensuite ajouter les "callbacks" de capture d'"erreur" ou de fin du "stream" en passant un objet à la méthode subscribe avec les trois callbacks suivantes : `next`, `error` et `complete`.

```typescript
data$.subscribe({
    next: value => console.log(value),
    error: err => console.error(err),
    complete: () => console.log('DONE!')
});
```



