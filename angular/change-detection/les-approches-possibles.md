# Les Approches Possibles

Pour synchroniser le modèle avec la vue, les approches les plus communes sont les suivantes :

## **Approche Impérative Explicite**

Les développeurs mettent à jour explicitement la vue. _\(e.g. : Vanilla JS / JQuery / ...\)_

```typescript
user.isDisabled = true;
document.querySelector('button').disabled = user.isDisabled;
```

| **Avantages** | **Inconvénients** |
| --- | --- | --- | --- | --- | --- |
| Simple et performant pour des cas **très basiques**. | Couplage fort avec la vue. |
| Pas besoin de framework. | Complexité. |
|  | Risques importants de désynchronisation du modèle avec la vue. |
|  | Problèmes de performances. |
|  | Bugs fréquents. |

## Approche Impérative Implicite

Les développeurs couplent la vue avec le modèle afin de mettre à jour cette dernière à chaque changement.

```typescript
export class Customer {
    
    set firstName(value) {
        this._firstName = value;
        this.notifyFirstNameChange(value);
    }
    
}

customer.subscribeToFirstNameChange(firstName => nameElement.textContent = firstName);
```

| **Avantages** | **Inconvénients** |
| --- | --- | --- | --- |
| Synchronisation du modèle et de la vue. | Complexité et comportement implicite. |
|  | Problèmes de performances garantis. |
|  | Risques de boucles infinies. |

## Approche Déclarative

Dans ce cas, ni le controller ni le modèle n'ont conscience de la vue.

En revanche, la vue sait quelles propriétés du modèle surveiller pour maintenir la vue à jour.

### Code du Template

```markup
<div>{{ user.firstName }}</div>
```

### Code Implicite

```typescript
refresh() {

    /* No change. */
    if (lastUserFirstName === user.firstName) {
        return;
    }
    
    /* Update view. */
    divElement.textContent = lastUserFirstName = user.firstName;
    
}
```

| **Avantages** | **Inconvénients** |
| --- | --- | --- | --- |
| Simplicité. | Gourmand en performance sans optimisations. |
| Couplage faible avec la vue. |  |
| Contrôle total. |  |

### Mais comment ?

... mais **combien y a-t-il de fonctions refresh**, **qui les appelle** et **quand** ?

