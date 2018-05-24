# Duck Typing

On retrouve en TypeScript ce concept très pythonique et pratique.

> If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck.

... ce à quoi nous pouvons ajouter que si ce n'était pas un canard alors il n'avait qu'à **mieux marquer sa différence** et surtout **ne pas rôder autour des chasseurs**. 

Autrement dit, si deux objets "matchent" en terme de "duck typing" alors que leurs propriétés n'ont pas du tout la même signification alors cela révèle plutôt un problème de design qu'un problème lié au "duck typing" en lui-même.

Exemple :

```typescript
/* Some user library. */
class User {
    firstName: string;
    lastName: string;
    email: string;
    address: string;
}

/* Some shop administration library. */
class ShopOwner {
    firstName: string;
    lastName: string;
    email: string;
    phoneNumber: string;
}

class Shop {
    email: string;
}

/* Emailing library. */
interface Emailable {
    firstName: string;
    lastName: string;
    email: string;
}

const sendEmail = (message: string, emailable: Emailable) => {
    ...
};

// OK
sendEmail('Hi', new User());
// OK
sendEmail('Hi', new ShopOwner());
// OK
sendEmail('Hi', {
    firstName: 'Foo',
    lastName: 'BAR',
    email: 'foo.bar@wishtack.com'
});

// error TS2345: Argument of type 'Shop' is not assignable to parameter of type 'Emailable'.
//  Property 'firstName' is missing in type 'Shop'.
sendEmail('Hi', new Shop());
```



