# Décorateurs de Propriété

Le code ci-dessous implémente et utilise un décorateur `ReadOnly` qui surcharge l'implémentation du "setter" de la propriété et déclenche automatiquement une exception en runtime.

`ReadOnly` est une factory qui retourne un décorateur.

Le décorateur prend deux paramètres :

* `target` : la référence de la classe.
* `key` : le nom de la propriété.

```typescript
const ReadOnly = () => {
    
    /* Return the decorator. */
    return (target, key: string) => {
        Object.defineProperty(target, key, {
            set: () => {
                throw new Error(`${target.constructor.name}.${key} is read only`);
            }
        });
    };
    
};

class Customer {
    @ReadOnly() firstName: string;
}

const customer = new Customer();
customer.firstName = 'Foo';
```

Depuis la version 2.0 de TypeScript, il est désormais possible d'indiquer si une propriété est `readonly` avec la syntaxe native suivante :

```typescript
class Customer {
    readonly firstName: string;
}
```

{% hint style="warning" %}
Il faut bien noter la différence entre les deux.

La visibilité readonly n'est analysée que statiquement et peut donner une fausse impression de confiance.

En revanche, le décorateur est exécuté en runtime et il faudra penser à le désactiver en production pour éviter son coût d'exécution.
{% endhint %}

