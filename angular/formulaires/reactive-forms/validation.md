# Validation

## "Validators"

Les constructeurs des "controls" _\(`FormControl`, `FormGroup` et `FormArray`\)_ acceptent en second paramètre **une liste de fonctions de validation** appelées "validators".

Les "validators" natifs d'Angular sont regroupés sous forme de méthodes statiques das la classe `Validators`.

{% code-tabs %}
{% code-tabs-item title="book-form.component.ts" %}
```typescript
export class BookFormComponent {

    bookForm = new FormGroup({
        title: new FormControl(null, [
            Validators.required
        ]),
        description: new FormControl()
    });

    submitBook() {
        console.log(this.bookForm.value);
    }
    
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
Remarquez que l'ajout du "validator" ne change pas le comportement du composant : **la méthode `submitBook` continue à être appelée bien que la contrainte de validation ne soit pas respectée.**

C'est au composant de décider de l'action à mener en fonction de l'état des "controls".
{% endhint %}

Les "controls" disposent d'une série de **propriétés et de méthodes permettant d'en vérifier l'état** :

* `valid` : Valeur booléenne indiquant si le "control" est valide. Dans le cas d'un `FormGroup` ou `FormArray`, **le "control" est valide si les "controls" qui le composent sont tous valides**. 
* `errors` : "plain object" combinant les erreurs de tous les validateurs.  Vaut `null` si le "control" est valide. 
* `touched` : Valeur booléenne positionnée à `true` dès le déclenchement de l'événement `blur` _\(i.e. l'utilisateur change de "focus"\)_. 
* `pristine` : Valeur booléenne indiquant si le "control" a été modifié.

### Exemple de désactivation du "submit"

{% code-tabs %}
{% code-tabs-item title="book-form.component.html" %}
```markup
<button
    [disabled]="!bookForm.valid"
    type="submit">SUBMIT</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## `hasError` & `getError`

Les méthodes `hasError` et `getError` sont deux méthodes "helpers" permettant d'**accéder plus facilement** aux informations d'erreur d'un "control".

{% code-tabs %}
{% code-tabs-item title="book-form.component.html" %}
```markup
<div *ngIf="shouldShowTitleRequiredError()">Title is required.</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="book-form.component.ts" %}
```typescript
shouldShowTitleRequiredError() {

    const title = this.bookForm.controls.title;
    
    return title.touched && title.hasError('required');

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="danger" %}
Préférez les méthodes `hasError` et `getError` aux opérateurs ternaires _\(`title.errors ? title.errors.required : null`\)_ !
{% endhint %}

## "Validator" personnalisé

Un "validator" est une fonction qui est **appelée à chaque changement de la valeur du "control"** afin d'en vérifier la validité. Si la valeur est valide, le "control" retourne **`null`** ou un **objet d'erreur** dans le cas contraire.

{% code-tabs %}
{% code-tabs-item title="valid-book-title.validator.ts" %}
```typescript
import { ValidatorFn } from '@angular/forms';
​
export const validBookTitle: ValidatorFn = (control) => {

    /* Is not valid. */
    if (/learn .*? in one day/i.test(control.value)) {
        return {
            'validBookTitle': {
                reason: 'blacklisted',
                value: control.value
            }
        };
    }

    /* Is valid. */
    return null;

};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="book-form.component.ts" %}
```typescript
bookForm = new FormGroup({
    title: new FormControl(null, [
        Validators.required,
        validBookTitle
    ]),
    description: new FormControl()
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
Les informations d'erreur \(`reason` et `value`\) sont alors accessibles grâce à la méthode `getError`.

```typescript
const error = bookForm.getError('validBookTitle', 'title');
if (error != null) {
    console.log(error.reason);
    console.log(error.value);
}
```
{% endhint %}

### "Validator" paramétré

Tels que le "validator" `minLength`, **certains "validators" ont besoin de paramètres** pour personnaliser leur comportement.  
Dans ce cas, il suffit d'implémenter une **"factory" de "validators"** _\(i.e. : une fonction qui retourne des fonctions de type "validator"\)_.

{% code-tabs %}
{% code-tabs-item title="valid-pattern.validator.ts" %}
```typescript
export type ValidPattern = (args: {blacklistedPatternList: RegExp[]}) => ValidatorFn;

const validPattern: ValidPattern = ({blacklistedPatternList}) => {

    return (control) => {

        for (const blacklistedPattern of blacklistedPatternList) { ... }

        return null;

    };

};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Ou en utilisant **le "currying" des** [**Arrow Functions**](../../../ecmascript-6+/arrow-functions.md) :

{% code-tabs %}
{% code-tabs-item title="valid-pattern.validator.ts" %}
```typescript
export type ValidPattern = (args: {blacklistedPatternList: RegExp[]}) => ValidatorFn;

const validPattern: ValidPattern = ({blacklistedPatternList}) => control => {

    for (const blacklistedPattern of blacklistedPatternList) {

        const result = blacklistedPattern.exec(control.value);

        if (result != null) {
            return {
                'validPattern': {
                    reason: 'blacklisted',
                    match: result[0],
                    value: control.value
                }
            };
        }

    }

    return null;

};
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Le "validator" est alors personnalisé à l'utilisation :

{% code-tabs %}
{% code-tabs-item title="book-form.component.ts" %}
```typescript
bookForm = new FormGroup({
    title: new FormControl(null, [
        Validators.required,
        validPattern({
            blacklistedPatternList: [
                /^learn .*? in one day/i,
                /hero/i,
                /ninja/i
            ]
        })
    ]),
    description: new FormControl()
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
Même l'erreur contient alors des informations personnalisées :

```typescript
const error = bookForm.getError('validPattern', 'title');
if (error != null) {
    console.log(error.reason); // blacklisted
    console.log(error.value); // Become an Angular Ninja
    console.log(error.match); // Ninja
}
```
{% endhint %}

## "Async Validators"'

Il est parfois **nécessaire d'implémenter des "validators" asynchrones** _\(e.g. vérification distante via une API\)_.  
Un "validator" asynchrone se comporte de la même façon qu'un "validator" synchrone mais au lieu de retourner `null` ou un objet d'erreur, **il doit retourner un `Observable`**.

{% hint style="success" %}
Les **"validators" asynchrones peuvent être transmis au "control"** par paramètre ordonné _\(3ème paramètre après la valeur initiale et les validators synchrones\)_ mais **il est préférable d'utiliser un objet** plus explicite en guise de second paramètre :

```typescript
new FormControl(null, {
    validators: [Validators.required],
    asyncValidators: [myAsyncValidator]
})
```
{% endhint %}

### Validators & "Dependency Injection"

Les "validators" _\(et plus particulièrement les "validators" asynchrones\)_ ont parfois **besoin d'accéder aux services** _\(au sens Angular\)_ mais sans [Dependency Injection](../../dependency-injection/) pour les "validators", l'astuce consiste à **implémenter la "factory" du "validator" dans un service dédié afin de profiter de la "Dependency Injection"**.

{% code-tabs %}
{% code-tabs-item title="valid-pattern-factory.ts" %}
```typescript
@Injectable({
    providedIn: 'root'
})
export class ValidPatternFactory {

    constructor(private _httpClient: HttpClient) {
    }

    create(): AsyncValidatorFn {
        return control => {

            return this._httpClient.post<ApiValidatorResult>('/validations', {
                validatorId: 'VALIDATOR_ID',
                keywords: control.value
            })
                .pipe(map(result => result.errors));

        };
    }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="book-form.component.ts" %}
```typescript
export class BookFormComponent {

    bookForm: FormGroup;
​
    constructor(private _validPatternFactory: ValidPatternFactory) {
        this.bookForm = new FormGroup({
            title: new FormControl(null, {
                validators: [Validators.required],
                asyncValidators: [this._validPatternFactory.create()]
            }),
            description: new FormControl()
        });
    }
    
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Annulation des Observables

{% hint style="warning" %}
Dès la saisie d'une nouvelle valeur, l'`Observable` **récupéré précédemment sera annulé** avant de déclencher le traitement du nouvel `Observable` retourné _\(via un `subscribe`\)_.
{% endhint %}

## Personnalisation de l'affichage

Afin de **faciliter la personnalisation du CSS en fonction de la validité** des éléments du formulaire, Angular ajoute automatiquement les classes CSS suivantes en fonction de l'état du "control" :

* `.ng-valid`
* `.ng-invalid`
* `.ng-pending`
* `.ng-pristine`
* `.ng-dirty`
* `.ng-untouched`
* `.ng-touched`

```css
input.ng-touched.ng-invalid {
    border-width: 1px;
    border-left: red solid 5px;
}
```

