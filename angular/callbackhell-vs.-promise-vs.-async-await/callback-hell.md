# Callback Hell

Supposons deux fonctions asynchrones :

```typescript
getCurrentCity(callback: (error, city: string) => void);
```

et

```typescript
getWeatherInfo(city: string, callback: (error, weatherInfo: WeatherInfo) => void);
```

Avec des "callbacks" classiques, nous sommes amenés à utiliser ces fonctions de la façon suivante :

```typescript
const handleError => error => {
    console.error(`Something went wrong but I don't know how to handle it`);
};

getCurrentyCity((error, city) => {

    if (error != null) {
        handleError(error);
        return;
    }
    
    getWeatherInfo(city, (err, weatherInfo) => {
        
        if (error != null) {
            handleError(error);
        }
        
        console.log(`${city}: ${weatherInfo.temperature}`);
        
    });
    
});
```

Ce cas est relativement simple mais manque déjà en lisibilité et **présente déjà quelques erreurs**.

### Closure Cupide

A la ligne 12, le paramètre d'erreur a été nommé `err` mais à la ligne 14, c'est le paramètre `error` du closure parent qui est utilisé.

Plus les "callbacks" se cascadent plus ce type d'erreur a de chance de se produire.

### Multi-purpose Error-First Callback

Le problème avec l'approche **Error-First Callback** utilisée dans notre exemple en suivant les conventions NodeJS, est que la même "callback" sert à deux finalités : le succès et l'échec ; avec cette approche, il arrive souvent d'omettre la gestion d'erreur et donc d'appeler l'étape suivante malgré tout.

C'est le cas de notre exemple où il nous manque un `return` après la ligne 15.

### Pas de `catch`

Il est nécessaire d'appeler de capturer et gérer les erreurs de chaque appel.

### Annulation

Il est impossible dans l'état d'annuler l'enchainement des traitements une fois lancé.

### JavaScript Async Libraries

Bien sûr, il existe des librairies "cache-misère" telles que [http://caolan.github.io/async/](http://caolan.github.io/async/) mais elles sont progressivement abandonnées pour passer aux approches abordées dans les chapitres suivants.

