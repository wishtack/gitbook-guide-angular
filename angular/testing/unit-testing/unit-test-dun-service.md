# Unit-Test d'un Service

Il serait dommage de ne pas profiter de la [Dependency Injection](../../dependency-injection/) lors de l'implémentation des tests unitaire.

Grâce à la fonction `inject` _\(du module `@angular/core/testing`\),_ il est possible d'injecter des services dans les tests unitaire.

Cette fonction prend deux paramètres :

* **la liste des tokens à injecter** _\(généralement liste des classes à injecter\),_
* **une fonction de "callback"** qui prend en paramètre **la liste des services injectés** dans le même ordre que la liste de tokens.

{% hint style="danger" %}
Attention à **ne pas confondre la fonction `inject`** du module **`@angular/core/testing`** avec celle du module **`@angular/core`** qui sert à injecter des services dans les factories `factory: () => new MyService(inject(MyDep))` _\(Cf._ [_https://github.com/angular/angular/blob/master/packages/examples/core/di/ts/injector\_spec.ts\#L70_](https://github.com/angular/angular/blob/master/packages/examples/core/di/ts/injector_spec.ts#L70)_\)_.
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="picky-weather-station.ts" %}
```typescript
@Injectable({
    providedIn: 'root'
})
class PickyWeatherStation {

    getTemperature(city): Observable<number> {
        return of(42);
    }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="picky-weather-station.spec.ts" %}
```typescript
describe('PickyWeatherStation', () => {

    let weatherStation: PickyWeatherStation;

    beforeEach(inject([PickyWeatherStation], _weatherStation => weatherStation = _weatherStation));

    it('should give temperature', fakeAsync(() => {

        let temperature;

        weatherStation.getTemperature('Lyon')
            .subscribe(_temperature => temperature = _temperature);

        expect(temperature).toBe(42);

    }));

});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="success" %}
La fonction `inject` peut être utilisée **directement autour de la fonction de "spec"** mais il est généralement plus simple d'ajouter un **`beforeEach` pour chaque service** afin d'éviter les problèmes liés à l'ordre des tokens d'injection et le refactoring en général.
{% endhint %}

