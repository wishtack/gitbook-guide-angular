# Décorateurs de Classe

## Constructor tracker

`Track` est une factory qui retourne un "decorator".

Le décorateur prend en paramètre`target`, la référence de la classe et retourne une nouvelle classe.

```typescript
const Track = () => {

    return (target) => {

        const Klass = function (...args) {
            console.log(`new ${target.name}(${args})`);
            target.constructor.apply(this, args);
        };

        Object.assign(Klass.prototype, target.prototype);

        return Klass as any;

    };

};

@Track()
class Customer {

    constructor(public firstName?: string) {
    }

}

let customer = new Customer(); // new Customer()
customer = new Customer('Foo'); // new Customer(Foo)
```

## Custom element decorator

Cet exemple de factory prend des paramètres permettant de personnaliser le décorateur.

```typescript
const CustomElement = ({selector, template}) => (target) => {

    class Element extends HTMLElement {
    }

    Object.assign(Element.prototype, {
        connectedCallback() {
            this.innerHTML = template;
            return target.connectedCallback.bind(this)();
        }
    });

    customElements.define(selector, Element);

    return Element as any;

};

@CustomElement({
    selector: 'wt-angular-guide',
    template: `<a href="https://angular-guide.wishtack.io">Angular Guide</a>`
})
class AngularGuide {

    connectedCallback() {
        console.log('YEAY!');
    }

}
```

Essayez :

```bash
tsc playground.ts --experimentalDecorators --target es2015
```

Puis copier le résultat JavaScript dans la console de votre navigateur et insérer des éléments `<wt-angular-guide>` sur votre page.

