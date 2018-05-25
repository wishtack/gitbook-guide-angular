# Unit-Test Synchrone

## Ajout d'un Test

Pour ajouter un test, il suffit de créer un fichier avec l'extension `.spec.ts` dans le dossier `src`. Plus exactement, **la convention est de créer ce fichier dans le même dossier que le fichier contenant le code source à tester**.

Il suffit d'utiliser les 3 fonctions suivantes pour implémenter un premier test : 

* `describe` : pour définir une suite _\(ou groupe\)_ de "specs".
* `it` : pour définir une "spec" _\(ou un test\)_.
* `expect` : pour implémenter les assertions.

{% code-tabs %}
{% code-tabs-item title="calculator.spec.ts" %}
```typescript
import { Calculator } from './calculator';

describe('Calculator', () => {

    it('should evaluate 2 + 3 + 4 to 9', () => {

        const calculator = new Calculator();

        expect(calculator.evaluate('2 + 3 + 4')).toEqual(9);

    });

});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Output Console](../../../.gitbook/assets/karma-output-console.png)

![Output HTML](../../../.gitbook/assets/karma-output-html.png)

## L'approche T.D.D. _\(Test-Driven Development\)_

### 1. Définition du test



