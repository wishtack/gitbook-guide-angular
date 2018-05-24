# Feature Module

Les composants _\(ainsi que les directives, pipes, etc...\)_ sont **regroupés dans des modules par fonctionnalité**. Ces modules sont alors appelés "**feature module"**.

Il existe 5 familles de "feature modules" :

## Domain Feature Module

Ce module met à dispositions des modules qui l'importent un composant de type container qui représente une fonctionnalité entière. Exemple :

```typescript
@NgModule({
    declarations: [
        BookSearchComponent,
        BookSearchFormComponent
    ],
    exports: [
        BookSearchComponent
    ],
    imports: [
        BookModule
    ]
})
export class BookSearchModule {
}
```

## Service Feature Module

Un module qui ne met à disposition que des services _\(e.g. : `HttpClientModule`\)_. Cf. [Portée des Services](../dependency-injection/portee-des-services.md).

## Widget Feature Module

Un module ne contenant quasiment que des composants _\(généralement de type "presentational component"\) \(e.g. `MaterialButtonModule`\)._

{% hint style="success" %}
A moins de produire une librairie utilisée par plusieurs équipes _\(librairie open-source\)_, il est préférable d'exporter tous les composants pour éviter les mauvaises surprises d'export manquant lors de leur utilisation par les copains.
{% endhint %}

## Routed Feature Module

Un module contenant des composants associés à des routes. Cf. [Routing](../routing/).

Les modules de ce type sont généralement [Lazy Loaded](../routing/lazy-loading.md).

## Routing Feature Module

Un simple module généralement associé à une [Routed Feature Module](feature-module.md#routed-feature-module) dont le but est simplement d'alléger la définition de ce dernier en prenant en charge la configuration du [routing](../routing/).

