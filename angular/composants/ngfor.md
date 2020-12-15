# \*ngFor

La directive structurelle `ngFor` permet de boucler sur un array et d'injecter les éléments dans le DOM.

```markup
<ul>
    <li *ngFor="let book of bookList">{{ book.name }}</li>
</ul>
```

{% tabs %}
{% tab title="src/app.component.ts" %}
```typescript
export class AppComponent {
    bookList = [
        {
            name: 'eXtreme Programming Explained'
        },
        {
            name: 'Clean Code'
        }
    ];
}
```
{% endtab %}
{% endtabs %}

Il est possible de récupérer d'autre informations telles que l'index de l'élément :

* `index` : position de l'élément.
* `odd` : indique si l'élément est à une position impaire.
* `even` : indique si l'élément est à une position paire.
* `first` : indique si l'élément est à la première position.
* `last` : indique si l'élément est à la dernière position.

{% tabs %}
{% tab title="src/app.component.html" %}
```markup
<ul>
    <li *ngFor="let book of bookList; let index = index; let isFirst = first; let isOdd = odd;">
        <span>{{ index }}</span>
        <span>:</span>
        <span>{{ book.name }}</span>
        <span>( isFirst: {{ isFirst }}, isOdd: {{ isOdd }} )</span>
    </li>
</ul>
```
{% endtab %}
{% endtabs %}

![Exemple ngFor](../../.gitbook/assets/ng-for-example.png)

## Prochains workshops : 10% de réduction avec le code GUIDEANGULAR

{% embed url="https://www.eventbrite.com/e/angular-unit-testing-workshop-fondamentaux-tdd-francais-tickets-132818877839" caption="Angular Unit-Testing Workshop - Fondamentaux & TDD" %}

## Pour plus d'actus Angular, JavaScript et Web, suivez-moi sur Twitter !

{% embed url="https://twitter.com/yjaaidi" %}



