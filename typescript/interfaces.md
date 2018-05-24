# Interfaces

Une interface TypeScript permet de définir la signature _\(ou le contrat\)_ d'une classe où même une fonction.

Les interfaces TypeScript ne sont utilisées que lors de la transpilation. Elle disparaissent totalement en runtime étant donné qu'elles ne contiennent pas de code.

## Class interface

```typescript
interface Serializable {
    serialize(): string;
}

class Customer implements Serializable {

    firstName: string;
    
    serialize() {
        return this.firstName;
    }
    
}

class Cache {

    setItem(key: string, value: Serializable) {
        ...
    }

}

const cache = new Cache();

cache.setItem('current-user', new Customer());
```

{% hint style="success" %}
Les "styles guides" TypeScript et Angular découragent l'utilisation du préfixe `I` pour les interfaces.

Cela est principalement lié au deux raisons suivantes :

* Au fil des refactorings, les classes deviennent des interfaces avec plusieurs implémentations.
* Grâce à l'injection de dépendance, nous travaillerons la plupart du temps avec des interfaces. Le code perd alors en lisibilité si tous les types sont préfixés par des `I`.
{% endhint %}

## Function interface

Il est également possible de définir des interfaces pour les fonctions.

Très pratique dans un environnement asynchrone où il pleut de la "callback".

```typescript
class Product {
    name: string;
    price: number;
}

interface ProductFilterCallback {
    (product: Product): boolean
}

class ProductRepository {

    getMatchingItemList(filter: ProductFilterCallback) {
        ...
    }

}

const productRepository = new ProductRepository();

productRepository.getMatchingItemList(product => product.price < 100);

// error TS2345: Argument of type '(product: Product) => number' is not assignable to parameter of type 'ProductFilterCallback'.
//  Type 'number' is not assignable to type 'boolean'.
productRepository.getMatchingItemList(product => product.price);
```

Fini le reverse engineering ou ça :

```typescript
doSomething(data => {
    console.log(data); // 🤞
});
```



