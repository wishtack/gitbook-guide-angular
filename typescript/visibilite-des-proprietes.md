# Visibilité des Propriétés

{% hint style="info" %}
Par défaut, les propriétés sont publiques.
{% endhint %}

La visibilité des propriétés est contrôlée comme dans la plupart des langages objets statiquement typés :

```typescript
class Customer {
    public firstName: string;
    private _age: number;
}

const customer = new Customer();

// error TS2341: Property '_age' is private and only accessible within class 'Customer'.
console.log(customer._age);
```

{% hint style="success" %}
Bien que les conventions de nommage TypeScript et Angular découragent l'utilisation du préfixe underscore `_` pour indiquer qu'une propriété est privée, chez Wishtack, nous préfixons nos propriétés privées et vous le recommandons pour les raisons suivantes :

* Il est plus facile de déduire la visibilité d'une propriété sans lire le code source de la classe ou ouvrir un IDE. Exemple: `diff` de code ou code review.
* Vous constaterez plus facilement la fuite de propriétés privées dans des échanges JSON avec une API ou autre suite à une sérialisation maladroite.
* Tant que l'on ne build pas une application Angular avec l'AOT \(en mode production\), les propriétés utilisées dans les templates peuvent être private. Vous serez contents de dénicher rapidement les propriétés privées utilisées dans les templates grâce au préfixe plutôt que d'attendre l'echec du build sur votre intégration continue.

Dans tous les cas, l'important est d'être cohérent et homogène au sein d'une équipe.

P.S.: Angular et Angular Material préfixent également les propriétés privées par `_`.  
[https://github.com/angular/angular](https://github.com/angular/angular)  
[https://github.com/angular/material2](https://github.com/angular/material2)
{% endhint %}

N'oubliez pas que TypeScript analyse le code de façon statique et donc certaines peuvent lui échapper.  
C'est pour cette raison que le code suivant compile et s'exécute sans erreur.

```typescript
const customer = new Customer();
customer['_age'] = 30;
```



