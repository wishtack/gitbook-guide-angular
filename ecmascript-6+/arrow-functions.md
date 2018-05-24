# Arrow Functions

```javascript
/* 90s */
function sayHi(userName) {
    console.log('Hi ' + userName);
}

/* 2000s */
var sayHi = function (userName) {
    console.log('Hi ' + userName);
};

/* 2015 */
const sayHi = (userName) => {
    console.log('Hi ' + userName);
});
```

## No binding

Les arrow functions ne peuvent être "bound" et accèdent donc au `this` du closure parent.

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
        setTimeout(() => {
            this.sayHi();
        }, 1000);
    }

}
```

## Exemple avec `Array.filter` et `Array.map`

```javascript
const productList = [
    {
        title: 'Browserstack',
        price: 50
    },
    {
        title: 'Keyboard',
        price: 20
    },
    {
        title: 'Prerender',
        price: 10
    }
];

const cheapProductList = productList.filter((product) => {
    return product.price < 25;
});

const cheapProductTitleList = cheapProductList.map((product) => {
    return product.title;
});

console.log(cheapProductTitleList); // ['Keyboard', 'Prerender']

```

## One-liner

Peu importe le contexte, les fonctions de callback sont souvent des "one-liners".  
Dans ce cas, les accolades, le `return` et le `;` peuvent être retirés.  
De même, si la fonction ne prend qu'un seul paramètre, les parenthèses peuvent être également ignorées.

On peut aussi remarquer le pattern builder des méthodes `filter` et `map` qui nous permet de chaîner les appels.

```javascript
const cheapProductTitleList = productList
    .filter(product => product.price < 25)
    .map(product => product.title);
```

{% hint style="warning" %}
En cas de shadowing de nom de variable, évitez les variables à une lettre ou des noms génériques.

`filter(u => u.id === user.id)`

`filter(it => it.id === user.id)`
{% endhint %}



{% hint style="success" %}
Il est commun de préfixer par un underscore \`\_\` les variables servant à itérer.

`filter(_user => _user.id === user.id`_`)`_
{% endhint %}



