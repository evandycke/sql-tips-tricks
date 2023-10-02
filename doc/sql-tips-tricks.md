# Quelques trucs et astuces pour optimiser vos requêtes SQL

## Généralités

### Créez un index sur de très grandes tables

Lorsqu'une requête est exécutée sur une grande table sans index, la base de données doit analyser la table entière pour trouver les données pertinentes. Cela peut prendre du temps, surtout si la table contient des millions de lignes. Cependant, si un index est créé sur les colonnes concernées, la base de données peut utiliser l'index pour localiser rapidement les données, ce qui peut réduire considérablement le temps de réponse des requêtes.

Au delà d'un million de lignes dans la table, prévoyez toujours un index.
Et dans le doute, prévoyez toujours un index quelque soit le volume de la table !

### Utilisez les champs SELECT au lieu de SELECT *

Si une table comporte de nombreuses colonnes, leur sélection peut forcer la base de données à effectuer des jointures inutiles, ce qui peut s'avérer coûteux et ralentir l'exécution des requêtes. En sélectionnant uniquement les champs obligatoires, vous pouvez éviter ces jointures inutiles.

On prend pour habitude d'avoir une consommation raisonnée des données et on ne consomme que ce dont on a besoin !

### Exécutez votre requête pendant les heures creuses

Pendant les heures de pointe, le serveur peut déjà être soumis à une forte charge en raison de l'activité élevée des utilisateurs (reporting opérationnel, ...), et l'exécution de requêtes supplémentaires peut encore augmenter la charge. En exécutant des requêtes pendant les heures creuses, la charge du serveur peut être réduite, ce qui peut améliorer les performances globales de la base de données et l'empêcher de ne plus répondre ou de planter.

## Jointures et sous-requêtes

### Utilisez INNER JOIN au lieu de WHERE pour joindre des tables

INNER JOIN permet au moteur de base de données d'optimiser le plan d'exécution des requêtes en choisissant la méthode de jointure la plus efficace en fonction de la taille de la table, des index et des statistiques. D'un autre côté, l'utilisation de WHERE pour joindre des tables peut forcer la base de données à utiliser une méthode de jointure moins efficace, ce qui peut ralentir l'exécution des requêtes.

### Minimisez l'utilisation de sous-requêtes

Les sous-requêtes peuvent nécessiter beaucoup de ressources et de temps, en particulier lorsque vous travaillez avec de grands ensembles de données. En minimisant leur utilisation, vous pouvez améliorer les performances des requêtes et réduire le temps d'exécution de vos instructions SQL.

### Utilisez des tables dérivées et temporaires

Des tables temporaires peuvent être utilisées pour stocker des résultats intermédiaires et réduire la quantité de traitement nécessaire à l'exécution d'une requête. Cela peut améliorer les performances des requêtes, en particulier pour les requêtes complexes impliquant plusieurs jointures ou sous-requêtes.

### Choisissez la jointure GAUCHE/DROITE lorsque cela est possible plutôt que la jointure INNER pour la même sortie

La jointure INNER inclut uniquement les enregistrements qui ont une correspondance dans les deux tables. Cela signifie que tous les enregistrements qui n'ont pas de correspondance dans l'autre table seront exclus du jeu de résultats. En utilisant la jointure GAUCHE/DROITE, vous pouvez éviter de perdre des données qui ne correspondent pas dans l'autre table.

## Agrégats

### Utilisez EXISTS() au lieu de COUNT() pour rechercher un élément dans le tableau

EXISTS() est généralement plus rapide que COUNT() car il n'a besoin de trouver qu'une seule ligne correspondante dans le tableau, tandis que COUNT() doit analyser la table entière pour compter toutes les lignes correspondantes. Cela peut être particulièrement important pour les grandes tables, où la différence de performances entre les deux approches peut être significative.

### Évitez SELECT DISTINCT si possible

SELECT DISTINCT peut être lent et gourmand en ressources, en particulier pour les grands ensembles de données. En effet, cela nécessite de trier et de comparer toutes les lignes du jeu de résultats pour identifier et supprimer les doublons. Dans certains cas, il peut être plus rapide d'utiliser d'autres techniques pour supprimer les doublons, telles que le regroupement ou la jointure sur une clé unique.

### Utilisez GROUP BY sur les fonctions de fenêtre

### Utilisez la clause WHERE au lieu de HAVING

## Combinaison de résultats

### Utilisez UNION ALL au lieu de UNION si possible

UNION ALL est généralement plus rapide que UNION car il ne supprime pas les doublons et n'effectue aucun traitement supplémentaire pour garantir l'unicité. En évitant ce traitement supplémentaire, UNION ALL peut être plus rapide et plus efficace, en particulier lorsque vous travaillez avec de grands ensembles de données.

### Utilisez UNION au lieu de OR si possible

UNION peut être plus rapide et plus efficace que l'utilisation de OR car il permet à la base de données d'utiliser des index et d'optimiser le plan d'exécution des requêtes. OU peut être plus difficile à optimiser, en particulier lorsque vous travaillez avec de grands ensembles de données ou des requêtes complexes.

## Améliorer les performances

1. Utilisez LIMIT pour échantillonner les résultats de la requête
2. Supprimez l'index avant de charger des données en masse
3. Utilisez des vues matérialisées au lieu des vues
4. Partitionnez les grandes tables pour améliorer les performances des requêtes
5. Utilisez le regroupement de connexions à la base de données

## Eviter de faire cela

1. Sous-requêtes dans la clause WHERE
2. OR dans les requêtes de jointure
3. Opérateurs != ou <> (différents)
4. Fonctions scalaires dans les clauses WHERE et JOIN
5. Caractères génériques au début de l'opérateur LIKE

## Bonnes pratiques

1. Utilisez EXISTS au lieu de IN ou NOT IN
2. Utilisez des types de données et un classement appropriés pour les colonnes
3. Encapsulez des requêtes complexes dans des procédures stockées
4. Utilisez des variables de liaison ou des requêtes paramétrées
5. Utilisez des sources temporaires pour les ensembles de données fréquemment récupérés
