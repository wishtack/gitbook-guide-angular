# Unit-Test et HttpClient

Pour "mocker" les appels HTTP, pas besoin d'utiliser les "spies" sur les méthodes de la classe `HttpClient`.

**Le module `HttpClientTestingModule` fournit les services nécessaires pour contrôler les échanges HTTP.**

```typescript
describe('PickWeatherStation', () => {

    let httpTestingController: HttpTestingController;

    beforeEach(async(() => {
        TestBed.configureTestingModule({
          imports: [ HttpClientTestingModule ]
        })
            .compileComponents();
    }));
    
    let weatherStation: PickyWeatherStation;
    beforeEach(() => weatherStation = TestBed.get(PickyWeatherStation));

    let httpTestingController: HttpTestingController;
    beforeEach(() => httpTestingController = TestBed.get(HttpTestingController));
    
    afterEach(() => {
        httpTestingController.verify();
    });
    
    it('should get temperature from API', fakeAsync(() => {
    
        let temperature: number;

        weatherStation.getTemperature('Lyon')
            .subscribe(_temperature => temperature = _temperature);

        const req = httpTestingController.expectOne('/city-weather/lyon');

        req.flush({
            temperature: 30
        });

        expect(temperature).toEqual(30);
    
    }));
    
});
```

{% hint style="success" %}
Pensez à toujours ajouter l'`afterEach` ci-dessous afin de vérifier qu'il n'y a aucune requête en attente ! 

```typescript
afterEach(() => {
    httpTestingController.verify();
});
```
{% endhint %}

