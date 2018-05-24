# Pourquoi HttpClient ?

Le service `HttpClient` a pour avantages de :

* **simplifier** l'implémentation d'échanges HTTP,
* fournir les **outils** nécessaires pour faciliter l'implémentation des "**tests unitaires**",
* se baser sur les **`Observable`**s permettant ainsi :
  * **d'annuler les requêtes** si nécessaire,
  * de suivre la **progression** d'"upload" et de "download",
  * d'implémenter des **wrappers** permettant de produire une "response" à partir d'un résultat temporaire en cache puis le résultat reçu depuis l'API. Cf. [https://github.com/wishtack/wishtack-steroids/tree/develop/packages/rest-cache](https://github.com/wishtack/wishtack-steroids/tree/develop/packages/rest-cache).

