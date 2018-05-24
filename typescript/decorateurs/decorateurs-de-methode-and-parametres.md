# Décorateurs de Méthode & Paramètres

Un décorateur de méthode permet d'en modifier le comportement par "wrapping".

Le décorateur en exemple ci-dessous n'opère aucun changement.

```typescript
const Noop = () => (target, key: string) => {
    return target[key];
};

class Calculator {

    @Noop()
    sum(a, b) {
        console.log('computing...');
        return a + b;
    }

}

const calculator = new Calculator();
```

{% hint style="warning" %}
Remarquez le pattern de "currying" très fréquent dans l'implémentation de décorateurs.

La syntaxe suivante :

```typescript
const Noop = () => {
    return (target, key) => {
        return target[key];
    };
}
```

... est identique à celle-ci :

```typescript
const Noop = () => (target, key) => {
    return target[key];
};
```

A consommer avec modération.
{% endhint %}



Les décorateurs de paramètres permettent principalement d'ajouter des metadata à la classe pour que les décorateurs de méthode puissent s'en servir.

## Mémorisation des résultats

Le décorateur ci-dessous construit progressivement un objet de mémorisation permettant de "mapper" les paramètres au dernier résultat obtenu afin d'éviter de refaire le même calcul inutilement.

```typescript
const Memoize = () => (target, key: string) => {

    const memory = {};

    const original = target[key];

    target[key] = function (...args) {

        /* Retrieve last returned value from memory if available. */
        const value = memory[args.toString()];

        if (value !== undefined) {
            return value;
        }

        const result = original.apply(this, args);

        memory[args.toString()] = result;

        return result;

    };

};

class Calculator {

    @Memoize()
    sum(a, b) {
        console.log('computing...');
        return a + b;
    }

}

const calculator = new Calculator();

console.log(calculator.sum(1, 2));
// computing...
// 3
console.log(calculator.sum(1, 2));
// 3
console.log(calculator.sum(1, 2));
// 3
console.log(calculator.sum(2, 2));
// computing...
// 4
console.log(calculator.sum(1, 2));
// 3
```

## Contract checking en runtime

```typescript
import 'reflect-metadata';

const _contractDictMetadataKey = '__contractDict';

const ApplyContracts = () => (target, key: string) => {

    const originalMethod = target[key];
    const contractDict = Reflect.getOwnMetadata(_contractDictMetadataKey, target, key);

    target[key] = function (...argList) {

        if (contractDict !== undefined) {

            argList.forEach((value, index) => {

                const contract = contractDict[index];

                if (contract === undefined) {
                    return;
                }

                if (!contract(value)) {
                    throw new Error(`Value '${value}' does not respect contract for \`${target.constructor.name}.${key}(param_${index})\`.`);
                }

            });

        }

        return originalMethod(...argList);

    }

};

const Contract = (contract: (value: any) => boolean) => (target, key, index): any => {

    let contractDict = Reflect.getOwnMetadata(_contractDictMetadataKey, target, key);

    if (contractDict === undefined) {
        contractDict = {};
    }

    contractDict[index] = contract;

    Reflect.defineMetadata(_contractDictMetadataKey, contractDict, target, key);

};

const isPositive = value => value > 0;

class Utils {

    @ApplyContracts()
    double(@Contract(isPositive) number: number): number {
        return number * 2;
    }

}

console.log(new Utils().double(2)); // 4

// Error: Value '0' does not respect contract for `Utils.double(param_0)`.
console.log(new Utils().double(0));                
```

