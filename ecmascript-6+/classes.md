# Classes

## Création d'une classe

{% tabs %}
{% tab title="ES6 Class" %}
```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    getName() {
        return this.firstName;
    }

}
```
{% endtab %}

{% tab title="Legacy Prototype" %}
```javascript
var Customer = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

Customer.prototype = {
    getName: function () {
        return this.firstName;
    }
}

```
{% endtab %}
{% endtabs %}

## Visibilité

En attendant la notion de [class fields](https://github.com/tc39/proposal-class-fields) qui sera probablement bientôt introduite en ECMAScript 2019 [http://kangax.github.io/compat-table/esnext/](http://kangax.github.io/compat-table/esnext/), la notion de visibilité `private` se base sur la convention de nommage qui consiste à préfixer la propriété ou la méthode par le caractère underscore : `_`

```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = null;
        this._isBadPayer = this._tellIfBadPayer();
    }
    
    getName() {
        return this.firstName;
    }
    
    _tellIfBadPayer() {
        return this.firstName === 'foo';
    }

}
```

## Propriétés

```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
    }

    get firstName() {
        return this._firstName;
    }
    
    set firstName(value) {
        this._firstName = value;
    }
    
}

/* @HACK: Last time we use var, I promise! */
var customer = new Customer();

customer.firstName = 'Foo';

console.log(customer.firstName); // Foo
```

{% hint style="danger" %}
Ne les utilisez pas.

L'implémentation de propriétés peut s'avérer pratique dans certains cas extrêmes tels que l'intégration d'une librairie "legacy", mocking, décoration pour "type checking" etc...

Autrement, cela introduit surtout de l'ambiguité dans le code.

Qui pourrait imaginer que le code suivant puisse lever une exception ?

```javascript
var customer = new Customer();
element.textContent = customer.name;
```

Ou pire encore :

```javascript
/* @HACK: Do not remove this useless line as it initializes
 * the user eagerly instead of running it lazily. */
request.user;
```

_Toute ressemblance avec du code existant est fortuite._
{% endhint %}

## Héritage

```javascript
export class WishtackProduct extends Product {

    ...

    getProductId() {
        return 'wishtack-' + this._wishtackId;
    }

}
```

{% hint style="warning" %}
Evitez l'héritage...

... et préférez la composition !
{% endhint %}

## Bonnes pratiques

{% hint style="success" %}
En l'absence de notion de "class fields", il est recommandé d'initialiser toutes les attributs dans le constructeur. Autrement, il est difficile de déterminer les attributs d'une classe et les attributs présents sur une instance dépendront alors des méthodes appelées.
{% endhint %}



