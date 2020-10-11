*Rapport groupe 4 (Bocquet/Bonin/Burie)*

# Data

[Lien vers la page Kaggle contenant le dataset que nous avons utilisé.](https://www.kaggle.com/pierremegret/dialogue-lines-of-the-simpsons)

Le dataset que nous avons choisi contient les dialogues des 27 saisons de la série « Les Simpsons » (environ 600 épisodes). Cela correspond a plus de **130 000** lignes de dialogues. Notre dataset est composé de 2 colonnes :
<li>"<i>raw_character_text</i>" , le nom du personnage qui parle</li>
<li>"<i>spoken_words</i>" , le texte énoncé par le personnage</li>

Ci-dessous un échantillon du dataset brut.
![data_head.png](attachment:data_head.png)

Avec ces données, nous nous sommes donnés le challenge de prédire quel personnage a parlé selon une certaine ligne de dialogue.

## Preprocessing

### Sélection des personnages

Afin de mener à bien notre projet, nous avons choisi de nous focaliser sur les personnages ayant un minimum d’activité dans la série. Nous avons donc choisi de sélectionner les personnages ayant plus de **1000** lignes de dialogues, ce qui laisse **14** personnages différents dans le dataset.
Voici ci-dessous un diagramme en barre indiquant les personnages restant dans le dataset et le nombre de phrases qu'ils ont dit.
![charact_diagram.png](attachment:charact_diagram.png)

### Word processing

Comme nous devions faire 2 processing différents, en plus de tous les traitements (lemmatization, non-alphabetic characters removing, ...) nous avons de choisi d'en faire un qui supprime les stop words et un qui les ignore. Nous nous sommes dits que garder les stop words pouvait être intéressant comme il s'agit de lignes de dialogues.
Après le premier traitement (qui supprime les stop words) nous nous retrouvons **61 361** mots différents et après le deuxième (qui ignore les stop words), nous nous retrouvons avec un dataset comptant **71 649** mots différents.




# Méthodes de classification

## Naive Bayes

## Random Forest

Pour mettre au point le modèle random forest, nous avons choisi de garder les **5000** mots qui apparaissent le plus en fréquence, sans compter les mots qui apparaissaient dans plus de **70%** des phrases.
Dans le code, nous avons choisi de faire **10** arbres dans notre random forest afin d'éviter les problèmes de fuites mémoire et les temps d'exécution trop longs, mais il est possible de mettre 1000 arbres dans le modèle avec de la patience pour obtenir une meilleure précision.
De même, la cross-validation pour le modèle random forest n'est composée que de **2** segments par souci de performances.

# Résultats

## Naive Bayes

![NB_Resume.PNG](attachment:NB_Resume.PNG)

Le modèle Naive Bayes pour ce dataset est mauvais, la précision du modèle est de **36,5%** avec les stop words et de **37%** sans. Cette faible précision est dûe au fait qu'il est compliqué d'attribuer un mot à une personne et que la repartition des données est inégale, par exemple à Homer sont attribués 57 000 mots sur les 61 000, le deuxième en a seulement 1800. Cette répartition induit donc un mauvais modèle pour prédire qui dit quoi.
Homer possède une place beaucoup trop importante dans le dataset, en moyenne presque tous les mots du dataset ont été dits plus de fois par Homer que par les autres personnes ce qui fausse les prédictions.

## Random Forest

*Après le word processing retirant les stop words*
![RF_Resume.PNG](attachment:RF_Resume.PNG)

*Après le word processing ignorant les stop words*
![RF_Resume_1.PNG](attachment:RF_Resume_1.PNG)

Le modèle issu de la random forest présente une présicion simillaire à celui avec Naive Bayes (environ **37%** en supprimant les stop words et environ **35,6%** en les ignorant). Cependant ce modèle est plus général que celui prédit par Naive Bayes : il permet de déterminer avec plus de précision qui a dit une certaine phrase dans le cas général.

Vous trouverez tous les résultats et codes dans les 2 fichiers joints au git (Naive Bayes : *TD4_NB.ipynb*, Random Forest : *TD4_RF.ipynb*)
