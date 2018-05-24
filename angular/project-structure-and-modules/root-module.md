# Root Module

Une application Angular contient un seul et unique "root module" _\(`AppModule` par défaut\)_.

Le "root module" est un module classique dont la particularité est de définir le "root component" de l'application via la propriété `bootstrap`.

{% code-tabs %}
{% code-tabs-item title="src/app/app.module.ts" %}
```typescript
@NgModule({

    declarations: [
        AppComponent
    ],
    imports: [
        BookModule
    ],
    bootstrap: [
        AppComponent
    ]
})
export class AppModule {
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> `bootstrap` est une liste car dans certains cas extrêmes, il est possible d'avoir plusieurs "root components".

> Une alternative à la propriété `bootstrap` est de surcharger la méthode `ngDoBootstrap`.

Le module `AppModule` est désigné comme "root module" via la ligne suivante du fichier `main.ts`.

{% code-tabs %}
{% code-tabs-item title="src/main.ts" %}
```typescript
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Au démarrage de l'application, Angular recherche dans le DOM _\(Cf. `src/index.html`\)_, **le premier élément** correspondant au **sélecteur du composant** `AppComponent` _\(`wt-app`\)_ et injecte alors le composant à cet endroit.

