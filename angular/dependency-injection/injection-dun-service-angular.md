# Injection d'un Service Angular

## Qu'est-ce qu'un service Angular ?

Avec Angular, **une dépendance** est généralement l'instance d'une classe permettant de **factoriser certaines fonctionnalités** ou d'**accéder à un état** permettant ainsi aux composants de communiquer entre eux.

Dans le vocabulaire Angular, ces classes sont appelées "**services"**.

Les services sont le plus souvent **des singletons**. Cf. [Portée des Services](portee-des-services.md).

## Injection d'un service Angular

Un service Angular peut être injecté par n'importe quelle classe Angular _\(i.e. : composant,_ [_Directive_](../directives/)_,_ [_Service_](services-and-providers.md) _ou_ [_Pipe_](../pipes.md)_\)_ via les paramètres de son constructeur.

{% tabs %}
{% tab title="book-preview.component.ts" %}
```typescript
@Component({
    ...
})
export class BookPreviewComponent {

    constructor(private _httpClient: HttpClient) {
    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Vous remarquerez l'utilisation des [TypeScript Parameter Properties](../../typescript/typing-des-proprietes.md#raccourci-pour-les-parametres-ordonnees-du-constructeur) afin de copier le service `HttpClient` dans la propriété `_httpClient`.
{% endhint %}

