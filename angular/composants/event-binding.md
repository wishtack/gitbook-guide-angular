# Event Binding

Grâce à l'interpolation et le "property binding", nous contrôlons le contenu affiché mais cela manque d'interactions avec l'utilisateur.

Il nous faut ajouter des "listeners" d'évènements déclenchés sur les éléments du DOM afin de modifier l'état de notre application et laisser Angular mettre à jour la "view".

La syntaxe Angular ci-dessous est équivalente au code natif `button.addEventListener('click', () => this.buy())`.

{% tabs %}
{% tab title="src/app.component.html" %}
```typescript
<button
        type="button"
        (click)="buy()">BUY</button>
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="src/app.component.ts" %}
```typescript
...
export class AppComponent {

    buy() {
        ...
    }

}
```
{% endtab %}
{% endtabs %}

## `$event`

La valeur de l'événement peut être récupérer via la variable `$event`.

{% tabs %}
{% tab title="src/app.component.html" %}
```markup
<div (drop)="onDrop($event)">BUY</div>
```
{% endtab %}
{% endtabs %}

