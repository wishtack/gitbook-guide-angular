# Pipes

Les `Pipe`s sont des **filtres utilisables directement depuis la vue afin de transformer les valeurs** lors du "binding".

## Utilisation des `Pipes`

### Syntaxe

La syntaxe des `Pipe`s est simplement inspirée des `Pipe`s des shell UNIX que l'on retrouve dans de nombreux systèmes de templating.

```markup
<div>{{ user.firstName | lowercase }}</div>
```

### Paramètres

Les `Pipe`s peuvent prendre des paramètres qu'il faut mettre après le `Pipe` et séparés avec le symbole ":".

```markup
<div>{{ user.firstName | slice:0:10 }}</div>
```

### Chaînage

Les "pipes" peuvent être chaînés.

```markup
<div>{{ user.firstName | slice:0:10 | lowercase }}</div>
```

### Les `Pipes` natifs

Angular dispose de plusieurs "pipes" natifs : [https://angular.io/api?type=pipe](https://angular.io/api?type=pipe).

## Le `Pipe` `async`

Le `Pipe` `async` est un `Pipe` **capable de consommer des `Observable`s** _**\(ou `Promise`\)**_ en appelant implicitement la méthode `subscribe` _\(ou `then`\)_ afin de **"binder" les valeurs contenus dans l'`Observable`** _\(ou la `Promise`\)_.

Dans les cas des `Observable`s, ce `Pipe` `unsubscribe` automatiquement à la destruction de la vue.  
Cf. [Gestion de la Subscription](http/gestion-de-la-subscription.md).

## `Pipe`s personnalisés

Pour créer un `Pipe` personnalisé, il faut :

1. **implémenter une classe** suivant l'interface `PipeTransform`,
2. décorer cette classe avec le **décorateur `@Pipe()`** en indiquant le nom du `Pipe`.
3. ajouter la classe aux **`declarations` \(et `exports`\) du module** associé.

Le `Pipe` `price` ci-dessous permet d'afficher le prix d'un produit représenté avec la classe `Price` suivante :

{% tabs %}
{% tab title="price.ts" %}
```typescript
export class Price {

    coefficient?: number;
    exponent?: number;
    currency?: string;

    constructor(args: Price = {}) {
        this.coefficient = args.coefficient;
        this.exponent = args.exponent;
        this.currency = args.currency;
    }

}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="price.pipe.ts" %}
```typescript
@Pipe({
    name: 'price'
})
export class PricePipe implements PipeTransform {

    transform(price: Price): string {

        if (price == null) {
            return null;
        }

        const amount = price.coefficient * Math.pow(10, price.exponent);

        return `${amount} ${price.currency}`;

    }

}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="price.module.ts" %}
```typescript
@NgModule({
    declarations: [
        PricePipe
    ],
    exports: [
        PricePipe
    ]
})
export class PriceModule {
}
```
{% endtab %}
{% endtabs %}

Le `Pipe` peut alors être utilisé dans **n'importe quel composant contenu dans un module qui importe le module** **`PriceModule`** :

{% tabs %}
{% tab title="book-preview.component.ts" %}
```typescript
export class BookPreviewComponent {
    bookPrice = new Price({
        coefficient: 1010,
        exponent: -2,
        currency: 'USD'
    });
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="book-preview.component.html" %}
```markup
<div>{{ bookPrice | price }}</div>
```
{% endtab %}
{% endtabs %}

On obtient alors le résultat suivant :

```markup
<div>10.1 USD</div>
```

## "Dependency Injection"

Le **constructeur d'un `Pipe`** peut être utiliser pour **injecter des dépendances**.

Dans notre cas, nous souhaitons injecter le `Pipe` `currency` afin de **profiter de ses fonctionnalités tout en simplifiant son utilisation**.

{% tabs %}
{% tab title="price.pipe.ts" %}
```typescript
@Pipe({
    name: 'price'
})
export class PricePipe implements PipeTransform {

    constructor(private _currencyPipe: CurrencyPipe) {
    }

    transform(price: Price): string {

        if (price == null) {
            return null;
        }

        const amount = price.coefficient * Math.pow(10, price.exponent);

        return this._currencyPipe.transform(amount, price.currency);

    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Malheureusement, Angular ne définit pas nativement de `providers` pour les `Pipe`s.

En attendant la résolution de ce bug [https://github.com/angular/angular/issues/15107](https://github.com/angular/angular/issues/15107), une des solutions de contournement pour injecter un `Pipe` est de l'ajouter à la liste des `providers` d'un module importé par le module définissant notre Pipe.

```typescript
/* @HACK: Cf. https://github.com/angular/angular/issues/15107 */
@NgModule({
    providers: [
        CurrencyPipe
    ]
})
export class InjetableCurrencyPipeModule {
}

@NgModule({
    imports: [
        InjetableCurrencyPipeModule
    ]
})
export class BookModule {
}
```
{% endhint %}

## Pureté du `Pipe`

### Les `Pipe`s purs

Par défaut, les `Pipe`s sont purs. C'est à dire que **leur méthode `transform` est une fonction pure**, la **valeur retournée ne dépend que des paramètres** reçus et les appels n'ont aucun effet de bord. Autrement dit, le `Pipe` est "stateless".

{% hint style="warning" %}
Par optimisation, à chaque [Change Detection](change-detection/), **Angular n'évalue les `Pipe`s purs** _\(i.e. appel de la méthode `transform`\)_ **que quand les paramètres changent**.

**Angular ne considère que les paramètres ont changé que si leurs références ont changé**.

Il faut donc **veiller au respect de l'**[**immutabilité**](change-detection/immutabilite.md).
{% endhint %}

### Les `Pipe`s impurs

Les `Pipe`s impurs sont évalués à chaque [Change Detection](change-detection/) même quand les paramètres ne changent pas.  
Ils sont déclarés en passant la valeur `false` au paramètre `pure` du décorateur `Pipe` :

```typescript
@Pipe({
    pure: false
})
export class BadPipe {
}
```

Nativement, seuls les `Pipe`s suivant sont impurs :

* `async` : Il est impur car les valeurs retournées par ce `Pipe` peuvent changer à n'importe quel moment étant donné qu'elles proviennent d'une source asynchrone _\(`Observable` ou `Promise`\)._ 
* `json` : Etant donné qu'il est principalement utilisé pour le "debug", ce `Pipe` est impur car il est préférable que la valeur retournée soit mise à jour même si l'immutabilité n'est pas respectée. 
* `slice` : Bien que ce `Pipe` fonctionnerait parfaitement en étant pur, il est probablement impur pour faciliter l'adoption d'Angular par la communauté car malheureusement l'immutabilité des `Array`s est rarement respectée par les développeurs Angular.

{% hint style="danger" %}
**Ne les utilisez pas !**

L'implémentation de `Pipe`s impurs est un "code smell" qui révèle généralement des problèmes de conception.
{% endhint %}

