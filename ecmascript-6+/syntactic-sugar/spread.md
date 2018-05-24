# Spread

## Array Spread

```javascript
const itemList = [1, 2, 3];
const additionalItemList = [5, 6];

const newItemList = [...itemList, 4, ...additionalItemList];

console.log(newItemList); // [1, 2, 3, 4, 5, 6]
```

## Object Spread

Très pratique pour respecter l'immutabilité.

```javascript
const user = {
    firstName: 'Foo',
    lastName: 'BAR',
    email: 'invalid@wishtack.com'
};

const newUser = {
    ...user,
    email: 'foo.bar@wishtack.com',
    phoneNumber: '+6 12 34 56 78'
};

console.log(newUser);
// {
//    firstName: 'Foo',
//    lastName: 'BAR',
//    email: 'foo.bar@wishtack.com',
//    phoneNumber: '+6 12 34 56 78'
//}
```

## 

