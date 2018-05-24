# Structural Directive

Les "Structural Directives" permettent de **manipuler le DOM** en ajoutant ou en retirant des éléments.

Les "Structural Directives" natives les plus utilisées sont `ngIf` et `ngFor`.

Les "Structural Directives" sont généralement utilisées en préfixant les directives par un astérisque :

```markup
<h1 *ngIf="book != null">{{ book.title }}</h1>
```

Il s'agit d'un raccourci pour la syntaxe suivante :

```markup
<ng-template [ngIf]="book != null">
    <h1>{{ book.title }}</h1>
</ng-template>
```

Le tag `ng-template` est un tag particulier qui **n'affiche pas les éléments qui y sont contenus** mais les rend **disponible aux directives** appliquées dessus **par l'injection de la classe `TemplateRef`**.

La directive `ngIf` a donc accès au template en injectant la classe `TemplateRef` ; elle décide ensuite d'insérer ou non ce template dans la vue en fonction de la valeur de l'expression `book != null`.

```typescript
export class SomeStructuralDirective {

    constructor(
        private _templateRef: TemplateRef,
        private _viewContainerRef: ViewContainerRef
    ) {
    }
    ​
    hide() {
        this._viewContainerRef.clear();
    }
    ​
    show() {
        this._viewContainerRef.createEmbeddedView(this._templateRef);        
    }

}
```

{% hint style="danger" %}
Pour une meilleure maintenabilité et stabilité de vos applications, évitez l'implémentation de "Structural Directives".
{% endhint %}

