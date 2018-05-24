# Testing

## Pourquoi Tester ?

### Le coût du changement

Sans tests, le coût du changement augmente exponentiellement.

En l'absence de tests _\(ou s'ils sont simplement insuffisants\)_, il est **difficile d'évaluer si l'application fonctionne** comme attendu. Il devient **nécessaire de tester l'application manuellement à chaque changement** ; ****mais comment choisir les parties à tester et quels effets de bord attendre de chaque changement ?  
L'équipe finit par **éviter le changement pour éviter les régressions**.

### Cross-Browser et Cross-Platform

Les tests manuels peuvent s'avérer très rapidement couteux, particulièrement si l'on souhaite découvrir les anomalies spécifiques à certains browsers ou "devices" avant l'utilisateur final.

### Pas de tests, pas d'update

Une application Angular a généralement quelques **millions de lignes de code** en dépendance.  
**Chaque jour**, un peu moins d'**une dizaine** de ces dépendances sont mises à jour.  
**Comment profiter des mises à jour sans introduire de régressions** ?

### "Tests are Specs"

Les tests forment à la fois les spécifications et la documentation de l'application.

Pour découvrir _\(ou mieux comprendre\)_ certaines fonctionnalités non documentées _\(ou dont la documentation n'est pas à jour\)_ d'un module open-source _\(e.g. Angular\)_, les tests s'avèrent très intéressants comme source d'information.

Contrairement à des spécifications ou documentations, les tests ont l'avantage d'être toujours à jour.

## Les Familles de Tests

Nous aborderons ici les deux principales familles de tests Angular suivantes :

{% page-ref page="unit-testing.md" %}

{% page-ref page="end-to-end.md" %}



