# Entity Constructor

Essayons d'appliquer les [named parameters](../../ecmascript-6+/named-parameters.md) au constructeur d'une entité TypeScript.

```typescript
class Customer {

    firstName: string;
    lastName: string;
    
    constructor(args) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }

}
```

## Paramètre `args` optionnel

Le paramètre `args` est obligatoire.

Il suffit de lui indiquer une valeur par défaut.

En utilisant `args?` ou `args = null` nous obtiendrions une exception lors de l'accès à la propriété `firstName`. Il faut donc initialiser à `{}`.

```typescript
class Customer {

    firstName: string;
    lastName: string;

    constructor(args = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }

}
```

## Typing du paramètre `args`

Le paramètre `args` n'est pas typé et notre IDE ne nous aidera pas.

Il faut typer le paramètre `args`.

```typescript
interface CustomerArgs {
    firstName: string;
    lastName: string;
}

class Customer {

    firstName: string;
    lastName: string;

    constructor(args: CustomerArgs = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }

}
```

## Valeurs par défaut

La valeur par défaut `{}` ne correspond pas au type `CustomArgs`. Il suffit de créer une constante avec les valeurs par défaut ou plus simplement marquer toutes les propriétés comme optionnelles.

```typescript
interface CustomerArgs {
    firstName?: string;
    lastName?: string;
}

class Customer {

    firstName: string;
    lastName: string;

    constructor(args: CustomerArgs = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }

}
```

## Le Duck Typing rentre en jeu

`Customer` et `CustomerArgs` sont similaires au sens [Duck Typing](../duck-typing.md).

```typescript
class Customer {

    firstName?: string;
    lastName?: string;

    constructor(args: Customer = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }

}
```

## Une "factory" gratuite

Grâce au Duck Typing, nous obtenons une "factory" gratuitement :

```typescript
/* Default. */
customer = new Customer();

/* Partial. */
customer = new Customer({
    firstName: 'Foo'
});

/* Deserializer. */
const data = JSON.parse('...');
// Instead of new Customer(data.firstName, data.lastName)
customer = new Customer(data); 

/* Copy. */
customer = new Customer(customer);

// error TS2345: Argument of type '{ firstName: string; lastName: string; email: string; }' is not assignable to parameter of type 'Customer'.
//  Object literal may only specify known properties, and 'email' does not exist in type 'Customer'.
customer = new Customer({
    firstName: 'Foo',
    lastName: 'BAR',
    email: 'foo.bar@wishtack.com'
});
```

## Entity schema

Si on change la signature de la classe `Customer`, paf ! ça casse tout !

```typescript
class Customer {

    firstName?: string;
    lastName?: string;

    // error TS2322: Type '{}' is not assignable to type 'Customer'.
    //   Property 'getName' is missing in type '{}'.
    constructor(args: Customer = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }
    
    getName() {
        return `${this.firstName} ${this.lastName}`;
    }

}
```

Dans ce cas, la solution ne nécessite qu'un léger refactoring local en séparant la description de l'entité avec le constructeur dans la classe `CustomerSchema` puis les méthodes dans la classe `Customer`.

```typescript
class CustomerSchema {

    firstName?: string;
    lastName?: string;

    // error TS2322: Type '{}' is not assignable to type 'Customer'.
    //   Property 'getName' is missing in type '{}'.
    constructor(args: CustomerSchema = {}) {
        this.firstName = args.firstName;
        this.lastName = args.lastName;
    }
    
}

class Customer extends CustomerSchema {
    
    getName() {
        return `${this.firstName} ${this.lastName}`;
    }

}
```

{% hint style="success" %}
Dans une approche plus fonctionnelle et avec un souci de séparation des responsabilités, la méthode `Customer.getName` pourrait être déplacée dans une classe `CustomerHelper:`

```typescript
class CustomerHelper {
    getName(customer: Customer) {
        ...
    }
}
```
{% endhint %}

