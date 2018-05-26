# Création d'un Observable

Il est rare de devoir créer un `Observable` "from scratch" car Angular fournit généralement des "wrappers" à base d'`Observable`s pour la plupart des sources de données asynchrones _\(http, forms, route changes etc...\)_ mais il est intéressant de s'y aventurer au moins une fois pour mieux en comprendre le fonctionnement.

## `Observer`

L'exemple suivant :

```typescript
import { Observable } from 'rxjs';

const data$ = new Observable(observer => {

    observer.next(1);
    observer.next(2);
    observer.next(3);
    observer.complete();

});

data$.subscribe({
    next: value => console.log(value),
    error: err => console.error(err),
    complete: () => console.log('DONE!')
});
```

... produit le résultat :

```bash
1
2
3
DONE!
```

Vous remarquerez quelques similitudes avec [la création d'une `Promise`](../callback-hell-vs.-promise-vs.-async-await/promise.md).  
Le duo de paramètres `resolve` et `reject` est remplacé par un objet `Observer` disposant des méthodes `next`,  `error`,  `complete`.

{% hint style="danger" %}
En omettant l'appel à la méthode `complete`, nous produisons un `Observable` infini et risquons des fuites mémoire si l'on oublie d'`unsubscribe`.
{% endhint %}

## Erreur

La méthode `error` permet de déclencher une erreur.

```typescript
import { Observable } from 'rxjs';

const data$ = new Observable(observer => {

    observer.next(1);
    observer.next(2);
    observer.error(new Error('Oups!'));
    observer.next(3);
    observer.complete();

});

data$.subscribe({
    next: value => console.log(value),
    error: error => console.error(error.toString()),
    complete: () => console.log('DONE!')
});
```

```bash
1
2
Error: Oups!
```

{% hint style="info" %}
Une fois les méthodes `error` ou `complete` appelées, les appels suivants aux méthodes `next`, `error` et `complete` sont simplement ignorés.
{% endhint %}

## "Teardown logic"

Voici un exemple d'implémentation de la fonction RxJS `interval` utilisée précédemment.

```typescript
import { Observable } from 'rxjs';

const interval = period => new Observable(observer => {

    let i = 0;

    setInterval(() => observer.next(i++), period);

});

const data$ = interval(1000);

const subscription = data$.subscribe({
    next: value => console.log(value),
    error: error => console.error(error.toString()),
    complete: () => console.log('DONE!')
});

/* Unsubscribe after 5 seconds. */
setTimeout(() => subscription.unsubscribe(), 5000);
```

Cela produit le résultat suivant :

![Observable Leak](../../.gitbook/assets/observable-leak.gif)

5 secondes après la "subscription", la "callback" `next` ne reçoit plus de valeurs. Par contre, NodeJS ne rend pas la main car le `setInterval` continue à tourner _\(et émettre des valeurs ignorées\)_ en tâche de fond malgré l'appel à `unsubscribe`. Il s'agit d'une fuite.

Pour remédier à ce problème, il faudrait appeler la fonction `clearInterval` dès que :

* une **erreur est détectée**,
* la méthode **`complete`** est appelée,
* le consommateur de l'`Observable` appelle la méthode **`unsubscribe`**.

Il faut donc implémenter une sorte de destructeur d'`Observable`.

Ce destructeur est appelé "Teardown Logic" et doit être retourné par la fonction de `subscribe` passé en paramètre au constructeur de l'`Observable`.

```typescript
import { Observable } from 'rxjs';
​
const interval = period => new Observable(observer => {
​
    let i = 0;
​
    const handler = setInterval(() => observer.next(i++), period);
​
    return () => clearInterval(handler);

});
​
const data$ = interval(1000);
​
const subscription = data$.subscribe({
    next: value => console.log(value),
    error: error => console.error(error.toString()),
    complete: () => console.log('DONE!')
});
​
/* Unsubscribe after 5 seconds. */
setTimeout(() => subscription.unsubscribe(), 5000);
```

Le problème est ainsi résolu car `clearInterval` sera appelé peu importe l'origine de l'interruption : `unsubscribe`, `error` ou `complete`.



