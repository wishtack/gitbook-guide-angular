# Mise en Place du Routing

## 1. Tag `base`

Avant toute chose, il est nécessaire d'ajoute le tag `base` au `head` du fichier `index.html` de l'application :

{% tabs %}
{% tab title="src/index.html" %}
```markup
<!doctype html>
<html>
    <head>
        <base href="/">
        ...
    </head>
    <body>
        ...
    </body>
</html>
```
{% endtab %}
{% endtabs %}

Ce tag indique **la base du `path` à partir de laquelle le "Routing" Angular rentre en jeu**.

Cette valeur est généralement personnalisé dans le cas où plusieurs applications Angular sont hébergées sur un **même nom de domaine mais avec des `path`s différents**.

## 2. Configuration

### Configuration du "Routing"

La configuration du "Routing" est transmise au module `RouterModule` lors de son import par le "root module" `AppModule`.

{% hint style="success" %}
Par bonne pratique, il est recommandé de placer cette configuration dans un module dédié `AppRoutingModule` importé par le "root module" `AppModule`.
{% endhint %}

{% tabs %}
{% tab title="src/app/app-routing.module.ts" %}
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

export const appRouteList: Routes = [
    {
        path: 'landing',
        component: LandingViewComponent
    },
    {
        path: 'search',
        component: BookSearchViewComponent
    },
    {
        path: '**',
        redirectTo: 'landing'
    }
];

@NgModule({
    exports: [
        RouterModule
    ],
    imports: [
        RouterModule.forRoot(appRouteList)
    ]
})
export class AppRoutingModule {
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**\`**\` est une "wildcard"\*\* qui "match" toutes les urls _\(sauf celles qui ont "match" les routes précédentes\)_.

**Il faut donc faire attention à l'ordre des "routes".**
{% endhint %}

### Configuration d'une "Route" avec Paramètres

Le `path` d'une "route" peut définir des paramètres obligatoires grâce au préfixe `:`.

```typescript
export const appRouteList: Routes = [
    ...,
    {
        path: 'books/:bookId',
        component: BookDetailViewComponent
    },
    ...
];
```

### Configuration de l'Hébergement

{% hint style="info" %}
Pour le bon fonctionnement du "Routing", **il est important d'implémenter une règle de "rewrite" sur votre plateforme d'hébergement** afin que toutes les "routes" renvoient le même fichier `index.html` produit dans le dossier `dist` lors du "build" _\(`yarn build`\)_.

Autrement, en accédant directement à une "route" de l'application, l'utilisateur obtiendrait une erreur 404.
{% endhint %}

## 3. `<router-outlet>`

La configuration du "Routing" permet de définir quel composant afficher en fonction de la route mais cela n'indique pas à Angular **où injecter le composant** dans la page.

Pour **indiquer l'emplacement d'insertion du composant**, il faut utiliser la **directive `<router-outlet>`** directement dans le "root component" `AppComponent` _\(ou dans un "child component" dédié, e.g. `HomeLayoutComponent`\)_.

{% tabs %}
{% tab title="app.component.html" %}
```markup
<header>
...
</header>
<router-outlet></router-outlet>
<footer>
...
</footer>
```
{% endtab %}
{% endtabs %}

En fonction de la "route" visitée, le **composant associé sera alors injecté en dessous du tag `router-outlet`** _\(et non à l'intérieur ou à la place du tag contrairement à ce que l'on pourrait supposer\)_.

## 4. Création de liens

En utilisant des liens natifs `<a href="/search">`, le "browser" va produire une requête HTTP `GET` vers le serveur et recharger toute l'application.

Pour éviter ce problème, **le module de "Routing" Angular fournit la directive `routerLink`** qui permet d'**intercepter l'événement `click`** sur les liens et de **changer de "route" sans recharger toute l'application**.

```markup
<a routerLink="/search">Search</a>
```

{% hint style="info" %}
La directive `routerLink` **génère tout de même l'attribut `href`** pour **faciliter la compréhension de la page** par les "browsers" ou "moteurs de recherche". Cela permet par exemple, d'**ouvrir un lien dans une nouvelle fenêtre** grâce au menu contextuel ou encore **copier le lien** d'une "route".
{% endhint %}

### Construction Dynamique

La "route" peut être construite dynamiquement et passée à l'[Input](../interaction-entre-composants/input.md) `routerLink`.

```typescript
this.route = '/search';
this.routeName = 'Search';
```

```markup
<a [routerLink]="route">{{ routeName }}</a>
```

### Construction avec Paramètres

La "route" `/books/123` peut être construite avec des paramètres :

```markup
<a [routerLink]="['/books', book.id]">{{ routeName }}</a>
```

où `book.id = '123'`.

Il est également possible de **passer des paramètres optionnels par "query string"** via l'[`Input`](../interaction-entre-composants/input.md) `queryParams`.

```markup
<a
    routerLink="'/search"
    [queryParams]="{keywords: 'eXtreme Programming'}">eXtreme Programming Books</a>
```

## 5. Accès aux Paramètres

Le service **`ActivatedRoute` décrit l'état** actuel du "router".  
Il permet au composant associé à la "route" de **récupérer les paramètres via les propriétés `paramMap` et `queryParamMap`**.

{% hint style="info" %}
Les propriétés **`paramMap` et `queryParamMap` sont des `Observable`s** car par optimisation, en **naviguant vers la même route mais avec des paramètres différents** _\(e.g. `/books/123` =&gt; `/books/456`\)_, Angular **ne recharge pas le composant** mais propage les nouveaux paramètres via ces `Observable`s.
{% endhint %}

{% tabs %}
{% tab title="book-detail-view.component.ts" %}
```typescript
export class BookDetailViewComponent {

    book$: Observable<Book>;

    constructor(private _activatedRoute: ActivatedRoute,
                private _bookRepository: BookRepository) {

        this.book$ = this._activatedRoute.paramMap
            .pipe(
                map(paramMap => paramMap.get('bookId')),
                switchMap(bookId => this._bookRepository.getBook(bookId))
            );

    }

}
```
{% endtab %}
{% endtabs %}

Pour simplifier la récupération des paramètres, **il est également possible d'utiliser la propriété** **`snapshot`** qui contient l'état actuel de la route _\(e.g. : `snapshot.paramMap.get('bookId')`\)_. **Le risque dans ce cas est de ne pas mettre à jour la vue** en cas de navigation vers la même route avec des paramètres différents.

## 6. `Router` Service

Le `Router` est le service principal du "Routing". Il permet de :

* déclencher la navigation via la méthode `navigate`, `this._router.navigate(['/books', bookId], {queryParams: {}})`
* suivre les événements de navigation via l'`Observable` `router.events`,
* construire et parser les urls de "Routing",
* vérifier si une "Route" est actuellement visitée,
* ...

