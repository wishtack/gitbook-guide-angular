# Class vs Injection Token

Une dépendance peut être de type `string` ou "objet literal" ou même sans type. Cf. [https://angular.io/api/core/InjectionToken](https://angular.io/api/core/InjectionToken).

{% hint style="danger" %}
Pour des raisons de lisibilité et de maintenabilité, évitez l'utilisation d'`InjectionToken` et préférez l'utilisation de classes.
{% endhint %}

Au lieu de :

```typescript
export const API_BASE_URL = new InjectionToken<string>('ApiBaseUrl');

@NgModule({
    providers: [
        {
            provide: API_BASE_URL,
            useValue: 'https://api.wishtack.io/api/v2'
        }
    ]
})
export class AppModule {
}

@Component({
    ...
})
export class AppComponent {

    constructor(@Inject(API_BASE_URL) apiBaseUrl: string) {
    }
    
}
```

... préférez donc l'approche ci-dessous afin de bénéficier sans effort du "typing" TypeScript et éviter l'utilisation du décorateur `@Inject()`.

```typescript
export class Config {
    readonly apiBaseUrl: string;    
}

@Injectable()
export class CustomConfig extends Config {
    apiBaseUrl = 'https://api.wishtack.io/api/v2';
}

@NgModule({
    providers: [
        {
            provide: Config,
            useClass: CustomConfig
        }
    ]
})
export AppModule {
}

@Component({
    ...
})
export class AppComponent {
​
    constructor(config: Config) {
        this.apiBaseUrl = config.apiBaseUrl;
    }
    
}
```



