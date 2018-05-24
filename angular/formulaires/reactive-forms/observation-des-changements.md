# Observation des Changements

La propriété `valueChanges` est l'une des propriétés les plus importantes des "controls". Il s'agit d'un `Observable` permettant d'**adopter une approche réactive en détectant tous les changements apportés aux données du "control"** _\(le `FormGroup` regroupant tous les "controls" est lui-même un "control"\)_.

A titre d'exemple, le code suivant permet de construire un `Observable` contenant la liste des résultats associés à la recherche de l'utilisateur.  
L'opérateur `debounceTime` permet d'attendre 100ms d'inactivité avant de produire une requête.  
Grâce à l'opérateur `switchMap`, chaque requête en cours est annulée avant d'en produire une nouvelle.

```typescript
this.bookList$ = this.bookForm.valueChanges
    .pipe(
        debounceTime(100),
        switchMap(value => this._bookRepository.search({keywords: value.title}))
    );
```

{% hint style="info" %}
Tant que l'`Observable` n'est pas consommé _\(via un `subscribe`\)_, aucun traitement n'est exécuté.
{% endhint %}



