# Utilisation de HttpClient

## 1. Injection du service `HttpClient`

`HttpClient` est un service Angular ; on peut donc le récupérer avec la [Dependency Injection](../dependency-injection/).

{% tabs %}
{% tab title="book-search.component.ts" %}
```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
    selector: 'wt-book-search',
    templateUrl: './book-search.component.html'
})
export class BookSearchComponent {

    constructor(private _httpClient: HttpClient) {
    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
On obtient l'erreur suivante `No provider for HttpClient!` car le service `HttpClient` n'est pas encore [Tree-Shakable](../dependency-injection/tree-shakable-services.md) et il faut donc importer le module associé `HttpClientModule`.
{% endhint %}

Etant donné que le service `HttpClient` est stateless, nous pouvons importer le module `HttpClientModule` directement dans notre [Feature Module](../project-structure-and-modules/feature-module.md) `BookModule`.

{% tabs %}
{% tab title="book.module.ts" %}
```typescript
import { HttpClientModule } from '@angular/common/http';

@NgModule({
    declarations: [
        BookPreviewComponent,
        BookSearchComponent
    ],
    exports: [
        BookPreviewComponent,
        BookSearchComponent
    ],
    imports: [
        HttpClientModule,
        SharedModule
    ]
})
export class BookModule {
}
```
{% endtab %}
{% endtabs %}

## 2. Exécution de la requête

### `HttpClient.get` & co.

Nous pouvons donc récupérer les données par API dans le "lifecycle hook" `ngOnInit`.

{% tabs %}
{% tab title="book-search.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
    selector: 'wt-book-search',
    templateUrl: './book-search.component.html'
})
export class BookSearchComponent implements OnInit {

    private _bookListUrl = 'https://www.googleapis.com/books/v1/volumes?q=extreme%20programming';

    constructor(private _httpClient: HttpClient) {
    }

    ngOnInit() {
        this._httpClient.get(this._bookListUrl);
    }

}
```
{% endtab %}
{% endtabs %}

### Déclenchement de la requête au `subscribe`

En inspectant le comportement du "browser“, on peut remarquer que **la requête n'est pas envoyée**.

En effet, les méthodes `get`, `delete`, `patch`, `post`, `put`, `request` etc... **retournent toujours un `Observable`.**

Cet `Observable` est "lazy" et il faut donc `subscribe` pour déclencher le traitement.

{% hint style="info" %}
Par défaut, les paramètres `observe` et `responseType` valent respectivement `body` et `json` ; ce qui veut dire que nous allons directement **récupérer un objet JavaScript correspondant au contenu JSON** "parsé" depuis la "response".

Ces deux paramètres _\(`observe` et `responseType`\)_ peuvent être manipulés pour récupérer l'objet `HttpResponse`, les événements de progression ou le contenu brut d'une "response".
{% endhint %}

{% tabs %}
{% tab title="book-search.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
    selector: 'wt-book-search',
    templateUrl: './book-search.component.html'
})
export class BookSearchComponent implements OnInit {

    bookCount: number;
    bookList: Book[];

    private _bookListUrl = 'https://www.googleapis.com/books/v1/volumes?q=extreme%20programming';

    constructor(private _httpClient: HttpClient) {
    }

    ngOnInit() {
        this._httpClient.get(this._bookListUrl)
            .subscribe(googleVolumeListResponse => {

                this.bookCount = googleVolumeListResponse.totalItems;

                // @TODO: this.bookList = ...

            });
    }

}
```
{% endtab %}
{% endtabs %}

## 3. Traitement de la "response"

### "Hint" du type de la "response"

Lors de la compilation, TypeScript ne connait pas le type des données retournées par l'API.  
Par défaut, **la méthode `get` retourne un objet de type `Observable<Object>`** . C'est à dire que `googleVolumeListResponse` est de type `Object` _\(ouJavaScript Plain Object\)._  
Cela n'est pas bloquant mais on risque de perdre l'aide du compilateur et commettre des erreurs.

Les méthodes de la classe `HttpClient` sont des méthodes génériques et il est donc **possible de contrôler leur type de retour**.

Ainsi, avec l'exemple ci-dessous :

{% tabs %}
{% tab title="google-volume-list-response.ts" %}
```typescript
export interface GoogleVolumeListResponse {
    totalItems: number;
    items: Array<{
        volumeInfo: {
            title: string;
        }
    }>;
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="book-search.component.ts" %}
```typescript
this._httpClient.get<GoogleVolumeListResponse>(url)
    .subscribe(googleVolumeListResponse => {
        this.bookCount = googleVolumeListResponse.totalItem;
    });
```
{% endtab %}
{% endtabs %}

nous obtenons l'erreur suivante :

`error TS2551: Property 'totalItem' does not exist on type 'GoogleVolumeListResponse'. Did you mean 'totalItems'?`

{% hint style="danger" %}
**ATTENTION ! Le paramètre de type `GoogleVolumeListResponse` passé à la méthode n'est qu'un "hint".**

Si l'API retourne des données sous **une forme différente** ou **si l'interface `GoogleVolumeListResponse` est erronée** ou simplement **pas à jour**, alors la **compilation va réussir** mais en revanche nous risquons de nous retrouver avec des types incohérents dans nos variables et propriétés _\(et potentiellement n'importe où dans l'application tant que les données retournées par l'API ne sont pas vérifiées en "runtime"\)_.

A titre d'exemple, si l'API renvoie le JSON suivant : **`{"totalItems": "oups!"}`** alors notre propriété **`bookCount` qui est bien de type `number`, contiendra le string `oups!`.**
{% endhint %}

{% hint style="info" %}
Si vous disposez d'une spécification OpenAPI _\(Swagger\)_ de votre API, \_\_pensez à générer ces types avec **Swagger Codegen** [https://swagger.io/swagger-codegen/](https://swagger.io/swagger-codegen/) ou directement depuis **Swagger Online Editor** [https://editor.swagger.io/](https://editor.swagger.io/).
{% endhint %}

### Transformation de la "response"

Pour éviter les problèmes décrits précédemment, il est préférable d'**éviter de se baser uniquement sur le "hint" de type** de la réponse et de le **propager dans l'application**.

Nous allons donc **transformer la "response" en une entité associée à notre "feature"**.

{% tabs %}
{% tab title="book-search.component.ts" %}
```typescript
export interface GoogleVolumeListResponse {
    totalItems: number;
    items: Array<{
        volumeInfo: {
            title: string;
        }
    }>;
}

@Component({
    selector: 'wt-book-search',
    templateUrl: './book-search.component.html',
    styleUrls: ['./book-search.component.scss']
})
export class BookSearchComponent implements OnInit {

    bookCount: number;
    bookList: Book[];
​
    private _bookListUrl = 'https://www.googleapis.com/books/v1/volumes?q=extreme%20programming';

    constructor(private _httpClient: HttpClient) {
    }

    ngOnInit() {
        this._httpClient.get<GoogleVolumeListResponse>(this._bookListUrl)
            .subscribe(googleVolumeListResponse => {

                this.bookCount = googleVolumeListResponse.totalItems;​
                this.bookList = googleVolumeListResponse.items.map(item => new Book({
                    title: item.volumeInfo.title
                }));

            });
    }

}
```
{% endtab %}
{% endtabs %}

### Attention au type de retour

{% hint style="warning" %}
Malheureusement, la classe `HttpClient` **abuse massivement de l'anti-pattern** de "method overloading".

L'anti-pattern est tellement poussé à l'extrême que **le type de retour peut changer en fonction de la valeur de certains paramètres** _\(i.e. paramètres`observe`et `responseType`\)_.
{% endhint %}

### Astuce

Si vous **consommez votre propre API**, pensez à profiter du [Duck Typing](../../typescript/duck-typing-patterns/entity-constructor.md) pour construire directement vos entités :

```typescript
this._httpClient.get<BookListResponse>('https://books.wishtack.com/api/v2/books')
    .subscribe(response => {
        this.bookList = response.items.map(item => new Book(item));
    });
```

