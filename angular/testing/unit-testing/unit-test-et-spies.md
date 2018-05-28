# Unit-Test et Spies

Il est parfois nécessaire de **"mocker" les dépendances du code testé** pour éviter de tester une unité de code trop large, simuler des cas d'erreur par exemple ou tout simplement éviter d'appeler du code qui n'est pas encore implémenté.

## Jasmine Spies

Pour répondre à ce besoin, [Jasmine](jasmine.md) offre nativement les fonctions `createSpy`, `createSpyObj` et `spyOn` qui permettent de simuler le comportement d'un objet, d'une fonction ou d'une méthode.

## `spyOn`

`spyOn` permet de contrôler une méthode en modifiant son comportement et en analysant les appels reçus.

```typescript
import { Injectable } from '@angular/core';
import { fakeAsync, flush, TestBed } from '@angular/core/testing';
import { Observable, of } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable({
    providedIn: 'root'
})
export class Geolocation {

    getCoordinates(city): Observable<Coordinates> {
        throw new NotImplementedError();
    }

}

@Injectable({
    providedIn: 'root'
})
export class PickyWeatherStation {

    constructor(private _geolocation: Geolocation) {
    }

    getTemperature(city: string) {

        return this._geolocation.getCoordinates(city)
            .pipe(
                map(coordinates => Math.round((coordinates.longitude + coordinates.latitude) / 2))
            );

    }

}

describe('PickyWeatherStation', () => {

    let geolocation: Geolocation;
    beforeEach(() => geolocation = TestBed.get(Geolocation));

    let weatherStation: PickyWeatherStation;
    beforeEach(() => weatherStation = TestBed.get(PickyWeatherStation));

    it('should get random temperature', fakeAsync(() => {

        let temperature: number;

        spyOn(geolocation, 'getCoordinates').and.returnValue(of({
            latitude: 45.764043,
            longitude: 4.835659
        }));

        weatherStation.getTemperature('Lyon')
            .subscribe(_temperature => temperature = _temperature);

        /* Flush any async tasks. */
        flush();

        expect(temperature).toBe(25);

        /* Check that getCoordinates has been called once with the right parameters. */
        expect(geolocation.getCoordinates).toHaveBeenCalledTimes(1);
        expect(geolocation.getCoordinates).toHaveBeenCalledWith('Lyon');

    }));

});

```

{% hint style="warning" %}
**Pensez à toujours vérifier que vos "spies" sont appelés avec les bons paramètres et le bon nombre de fois.**
{% endhint %}

