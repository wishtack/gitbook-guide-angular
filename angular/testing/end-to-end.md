# End-to-End

## Pourquoi ?

Par définition, les tests unitaires ne testent pas les interactions entre les modules et certains effets de bord.

#### Il est important de tester automatiquement l'ensemble de l'application.

> Il n'est pas envisageable de ne pas tester ou d'effectuer tous les tests manuellement à chaque changement ou encore d'en sélectionner uniquement une partie.

## Protractor

**Protractor est un framework de tests e2e développé par l'équipe d'AngularJS**. Il permet à la fois de tester les applications AngularJS et Angular mais aussi des sites web non-Angular.

Protractor utilise [**Jasmine**](unit-testing/jasmine.md) **comme framework de tests** et d'assertion et le "webdriver" **Selenium pour communiquer avec les "browsers"**.

En plus de sa syntaxe simple, Protractor fournit des fonctionnalités supplémentaires spécifiques à Angular telles que l'"**Implicit Wait**" : il n'est pas nécessaire d'implémenter des temps d'attente ou des vérifications de présence d'éléments HTML. **Protractor attend l'arrêt de l'activité d'Angular pour évaluer l'étape suivante** du test.

## Ajout d'un test

Pour ajouter un test End-to-End, il suffit de créer un fichier  `.e2e-spec.ts` dans le dossier `e2e` et d'utiliser [Jasmine](unit-testing/jasmine.md) pour définir une suite de "specs".

{% code-tabs %}
{% code-tabs-item title="signin.e2e-spec.ts" %}
```typescript
describe('signin', () => {

    const signinPage = new SigninPage();

    it('should show an error message if credentials are invalid', async () => {
    
        await signinPage.signIn({
            userName: 'ninja',
            password: 'isnohype'
        });
        
        const errorMessage = await signinPage.getErrorMessage();
        
        expect(errorMessage).toEqual('Invalid credentials 😱.');
        
    });

});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## "Page Objects"

Une bonne pratique consiste à **factoriser les interactions avec une page** dans une classe dédiée _\(Separation of Concerns\)_. Ces classes sont des "**Page Objects**" prenant par convention l'extension `.po.ts`.

{% code-tabs %}
{% code-tabs-item title="signin.po.ts" %}
```typescript
import { browser, by, element } from 'protractor';

export class SigninPage {
    
    navigateTo() {
        return browser.get('/signin');
    }
    
    getErrorMessage() {
        return element(by.css('[data-error-message]')).getText();
    }
    
    async signIn({userName, password}) {
        await element(by.css('[data-username]')).sendKeys(userName);
        await element(by.css('[data-password]')).sendKeys(password);
        await element(by.css('[data-submit-button]')).click();
    }
    
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



