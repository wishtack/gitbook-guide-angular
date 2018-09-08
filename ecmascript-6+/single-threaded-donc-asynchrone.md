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

## Event Loop

### Comportement de l'Event Loop

Quel est l'ordre d'exécution ?

{% tabs %}
{% tab title="🧐" %}
```javascript
var value;

setTimeout(function () {
    value = 'VALUE';
}, 100 /* 100 ms. */);

console.log(value); // ???

setTimeout(function () {
    console.log(value); // ???
}, 200);
```
{% endtab %}

{% tab title="👍" %}
```javascript
var value;

setTimeout(function () {
    value = 'VALUE';
}, 100 /* 100 ms. */);

console.log(value); // 1 - undefined

setTimeout(function () {
    console.log(value); // 2 - VALUE
}, 200);
```
{% endtab %}
{% endtabs %}

Et dans ce cas ?

{% tabs %}
{% tab title="🧐" %}
```javascript
function main() {

    var value;

    setTimeout(function () {
        value = 'VALUE';
    }, 0 /* 0 ms. */);

    console.log(value); // ???

    setTimeout(function () {
        console.log(value); // ???
    }, 0);
    
    console.log(value); // ???

}

main();
```
{% endtab %}

{% tab title="👍" %}
```javascript
function main() {

    var value;

    setTimeout(function () {
        value = 'VALUE';
    }, 0 /* 0 ms. */);

    console.log(value); // 1 - undefined

    setTimeout(function () {
        console.log(value); // 3 - VALUE
    }, 0);
    
    console.log(value); // 2 - undefined
    
}

main();
```
{% endtab %}
{% endtabs %}

### Fonctionnement de l'Event Loop

1. **Inscription d'un listener** : Tous les traitements asynchrones nécessitent la définition d'une fonction de callback afin de récupérer le résultat du traitement ou simplement savoir que le traitement a abouti. Lors de cette étape, nous inscrivons explicitement ou indirectement un listener \(notre callback\) sur un événement. Exemples: `setTimeout`, `addEventListener` etc... 
2. **Ajout de la fonction à la queue** : Cette fonction de callback ne peut pas être appelée immédiatement dès réception du résultat car le thread est probablement occupé par l'exécution d'une autre fonction. Dans notre cas, nos deux fonctions de callback sont prêtes à être appelées mais le thread est occupé par l'exécution de notre fonction `main`. Le moteur JavaScript ajoute donc les fonctions de callback à la fin d'une "queue" de fonctions à appeler quand le thread sera libre. 
3. **Tick** : A la fin de l'exécution de la fonction `main`, le thread n'a pas le temps de s'ennuyer et va récupérer la fonction en tête de "queue" pour l'exécuter. Plus précisément, il s'agit ici de l'"event loop" qui est simplement la boucle infinie qui lance les fonctions les unes après les autres. **L'instant où l'"event loop" récupère une nouvelle fonction à exécuter s'appelle un "tick".**

![Event Loop](../.gitbook/assets/event-loop.jpg)

{% embed data="{\"url\":\"https://www.youtube.com/watch?v=8aGhZQkoFbQ\",\"type\":\"video\",\"title\":\"Philip Roberts: What the heck is the event loop anyway? \| JSConf EU 2014\",\"description\":\"JavaScript programmers like to use words like, “event-loop”, “non-blocking”, “callback”, “asynchronous”, “single-threaded” and “concurrency”.\\n\\nWe say things like “don’t block the event loop”, “make sure your code runs at 60 frames-per-second”, “well of course, it won’t work, that function is an asynchronous callback!”\\n\\nIf you’re anything like me, you nod and agree, as if it’s all obvious, even though you don’t actually know what the words mean; and yet, finding good explanations of how JavaScript actually works isn’t all that easy, so let’s learn!\\n\\nWith some handy visualisations, and fun hacks, let’s get an intuitive understanding of what happens when JavaScript runs.\\n\\nTranscript: http://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html\\n\\nLicense: For reuse of this video under a more permissive license please get in touch with us. The speakers retain the copyright for their performances.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.youtube.com/yts/img/favicon\_144-vfliLAfaB.png\",\"width\":144,\"height\":144,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://i.ytimg.com/vi/8aGhZQkoFbQ/maxresdefault.jpg\",\"width\":1280,\"height\":720,\"aspectRatio\":0.5625},\"embed\":{\"type\":\"player\",\"url\":\"https://www.youtube.com/embed/8aGhZQkoFbQ?rel=0&showinfo=0\",\"html\":\"<div style=\\\"left: 0; width: 100%; height: 0; position: relative; padding-bottom: 56.2493%;\\\"><iframe src=\\\"https://www.youtube.com/embed/8aGhZQkoFbQ?rel=0&amp;showinfo=0\\\" style=\\\"border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;\\\" allowfullscreen scrolling=\\\"no\\\"></iframe></div>\",\"aspectRatio\":1.7778},\"caption\":\"What the heck is the event loop anyway?\"}" %}



