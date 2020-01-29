# Unit-Test d'un Composant

La méthode **`TestBed.createComponent`** instancie le composant dont le type est transmis en paramètre puis retourne un objet **`ComponentFixture` permettant de contrôler et inspecter le composant**.

Les principales propriétés et méthodes de cette classe sont les suivantes :

* **`componentInstance`** : l'instance de la classe `BookPreviewComponent`,
* **`debugElement`** : objet permettant d'inspecter et de manipuler le DOM.
* **`detectChanges()`** : déclenche la [Change Detection](../../change-detection/).

{% tabs %}
{% tab title="book-preview.component.spec.ts" %}
```typescript
import { async, fakeAsync, TestBed } from '@angular/core/testing';
import { Book } from './app/book/book';
import { BookPreviewComponent } from './app/book/book-preview/book-preview.component';
import { BookModule } from './app/book/book.module';

describe('BookPreviewComponent', () => {

    beforeEach(async(() => {

        TestBed.configureTestingModule({
            imports: [
                BookModule
            ]
        }).compileComponents();

    }));

    it('should show book info', fakeAsync(() => {

        const fixture = TestBed.createComponent(BookPreviewComponent);
        const component = fixture.componentInstance;
        const debugElement = fixture.debugElement;

        component.book = new Book({
            title: 'eXtreme Programming Explained'
        });

        fixture.detectChanges();

        expect(debugElement.nativeElement.querySelector('h1').textContent)
            .toEqual('eXtreme Programming Explained');

    }));

});
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Lors de la configuration du `TestBed`, **il est préférable d'importer le module contenant le composant à tester** que de redéclarer le composant et réimporter ses dépendances.
{% endhint %}

{% hint style="warning" %}
Pour déclencher des événements sur le DOM _\(e.g. : changement d'un input de formulaire\)_, il faut utiliser la méthode native `dispatchEvent`.

```typescript
const input: HTMLInputElement = debugElement.nativeElement.querySelector('input');
input.value = 'test';
input.dispatchEvent(new Event('input'));
```

N'oubliez pas d'**appeler la méthode `detectChanges` dès l'instanciation du composant** pour initialiser le formulaire et permettre à Angular d'ajouter les bons listeners etc...
{% endhint %}

