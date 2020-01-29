# Route Guards

Les "Guards" permettent de contrôler **l'accès à une "route"** _\(e.g. autorisation\)_ ou le **départ depuis une "route"** _\(e.g. enregistrement ou publication obligatoire avant départ\)_.

{% hint style="danger" %}
**Les "Guards" ne doivent en aucun cas être considérés comme un mécanisme de sécurité.**

La gestion de **permission des accès aux ressources doit se faire au niveau des APIs HTTP** : [https://blog.wishtack.com/api-rest-bonnes-pratiques-et-securite/](https://blog.wishtack.com/api-rest-bonnes-pratiques-et-securite/).

Les "Guards" servent à améliorer la "User eXperience" en évitant par exemple l'accès à des "routes" qui ne fonctionneraient pas car l'accès aux données serait rejeté par l'API.
{% endhint %}

## Configuration

Les "Guards" sont ajoutés au niveau de la configuration du "Routing" :

```typescript
export const appRouteList = [
    {
        path: 'cart',
        loadChildren: './views/cart/cart-routing.module#CartRoutingModule',
        canActivate: [
            IsSignedInGuard
        ]
    },
    {
        path: 'signin',
        component: SigninViewComponent,
        canActivate: [
            IsNotSignedInGuard
        ]
    },
    {
        path: 'profile',
        component: ProfileViewComponent,
        canDeactivate: [
            IsNotDirtyGuard
        ]
    }
]
```

## `CanActivate`

Une "Guard" d'activation est un service qui implémente l'**interface `CanActivate`**.

Ce service doit donc **implémenter la méthode `canActivate`**.  
Cette méthode est **appelée à chaque demande d'accès à la "route"** ; elle doit alors **retourner une valeur de type `boolean` ou `Promise<boolean>` ou `Observable<boolean>`** indiquant si l'accès à la "route" est autorisé ou non.

Il est donc possible d'attendre le résultat d'un traitement asynchrone pour décider d'autoriser l'accès ou non.

{% tabs %}
{% tab title="is-signed-in.guard.ts" %}
```typescript
@Injectable({
    providedIn: 'root'
})
export class IsSignedInGuard implements CanActivate {

    constructor(private _session: Session) {
    }

    canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
        return this._session.isSignedIn();
    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
En cas de refus d'accès, il est possible de **rediriger l'utilisateur vers une autre "route"** "manuellement" en injectant le service "Router" par exemple :

```typescript
    constructor(private _router: Router,
                private _session: Session) {
    }

    canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {

        const isSignedIn = this._session.isSignedIn();

        if (isSignedIn !== true) {
            this._router.navigate([...]);
        }

        return isSignedIn;

    }
```
{% endhint %}

## `CanDeactivate`

Les "Guards" de désactivation sont **couplées aux composants** car elles doivent **communiquer avec le composant** pour établir leur décision d'accès.

Une "Guard" de désactivation est un service qui implémente l'**interface `CanDeactivate`**.

Ce service doit donc **implémenter la méthode `canDeactivate`**.  
Cette méthode est **appelée à chaque fois que l'utilisateur souhaite quitter la route** _\(clic sur un lien ou déclenchement automatique\)_ ; elle doit alors **retourner une valeur de type `boolean` ou `Promise<boolean>` ou `Observable<boolean>`** indiquant si l'accès à la "route" est autorisé ou non.

Contrairement au "Guards" d'activation, cette "Guard" prend en **premier paramètre l'instance du composant**. C'est pour cette raison que l'interface `CanDeactivate` est générique.

{% hint style="warning" %}
La "Guard" appelle la méthode `isDirty` du composant `ProfileViewComponent` pour décider d'autoriser ou non l'utilisateur à quitter la route.

Malheureusement, la "Guard" est fortement couplée avec le composant `ProfileViewComponent`.

```typescript
@Injectable({
    providedIn: 'root'
})
export class IsNotDirtyGuard implements CanDeactivate<ProfileViewComponent> {

    canDeactivate(component: ProfileViewComponent,
                  currentRoute: ActivatedRouteSnapshot,
                  currentState: RouterStateSnapshot,
                  nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
        return component.isDirty();
    }

}
```

```typescript
export class ProfileViewComponent {

    isDirty() {
        return false;
    }

}
```
{% endhint %}

{% hint style="success" %}
Pensez à associer la "Guard" à une interface plutôt qu'au composant directement.

```typescript
export interface IsDirty {
    isDirty(): boolean | Observable<boolean>;
}

@Injectable({
    providedIn: 'root'
})
export class IsNotDirtyGuard implements CanDeactivate<IsDirty> {

    canDeactivate(component: IsDirty,
                  currentRoute: ActivatedRouteSnapshot,
                  currentState: RouterStateSnapshot,
                  nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
        return component.isDirty();
    }

}
```

```typescript
export class ProfileViewComponent implements IsDirty {

    isDirty() {
        return false;
    }

}
```
{% endhint %}

