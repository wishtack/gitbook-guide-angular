# this & "binding"

Qui suis-je ?

{% tabs %}
{% tab title="🧐" %}
```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    sayHi() {
        console.log('Hi ' + this.firstName);
    }
    
    sayHiLater() {
        setTimeout(function () {
            this.sayHi();
        }, 1000);
    }

}

const customer = new Customer('Foo', 'BAR');

customer.sayHiLater(); // ???
```
{% endtab %}

{% tab title="😱" %}
```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    sayHi() {
        console.log('Hi ' + this.firstName);
    }
    
    sayHiLater() {
        setTimeout(function () {
            this.sayHi();
        }, 1000);
    }

}

const customer = new Customer('Foo', 'BAR');

customer.sayHiLater(); // TypeError: this.sayHi is not a function
```
{% endtab %}
{% endtabs %}

## Binding

La fonction de callback utilisée avec `setTimeout` n'est pas "bound" à notre instance de `Customer`. De plus, `setTimeout` espère nous aider en "bindant" l'objet timeout qu'il retourne à notre fonction de callback.

```javascript
const timeout = setTimeout(function () {
    console.log(this === timeout); // true
});
```

Pour lutter contre ce comportement... ~~m\#@d!k~~... déstabilisant, nous pouvons "binder" notre instance de `Customer` à la fonction de callback.

### The hacky way

```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    sayHi() {
        console.log('Hi ' + this.firstName);
    }
    
    sayHiLater() {
        setTimeout(function () {
            this.sayHi();
        }.bind(this), 1000);
    }

}
```

### The other hacky way

```javascript
class Customer {

    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    sayHi() {
        console.log('Hi ' + this.firstName);
    }
    
    sayHiLater() {
        const _this = this;
        setTimeout(function () {
            _this.sayHi();
        }, 1000);
    }

}
```

### Solution idéale

Pour la solution idéale, rendez-vous au [prochain chapitre](arrow-functions.md).

