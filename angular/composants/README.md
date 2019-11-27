# Composants

## Le concept

L'un des principaux concepts d'Angular est de voir une application comme une arborescence de composants.

> A Component controls a patch of screen real estate that we could call a view, and declares reusable UI building blocks for an application.

### Custom Elements

C'est d'ailleurs un concept que l'on retrouve dans tous les frameworks modernes et même nativement avec les Custom Elements : [https://developers.google.com/web/fundamentals/web-components/customelements](https://developers.google.com/web/fundamentals/web-components/customelements)

Exemple à essayer dans la console de votre navigateur :

```javascript
/* The custom element definition. */
class HelloElement extends HTMLElement {
    connectedCallback() {
        this.textContent = 'Hello 🌍';
    }
}

/* Registering the custom element. */
customElements.define('wt-hello', HelloElement);

/* Injecting the element using innerHTML... */
document.body.innerHTML = '<wt-hello></wt-hello>';

/* ... or using appendChild & createElement. */
document.body.appendChild(document.createElement('wt-hello'));
```

### Custom Elements & Web Components Tooling

[https://stenciljs.com/](https://stenciljs.com/)

{% embed url="https://stenciljs.com/" %}

[https://github.com/Polymer/lit-element](https://github.com/Polymer/lit-element)

{% embed url="https://github.com/Polymer/lit-element" %}



## Separation of Concerns

Les composants permettent une meilleure décomposition de l'application, facilitent le refactoring et le testing.

## Isolement

Chaque composant est isolé des autres composants. Il n'hérite pas implicitement des attributs des composants parents.



