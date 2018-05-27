# TestBed

La classe **`TestBed`** est une classe Angular permettant principalement de **créer un environnement de test émulant le fonctionnement d'un module Angular**.

La méthode statique `configureTestingModule` prend en paramètre **une configuration partiellement similaire à `@NgModule()`** qui permet de déclarer les composants à tester ou les `providers` des services à tester ou **encore mieux importer le module contenant le code à tester**.

```typescript
    beforeEach(async(() => {
        TestBed.configureTestingModule({
            imports: [
                BookModule
            ]
        }).compileComponents();
    }));
```

{% hint style="success" %}
Utilisez `imports` pour **importer le module contenant le composant ou service \(ou autre\) à tester** afin d'éviter de redéfinir les imports des dépendances nécessaires.

Cela permet aussi de s'assurer que le module à tester \(`BookModule`\) est autonome et qu'il est importe bien ses propres dépendances.
{% endhint %}

{% hint style="info" %}
La méthode `compileComponents` est asynchrone _\(car elle télécharge les templateUrl dans des environnement hors CLI\)_ et retourne une `Promise`. C'est pour cette raison que le `beforeEach` de configuration utilise la fonction [`async`](unit-test-asynchrone.md).
{% endhint %}

