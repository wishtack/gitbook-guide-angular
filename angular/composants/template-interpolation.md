# Template Interpolation

Comme de nombreux langages de "templating", Angular utilise la syntaxe "double curly braces" pour l'interpolation :

{% code-tabs %}
{% code-tabs-item title="src/app.component.ts" %}
```typescript
@Component({
    selector: 'wt-app',
    templateUrl: './app.component.html'
})
export class AppComponent {
    bookName = 'eXtreme Programming Explained';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="src/app.component.html" %}
```markup
<div>
    <span>{{ bookName }}</span>
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

La syntaxe d'interpolation permet d'accéder directement aux propriétés du composant associé _\(un peu comme si toutes les expressions étaient préfixées par un `this.`\)._ En effet, l'instance de la classe `AppComponent` est le contexte utilisé par le moteur de rendu afin d'évaluer les expressions.

{% hint style="success" %}
Les expressions utilisées dans le "template interpolation" \(et les "[property binding](property-binding.md)"\) doivent rester **simples**.  
Vous pouvez utiliser des méthodes de votre composant mais elles doivent être **performantes**, **sans effet de bord** et produire des résultats **prédictibles**.
{% endhint %}

{% hint style="danger" %}
L'interpolation ne doit être utilisée que pour définir le contenu d'un élément HTML.

N'utilisez donc jamais la syntaxe d'interpolation pour contrôler les attributs d'un élément par exemple !

😱`<img src="{{ pictureUrl }}">`😱

Préférez l'utilisation du [Property Binding](property-binding.md) pour les raisons suivantes :

* Les attributs d'un élément ne sont pas tous associés à des propriétés et vice versa.
* Les attributs d'un élément ne sont pas forcéments toujours synchronisés avec ses propriétés.
* Les attributs d'un élément ne sont pas toujours du même type que la propriété associée _\(i.e. :_ _`element.getAttribute('disabled') !== element.disabled`\)._
* Certaines propriétés attendent des valeurs complexes alors que les attributs ne permettent de passer que des valeurs de type "string".
{% endhint %}



