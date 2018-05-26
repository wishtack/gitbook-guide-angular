# Async / Await

Les [Promises](promise.md) sont intéressantes mais loin d'être aussi claires que du code synchrone.

## Le rêve

Dans le dernier exemple du chapitre précédent, nous obtenons le résultat suivant :

```typescript
getCurrentCity()
    .then(city => getWeatherInfo(city))
    .then(weatherInfo => console.log(weatherInfo.temperature))
    .catch(error => console.error(error));
```

... qui est intéressant mais loin d'être aussi clair que du code synchrone :

```typescript
try {

    const city = getCurrentCity();
    const weatherInfo = getWeatherInfo(city);
    
    console.log(weatherInfo.temperature);
    
}
catch (error) {
    console.error(error);
}
```

## Fonctionnement d'Async / Await

L'approche "async / await" est un concept introduit en ECMAScript 7 _\(ou ES2016\)_ et inspiré du C\# _\(on le retrouve dans les langages les plus cools du marché tels que Python\)._

"async / await" est une **"syntactic sugar" autour des `Promise`s** permettant d'**indiquer explicitement** les endroits où une **fonction asynchrone attend le résultat d'une `Promise`**.  
En arrivant à ces endroits, un tick est déclenché et **l'"event loop" reprend la main**.  
Quand la **`Promise`** est "resolved", la fonction reprend son exécution à l'endroit où elle a été interrompue dès que possible _\(i.e. : une fois que toutes les tâches empilées dans la "queue" de l'"event loop" sont dépilées\)_.

On obtient alors le résultat suivant :

```typescript
const getCurrentCity = () => {
    /* Return an already resolved promise. */
    return Promise.resolve('Lyon');
};

const getWeatherInfo = (city) => {
    /* Return an already resolved promise. */
    return Promise.resolve({
        temperature: city.length * 6
    });
};

const main = async () => {

    try {

        const city = await getCurrentCity();
        const weatherInfo = await getWeatherInfo(city);

        console.log(`${city}: ${weatherInfo.temperature}`);

    }
    catch (error) {
        console.error(error);
    }

};

main()
    .then(() => console.log('Finished'))
    .catch(() => console.error('Failed!'));
```

{% hint style="warning" %}
`await` ne peut être utilisé que dans une fonction `async`.
{% endhint %}

{% hint style="info" %}
Une fonction `async` peut être appelée depuis n'importe quelle fonction et son type de retour sera une **`Promise`**.
{% endhint %}

