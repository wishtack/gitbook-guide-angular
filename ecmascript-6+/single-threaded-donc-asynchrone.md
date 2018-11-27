# "Single-Threaded" donc Asynchrone

## Multi-thread vs mono-thread

Dans un univers data-driven synchrone et "multi-threaded"...

```javascript
function processRequest(request) {

    /*
     * Pre-processing.
     */
    var query = prepareQuery(...);

    var result = execQuery(query); // might take few seconds...

    /*
     * Post-processing.
     */
    var response = createResponse(result);

    return response;

}
```

...mais dans un univers "single-threaded" cela peut être catastrophique car l'application est bloquée tant qu'elle est en attente.

Tant qu'on ne sort pas de la fonction, aucune autre fonction ne peut être exécutée.

## Besoin de traitement asynchrone

```javascript
function processRequest(request, callback) {

    /*
     * Pre-processing.
     */
    var query = prepareQuery(...);

    execQuery(query, function (result) {

        /*
         * Post-processing.
         */
        var response = createResponse(result);

        callback(response);

    });

}
```

## Avantages de l'approche asynchrone

* Pas de limitation due au nombre de threads.
* Pas de locks ou sémaphores.
* Pas de locks gourmants.
* Pas de deadlock.
* Les données ne peuvent pas varier lors de l'exécution d'une fonction synchrone.

## Closure

Un "closure" est la combinaison d'une définition de fonction ainsi que l'environnement lexical dans lequel cette dernière est définie.  
Les "closures" déterminent la portée des variables.

Les variables sont accessibles en lecture / écriture dans le "closure" où elles sont déclarées mais également dans les fonctions définies à l'intérieur.

```javascript
function main() {

    var userName = 'Foo';

    function setUserName(value) {
        userName = value;
    }

    function getUserName() {
        return userName;
    }

    setUserName('John');

    console.log(getUserName()); // 'John';

}

main();
```



