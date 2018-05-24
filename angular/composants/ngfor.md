# \*ngFor

La directive structurelle `ngFor` permet de boucler sur un array et d'injecter les éléments dans le DOM.

```markup
<ul>
    <li *ngFor="let book of bookList">{{ book.name }}</li>
</ul>
```

{% code-tabs %}
{% code-tabs-item title="src/app.component.ts" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

Il est possible de récupérer d'autre informations telles que l'index de l'élément :

* `index` : position de l'élément.
* `odd` : indique si l'élément est à une position impaire.
* `even` : indique si l'élément est à une position paire.
* `first` : indique si l'élément est à la première position.
* `last` : indique si l'élément est à la dernière position.

{% code-tabs %}
{% code-tabs-item title="src/app.component.html" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

![Exemple ngFor](../../.gitbook/assets/ng-for-example.png)

