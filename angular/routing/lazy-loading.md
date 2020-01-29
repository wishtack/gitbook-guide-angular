# Lazy Loading

En configurant l'intégralité du `Routing` de l'application dans le module `AppRoutingModule`, on serait amené à **importer tous les modules de l'application avant son démarrage**. A titre d'exemple, plus l'application sera riche, plus la page d'accueil sera lente à charger par effet de bord.

Pour éviter ces problèmes de "scalability", **Angular permet de charger les modules à la demande** _\(i.e. : "Lazy Loading"\)_ afin de ne pas gêner le chargement initial de l'application.

## Configuration du Lazy Loading

La configuration du "Lazy Loading" se fait au niveau du "Routing".

Le module de "Routing" `AppRoutingModule` peut **déléguer la gestion du "Routing" d'une partie de l'application à un autre module**. Ce module "Lazy Loaded" sera donc **chargé de façon asynchrone à la visite des "routes" dont il est en charge**.

{% tabs %}
{% tab title="tu" %}
```typescript
@NgModule({
    declarations: [
        AppComponent,
        LandingComponent
    ],
    imports: [
        AppRoutingModule,
        BrowserModule
    ],
    bootstrap: [AppComponent]
})
export class AppModule {
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="src/app/app-routing.module.ts" %}
```typescript
export const appRouteList: Routes = [
    {
        path: 'landing',
        component: LandingComponent
    },
    {
        path: 'book',
        loadChildren: './views/book/book-routing.module#BookRoutingModule'
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
        HttpClientModule,
        RouterModule.forRoot(appRouteList)
    ]
})
export class AppRoutingModule {
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="src/app/views/book/book-routing.module.ts" %}
```typescript
export const bookRouteList: Routes = [
    {
        path: 'search',
        component: BookSearchComponent
    },
    {
        path: '**',
        redirectTo: 'search'
    }
];

@NgModule({
    imports: [
        BookModule,
        RouterModule.forChild(bookRouteList)
    ]
})
export class BookRoutingModule {
}
```
{% endtab %}
{% endtabs %}

Cette configuration **délègue le "Routing"** de toute la partie **`/book/...`** de l'application **au module `BookRoutingModule`**.

{% hint style="danger" %}
Pour profiter du "Lazy Loading", **assurez-vous que les modules "Lazy Loaded" ne sont jamais chargé explicitement** _**\("Eagerly Loaded"\)**_ **!**

Il faut donc **épurer au maximum les `imports` d'`AppModule`**.
{% endhint %}

{% hint style="info" %}
La syntaxe `loadChildren: './views/book/book-routing.module#BookRoutingModule'` est un raccourci pour le chargement asynchrone de la classe `BookRoutingModule` :

```typescript
loadChildren: () => import('./views/book/book-routing.module')
    .then(module => module.BookRoutingModule);
```
{% endhint %}

{% hint style="info" %}
`BookRoutingModule` est à la fois un [Routed Feature Module](../project-structure-and-modules/feature-module.md) et un [Routing Module](../project-structure-and-modules/feature-module.md).
{% endhint %}

### Résultat du "build"

En analysant le résultat du "build" dans le dossier `dist`, on peut remarquer la création d'un nouveau fichier `0.9b8cc2f6fc3b76db8fd7.js`. Il s'agit du "**chunk**" contenant le code associé à la "Routed Feature Module" `BookRoutingModule`.

Tant que `BookModule` n'est importé que par `BookRoutingModule`, tout le code associé à ce module sera inclus dans le même "chunk".

## `forRoot` vs `forChild`

**Seul le module `AppRoutingModule` importe le module `RouterModule` avec la méthode statique `forRoot`** afin de définir le "Routing" racine et la configuration du router via le second paramètre.

Les "**Child Routing Modules"** importent le `RouterModule` avec la méthode `forChild`.

## Preloading Strategy

Une fois l'application démarrée et **pour éviter la latence de chargement des "Lazy Loaded Routes"**, il est possible de configurer le "Routing" pour **précharger tous les modules "Lazy Loaded" juste après le démarrage de l'application**.

```typescript
RouterModule.forRoot(appRouteList, {
    preloadingStrategy: PreloadAllModules
})
```

