# Rest

## Array Rest

Les "rest parameters" sont l'équivalents des paramètres variadiques dans d'autres langages.

La fonction `add` ci-dessous peut prendre une infinité de paramètres qui seront mises dans l'`Array` `valueList`.

```javascript
const add = (...valueList) => {
    return valueList.reduce((total, value) => total + value, 0);
};

add(0, 1, 2, 3); // 6
```

{% hint style="warning" %}
Il vaut mieux éviter ce type d'usage du "rest".  
Cela réduit l'extensibilité de la fonction et il est préférable de prendre un paramètre de type `Array`.

```typescript
const add = (valueList) => {
    return valueList.reduce((total, value) => total + value, 0);
};

add([1, 2, 3]); // 6
```
{% endhint %}

En revanche, cela peut s'avérer très pratique dans des cas de wrapping ou de décoration etc...

```javascript
const wrapper = (funk, ...args) => {
    console.log(`Calling function with ${args.length} arguments.`);
    return funk(...args);
};

wrapper(add, 1, 2, 3);
```

## Object Rest

La même syntaxe peut être utilisée avec le "[destructuring](destructuring.md#object-destructuring)" d'un objet :

```typescript
const user = {
    firstName: 'Foo',
    lastName: 'BAR',
    email: 'foo.bar@wishtack.com',
    phoneNumber: '123'
};

const { firstName, lastName, ...remainingProperties } = user;

console.log(remainingProperties); // { email: 'foo.bar@wishtack.com', phoneNumber: '123' }
```



