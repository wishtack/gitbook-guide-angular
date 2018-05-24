# Named Parameters

Les paramètres ordonnées nuisent à la lisibilité du code et au refactoring.

```javascript
class Customer {
    constructor(firstName, lastName, email, phoneNumber) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
        this.phoneNumber = phoneNumber;
    }
}

new Customer('Foo', null, null, '123');
```

Malheureusement, les named parameters n'existent pas mais une solution de contournement native est possible grâce au destructuring.

## Named Parameters avec un seul paramètre.

```javascript
class Customer {
    constructor(args) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
        this.email = args.email;
        this.phoneNumber = args.phoneNumber;
    }
}

new Customer({
    firstName: 'Foo',
    phoneNumber: '123'
});
```

... mais cette approche n'est pas très IDE-friendly. L'utilisateur de la classe ne verra pas clairement les paramètres attendus.

## Destructuring

```javascript
class Customer {
    constructor(args = {}) {
    
        const {firstName, lastName, email, phoneNumber = null} = args;
    
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
        this.phoneNumber = phoneNumber;
    }
}
```

Hop ! On gagne l'initialisation des valeurs par défaut.

Ou encore mieux :

```javascript
class Customer {
    constructor({firstName, lastName, email, phoneNumber = null} = {}) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
        this.phoneNumber = phoneNumber;
    }
}
```

## Recyclage

Pour des objets peu complexes, ce constructeur nous évite d'implémenter des "factories".

```javascript
const customer = new Customer({firstName: 'Foo'});

const customerFromJson = new Customer(JSON.parse(data));

const customerCopy = new Customer(customer);
```

