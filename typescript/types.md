# Types

## Les types les plus fréquents

### boolean

```typescript
let value: boolean;
value = true;
```

### number

```typescript
let value: number;
value = 10;
value = 10.3;
value = Infinity;
value = NaN;
```

### string

```typescript
let value: string;
value = 'Foo BAR';
```

### array

```typescript
let value: string[];

value = ['Angular', 'Python'];

value.push(42); // error TS2345: Argument of type 'number' is not assignable to parameter of type 'string'.
```

### enum

```typescript
enum Role {
    Coder,
    Developer
}

let role: Role;

role = Role.Developer;

console.log(role); // 1
console.log(Role[role]); // Developer
```

{% hint style="danger" %}
Comme dans de nombreux langages, il est préférable d'éviter les enums à auto-incrément pour les raisons suivantes :

* Ce type d'enums décourage le refactoring car il est nécessaire de "rebuild" toutes les applications et librairies qui en dépendent. _\(On revient aux problèmes de compatibilité binaire etc...\)_
* Le debug est moins pratique.
* Il faut absolument passer par un serializer/deserializer pour communiquer la valeur avec d'autres applications, services etc...
{% endhint %}

### string enum

```typescript
enum Role {
    Coder = 'coder',
    Developer = 'developer'
}

let role: Role;

role = Role.Developer;

console.log(role); // developer
```

### Number, String, Boolean and Object

{% hint style="danger" %}
N'utilisez jamais les types suivants : **Number**, **String**, **Boolean** et **Object**.

Ce ne sont pas les types primitifs. Considérez-les comme "legacy".  
  
Au lieu de `Object`, utilisez le type TypeScript `object`.
{% endhint %}

## Paramètres optionnels

Contrairement à l'ECMAScript où tous les paramètres sont considérés optionnels, en TypeScript, les paramètres doivent être explicitement indiqués comme optionnels avec la syntaxe suivante :

```typescript
const sendMessage = (email: string, message: string, subject?: string) => {
    console.log(`${email}: [${subject}] ${message}`);
};

sendMessage(); // error TS2554: Expected 2-3 arguments, but got 0.

sendMessage('contact@wishtack.com', 'Help!'); // contact@wishtack.com: [undefined] Help!
sendMessage('contact@wishtack.com', 'Help!', 'Coaching'); // contact@wishtack.com: [Coaching] Help!
```

... ou en spécifiant une valeur par défaut :

```typescript
const sendMessage = (email: string, message: string, subject: string = null) => {
    console.log(`${email}: [${subject}] ${message}`);
};

sendMessage('contact@wishtack.com', 'Help!'); // contact@wishtack.com: [null] Help!
```



