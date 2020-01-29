# Tree-Shakable Services

Pour éviter les problèmes décrits précédemment _\(Cf._ [_Portée des Services_](portee-des-services.md)_\)_ et les solutions complexes associées, **depuis Angular 6, il n'est plus nécessaire de définir les "providers" au niveau des modules**.

## Bye `providers`, welcome `providedIn`!

Désormais, les définitions du "provider" et de la portée du service peuvent se faire directement au niveau du décorateur `@Injectable` du service.

Il suffit alors de déclarer un service de cette façon :

{% tabs %}
{% tab title="book-repository.ts" %}
```typescript
@Injectable({
    providedIn: 'root'
})
export class BookRepository {
}
```
{% endtab %}
{% endtabs %}

... pour pouvoir ensuite l'injecter partout dans l'application.

{% hint style="info" %}
`providedIn: 'root'` indique que le provider est ajouter au "root injector".

Il est également possible de sélectionner un module _\(`providedIn: BookCoreModule`\)_ pour que le service ne soit disponible que si le module associé est importé.
{% endhint %}

Il est ensuite possible de personnaliser l'instanciation avec `useValue`, `useClass`, `useFactory` etc...

{% tabs %}
{% tab title="book-repository.ts" %}
```typescript
@Injectable({
    providedIn: 'root',
    useFactory: () => {
        return new BookRepository();
    }
})
export class BookRepository {
}
```
{% endtab %}
{% endtabs %}

## Avantages

Cette approche a pour avantage :

* d'être plus simple et rapide à implémenter,
* de ne plus avoir à mettre tous les services dans des modules _\(historiquement, on finissait souvent avec un service / un module\),_
* de **ne charger le code des services qu'à la première injection** _**\(lazy loading\)**_.
* et surtout de permettre le **Tree-Shaking des services non utilisés**.

## Tree Shaking

Grâce à cette approche, il n'est plus nécessaire de charger le code des services dès le chargement de l'application et par conséquent **si le service n'est jamais utilisé, le code du service sera entièrement retiré du build final de l'application**.

