# Typing des Propriétés

## Typing des propriétés et des paramètres

```typescript
class Customer {

    firstName: string;
    lastName: string;
    age: number;

    constructor(firstName: string, lastName: string, age: number) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

}
```

Pour le code suivant :

```typescript
new Customer('Foo', 123);
```

... nous obtenons l'erreur :

```bash
error TS2345: Argument of type '123' is not assignable to parameter of type 'string'.
```

{% hint style="warning" %}
Par défaut, les paramètres et propriétés sont de type `any`.  
Ils sont alors compatibles avec tous les autres types.  
Si nous n'avions pas typé les paramètres du constructeur, nous aurions pu instancier l'objet avec des propriétés contenant des valeurs de type autre que `string`.

**Exemple** :  
Le code ci-dessous compile sans souci...

```typescript
class Customer {

    firstName: string;

    constructor(firstName) {
        this.firstName = firstName;
    }

}

const customer = new Customer(123);

customer.firstName.toUpperCase();
```

... mais les problèmes apparaîtront en runtime car `customer.firstName` sera de type `number` et n'aura donc pas de méthode `toUpperCase`.
{% endhint %}

## Raccourci pour les paramètres ordonnées du constructeur

La duplication du type en paramètre du constructeur et en propriété est fastidieuse et fréquente.

TypeScript propose une syntaxe concise "**Parameter Properties**" pour déclarer et initialiser les propriétés en indiquant simplement la visibilité de chaque paramètre du constructeur.

```typescript
class Customer {

    constructor(public firstName: string,
                public lastName: string,
                public age: number) {
    }

}

const customer = new Customer('Foo', 'BAR', 30);

console.log(customer.firstName); // Foo
```



