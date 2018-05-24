# Testing

## Pourquoi tester ?

### Sans tests, le coût du changement augmente

En l'absence de tests _\(ou s'ils sont simplement insuffisants\)_, il est **difficile d'évaluer si l'application fonctionne** comme attendu. Il devient **nécessaire de tester l'application manuellement à chaque changement** ; ****mais comment savoir quel effet de bord attendre de chaque changement ?  
L'équipe finit par **éviter le changement pour éviter les régressions**.

### Pas de tests, pas d'update

Une application Angular a généralement quelques **millions de lignes de code** en dépendance.  
**Chaque jour**, un peu moins d'**une dizaine** de ces dépendances sont mises à jour.  
**Comment profiter des mises à jour sans introduire de régressions** ?

## Les deux familles de tests

{% page-ref page="unit-testing.md" %}

{% page-ref page="end-to-end.md" %}



