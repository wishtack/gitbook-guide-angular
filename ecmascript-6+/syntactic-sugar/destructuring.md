# Destructuring

## Array Destructuring

Très pratique pour les tests unitaires.

```javascript
const userList = [
    {firstName: 'Foo'},
    {firstName: 'John'}
];

const [user1, user2, user3, user4 = null] = userList;

console.log(user1); // { firstName: 'Foo' }
console.log(user2); // { firstName: 'John' }
console.log(user3); // undefined
console.log(user4); // null
```

## Object Destructuring

```javascript
const user = {
    firstName: 'Foo',
    lastName: 'BAR',
    email: 'foo.bar@wishtack.com'
};

const {firstName, lastName, phoneNumber} = user;

console.log(firstName); // Foo
console.log(lastName); // BAR
console.log(phoneNumber); // undefined
```

## 

