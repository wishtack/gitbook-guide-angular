# Test-Driven Development

## Pourquoi ?

L'approche T.D.D. _\(Test-Driven Development\)_ consiste à **implémenter les tests en premier**.

Cela offre les avantages suivants :

* Lors de l'implémentation du test, **on se concentre sur la fonctionnalité** et son utilisation **plutôt que sur les contraintes liées à son développement**. Autrement dit, **on se concentre sur ce que l'on veut** faire **plutôt que sur ce que l'on peut** faire. Cela évite par exemple l'utilisation de librairies inadaptées. 
* Le T.D.D. **encourage naturellement** l'adoption d'**architectures simples, modulaires et découplées** afin de **simplifier l'implémentation des tests**. On obtient alors une meilleure architecture et un code moins monolithique et plus facile à maintenir. 
* Les interfaces **peuvent être générées** à partir des tests. 
* Quand les tests passent, on sait que la fonctionnalité est opérationnelle. 
* Le développement est **plus rapide** car la vérification des résultats est instantanée et automatique.

## Le "Test-Driven Development" Etape par Etape

### 1. Définition du Test

Grâce à la fonction `xit` de [Jasmine](jasmine.md), on peut décrire une "spec" et l'exclure tant que la fonctionnalité n'est pas implémentée.

```typescript
describe('Calculator', () => {

    xit('should evaluate 2 + 3 + 4 to 9', () => {
        // @TODO implement this.
    });

});
```

{% hint style="info" %}
**Ce code peut être "commit" et "released".**
{% endhint %}

![Skipped Test](../../../.gitbook/assets/karma-output-console-skipped.png)

{% hint style="warning" %}
Les tests désactivés sont affichés sur les rapports.
{% endhint %}

{% hint style="success" %}
Par précaution, nous pouvons lever une exception dans le test pour éviter qu'il ne soit activé par erreur et que cela donne l'impression d'un test qui fonctionne.

```typescript
describe('Calculator', () => {

    xit('should evaluate 2 + 3 + 4 to 9', () => {
        throw new NotImplementedError();
    });

});
```

La classe `NotImplementedError` peut être implémentée ainsi.

```typescript
export class Exception {

    constructor(private _message: string) {
    }

    toString() {
        return this._message;
    }

}

export class NotImplementedError extends Exception {

    constructor() {
        super('Not implemented!');
    }

}
```
{% endhint %}

### 2. Implémentation du Test

{% tabs %}
{% tab title="calculator.spec.ts" %}
```typescript
describe('Calculator', () => {

    it('should evaluate 2 + 3 + 4 to 9', () => {

        const calculator = new Calculator();

        expect(calculator.evaluate('2 + 3 + 4')).toEqual(9);

    });

});
```
{% endtab %}
{% endtabs %}

**Un test désactivé sera** tout de même **compilé** par TypeScript.

Grâce à l'IDE, on peut générer les classes et fonctions sans les implémenter.

![Create Class](../../../.gitbook/assets/intellij-create-class.png)

![Generate Method](../../../.gitbook/assets/intellij-generate-method.gif)

{% tabs %}
{% tab title="calculator.ts" %}
```typescript
export class Calculator {

    evaluate(expression: string): number {
        throw new NotImplementedError();
    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Ce code peut être "commit" et "released".**

Par précaution, vous pouvez toujours ajouter une annotation `@deprecated: Work in progress` dans les commentaires de la méthode.
{% endhint %}

###  3. Implémentation de la Fonctionnalité

Avant d'implémenter la fonctionnalité, il faut lancer les tests unitaires et effectuer les modifications progressivement jusqu'à ce que tous les tests passent.

{% hint style="info" %}
Le code en cours de développement peut également être "commit" et "released" à condition de désactiver les tests associés aux fonctionnalités en cours de développement et de **ne pas impacter les tests existants**.
{% endhint %}

{% tabs %}
{% tab title="calculator.ts" %}
```typescript
export class Calculator {

    evaluate(expression: string): number {

        return expression.split('+')
            .map(rawValue => parseInt(rawValue, 10))
            .reduce((result, value) => result + value, 0);

    }

}
```
{% endtab %}
{% endtabs %}

