# Shared Module

Certains modules sont utilisés par quasiment tous les composants de l'application _\(e.g.: `CommonModule`, `FlexLayoutModule`, `RouterModule`\)_. Les importer dans chaque module peut s'avérer pénible.

Il est courant de factoriser ces imports en rassemblant toutes ces dépendances dans un module `SharedModule` importé par quasiment tous les autres modules de l'application.

{% code-tabs %}
{% code-tabs-item title="src/app/shared/shared.module.ts" %}
```typescript
@NgModule({
    imports: SharedModule.MODULE_LIST,
    exports: SharedModule.MODULE_LIST
})
export class SharedModule {

    static readonly MODULE_LIST = [
        CommonModule,
        FlexLayoutModule,
        RouterModule
    ];

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="danger" %}
Attention à ne pas trop surcharger ce module et en faire un "God Module".

N'y importez que les modules nécessaires pour quasiment tous les composants de l'application.
{% endhint %}

