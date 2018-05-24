# Formulaires

Il existe différentes façon d'implémenter les formulaires avec Angular.

Ce guide aborde **les deux approches les plus répandues** :

* [**Template-driven Forms**](template-driven-forms.md) _**\(à éviter\)**_ : inspirée du "two-way binding" utilisé dans AngularJS, cette approche a de nombreuses limitations et s'avère rapidement **fastidieuse à implémenter**, **peu extensible** et **peu efficace**. 
* [**Reactive Forms**](reactive-forms/) _**\(à adopter\)**_ : cette approche vient appuyer le paradigme "Reactive Programming" qui fait parti des fondements d'Angular avec : une meilleure **séparation de la logique du formulaire et de la vue**,  une **meilleure testabilité**, des `Observable`s, la **génération dynamique de formulaires** etc...

{% hint style="danger" %}
Il existe d'autres approches telles que celle décrite dans la documentation officielle [https://angular.io/guide/user-input](https://angular.io/guide/user-input) qu'il faut absolument éviter pour les raisons suivantes :

* **Couplage fort avec le DOM** et le "low level" en général.
* Ce sont des approches impératives qui ne respectent pas l'approche "Reactive" d'Angular _\(Cf._ [_Violations du MVC Angular_](../composants/lapproche-mvc.md#violations-du-mvc-angular)_\)_.
{% endhint %}





