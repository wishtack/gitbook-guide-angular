# Reactive Programming

L'idée du Reactive Programming est d'adopter au maximum une **approche déclarative plutôt qu'impérative.**

Le Reactive Programming permet de **réduire le couplage entre les actions, les données, les traitements et le résultat des traitements**. Cela améliore également la **résilience** et la "**scalability**" de l'architecture.

Le succès de cette approche est fortement lié à l'émergence des **Microservices**, des technologies telles que **Kafka Streams**, ****en frontend à des frameworks tels qu'Angular et à des plateformes de streaming telles que Netflix.

![Reactive Programming Trends](../../.gitbook/assets/reactive-programming-trends.png)



## Exemple d'approche impérative

1. Les ordres de virement bancaire sont stockées dans une base de données.
2.  Un "batch" est lancé chaque soir pour déclencher les virements.
3. Un "batch" est lancé quelques heures plus tard pour notifier les utilisateurs en fonction des résultats stockés par le "batch" précédent.

## Exemple d'approche réactive

1. L'utilisateur s'inscrit aux notifications de virements.
2. Le système de notification est donc inscrit au flux de résultats de traitement de virements.
3. Le flux de résultats de traitement est produit par l'inscription au flux d'ordres de virements.
4. Le flux d'ordres de virements est alimenté par différentes sources.

## ReactiveX

L'implémentation la plus commune des `Observable`s est ReactiveX et elle est disponible dans différents langages : **RxJS**, **RxPython**, **RxJava**, **Rx.NET** etc...

{% embed data="{\"url\":\"http://reactivex.io/\",\"type\":\"link\",\"title\":\"ReactiveX\",\"icon\":{\"type\":\"icon\",\"url\":\"http://reactivex.io/favicon.ico\",\"aspectRatio\":0}}" %}

## 

