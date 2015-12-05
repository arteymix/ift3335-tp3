---
title: IFT3335 --- Travail pratique 2
author:
 - Vincent Antaki
 - Guillaume Poirier-Morency
geometry: 0.5in
---

# Reuters et OHSUMED

L'application du filtre `StringToWordVector` est assez lente, nous avons donc
sauvegardé les résultats intermédiaires avant de procéder à l'application des
algorithmes d'apprentissage.

Les algorithmes d'apprentissages sont extrêmement lent à appliquer sur les
datasets fournis.

Une fois segmentés, le dataset Reuters contient 32870 attributs et le dataset
OHSUMED en contient 50400.

Les données de Reuters ont été traitée avec une quantité raisonnable de mémoire
et de temps.

Pour pouvoir traiter les données d'OHSUMED, il a fallu définir une limite
maximale d'utilisation de mémoire de 8GB.

##Naif Bayes

Le résultats suivants sont

Classe  Précision Rappel
------  --------- ------
Aucune  0.865     0.771
earn    0.791     0.845
housing 1         0.571
acq     0.74      0.816
coffee  0.282     0.629
gold    0.639     0.676
heat    1         0.75
Moyenne 0.811     0.803

Table: Valeurs de précision et de rappel du Bayes Naïf pour chaque classe du
dataset Reuters.

Classe      Précision Rappel
------      --------- ------
Aucune      0.751     0.224
Hyperplasia 0.179     0.559
Mitosis     0.228     0.458
Necrosis    0.442     0.51
Pediatrics  0.087     0.845
Pregnancy   0.694     0.461
Rats        0.455     0.777
Moyenne     0.623     0.401

Table: Valeurs de précision et de rappel par classe du classifieur Bayes Naïf entrainé sur l'entièreté des données du dataset OHSUMED.

# Sélection d'attributs

La sélection d'attributs a été appliqué sur l'ensemble d'entraînement, la
validation croisée étant trop coûteuse à calculer.

L'algorithme de scorage _Ranker_ a été utilisé pour effectuer la sélection des
attributs.

Nous avons effectués nos test avec l'idée d'estimer le nombre d'attributs à sélectionner pour maximiser l'efficacité du classifieur en tentant de trouver un maximum local ou un environ.

## Gain d'information

\# attributs Précision Rappel
------------ --------- ------
1000         0.81      0.8
500          0.807     0.796
750          0.807     0.801
825          0.807     0.801
910          0.811     0.802

Table: Précision et rappel moyens du Bayes Naïf selon le nombre d'attributs
sélectionné par gain d'information pour le dataset Reuters.

Le nombre d'attributs qui maximise la précision et le rappel du Bayes
Naïf est d'environ 910. On remarquera que l'efficacité est sensiblement la même que lorsque l'on traite l'entièreté des données. Par contre, le temps de calcul est beaucoup plus raisonnable.

\# attributs Précision Rappel
------------ --------- ------
1000         0.669     0.454
500          0.677     0.469
250          0.688     0.505
125          0.701     0.564
6            0.728     0.747
5            0.743     0.763
4            0.619     0.73

Table: Précision et rappel moyens du Bayes Naïf selon le nombre d'attributs
sélectionné par gain d'information sur le dataset OHSUMED.

Notre optimal empirique d'attributs sélectionné par gain d'information sur le dataset
OHSUMED est 5 (ou 6).

On remarque qu'une sélection très restreinte d'attribut avec le gain d'information augmente la performance du classfieur de Bayes naïf lorsque entrainé sur oshumed. On peut supposer que cela est dû au fait qu'oshumed est une collection d'articles spécialisés.

## Chi-carrés

\# attributs Précision Rappel
------------ --------- ------
1000         0.811     0.8
500          0.812     0.797
750          0.811     0.8
875          0.813     0.803

Table: Précision et rappel moyen du Bayes Naïf selon le nombre d'attributs
sélectionnés par la méthode des chi-carrés pour le dataset Reuters.

Par la méthode des Chi-carrés sur un classifieur de Bayes naïf, nous avons obtenu une meilleure performance pour le dataset reuter avec une sélection de 875 attributs. On remarquera que la performance est légèrement supérieur au deux dernières méthodes présentées.

\# attributs Précision Rappel
------------ --------- ------
1000         0.673     0.46
500          0.68      0.536
250          0.698     0.652
125          0.719     0.703
62           0.745     0.74
31           0.783     0.785
15           à rajouter

Table: Précision et rappel moyen du Bayes Naïf selon le nombre d'attributs
sélectionné par la méthode des chi-carrés sur le dataset OHSUMED.

On constate qu'une sélection d'environ 30 attributs offre une classification
optimale pour le Bayes Naïf à l'aide de la méthode des chi-carrés pour le
dataset OHSUMED. De plus, la précision et le rappel est légèrement meilleur
qu'avec la méthode par gain d'information.

Il semble y avoir un petit ensemble de termes qui sont fortement discriminants
pour le _dataset_ OHSUMED, contrairement à Reuters où une bonne classification
doit considérer une quantité considérable de termes.

La méthode par les chi-carrés offre de meilleures métriques pour les deux
ensembles de données.

# Racinisation

Une fois racinisé (*stemmed*), les datasets Reuters et OHSUMED contiennent respectivement
27135 et 42505 attributs.

Classe  Précision Rappel
------  --------- ------
Aucune  0.854     0.786
earn    0.791     0.843
housing 1         0.429
acq     0.746     0.8
coffee  0.415     0.629
gold    0.676     0.676
heat    1         0.75
Moyenne 0.809     0.804

Table: Valeurs de précision et de rappel pour chaque classe et en moyenne du
dataset Reuters traité avec la racinisation.

On constate un gain considérable pour la classe `coffee` dont la précision passe
de 0.282 à 0.415. De manière générale, les résultats sont égaux ou légèrement
inférieurs.

Un plus grand pourcentage des documents on été correctement classifés avec le
_stemming_, soit 80.3268% au lieu de 80.0686%.

Classe      Précision Rappel
------      --------- ------
Aucune      0.752     0.232
Hyperplasia 0.194     0.568
Mitosis     0.231     0.47
Necrosis    0.484     0.507
Pediatrics  0.081     0.819
Pregnancy   0.691     0.478
Rats        0.465     0.791
Moyenne     0.629     0.41

Table: Valeurs de précision et de rappel pour chaque classe et en moyenne du
dataset OHSUMED traité avec la racinisation.

La racinisation permet surtout de réduire le temps d'entrainement des
différents modèles en réduisant le nombre d'attributs à considérer. L'effet
observé sur les métriques est négligable.

## Sélection d'attributs

\# attributs Précision Rappel
------------ --------- ------
925          0.806     0.799

Table: Précision et rappel moyens du Bayes Naïf pour le dataset Reuters traité
par racinisation et sélectionné par la méthode des chi-carrés.

\# attributs Précision Rappel
------------ --------- ------
1000         0.669     0.454
30           0.731     0.729
20           0.749     0.757
11           0.755     0.77
10           0.764     0.771
8            0.763     0.77
5            0.747     0.748

Table: Précision et rappel pour le gain d'information pour le dataset OHSUMED stemmé

La sélection de 10 attributs est optimale pour le gain d'information.

\# attributs Précision Rappel
------------ --------- ------
1000         0.669     0.45
30           0.784     0.786
25           0.795     0.797
22           0.796     0.798
21           0.796     0.797
20           0.802     0.802
19           0.801     0.801
15           0.79      0.787

Table: Précision et rappel pour différents nombres d'attributs sélectionnés par
la méthode des chi-squared dataset OHSUMED stemmé

La sélection de 20 attributs est optimale pour la méthode des chi-carrés pour
le dataset OHSUMED traité par racinisation.

Encore une fois, sur les données racinisées, la méthode des chi-carrés offre de
meilleures métriques que le gain d'information. Les algorithmes pour le prochain

# Arbre de décision, SVM et Perceptron

Tous les algorithmes demandés on été exécuté à l'aide de racinisation, des
quantités optimales d'attributs ainsi que tout l'ensemble de données lorsque
c'était réaliste.

## Arbre de décision

\# attributs Précision Rappel
------------ --------- ------
1000         0.777     0.781
100          0.794     0.795
50           0.8       0.801
25           0.796     0.795
20           0.796     0.796

Table: Précision et rappel de l'arbre de décision J48 sur le  dataset OHSUMED
traité par racinisation en fonction du nombre d'attributs sélectionnés avec la
méthode des Chi-Carrés.

\# attributs Précision Rappel
------------ --------- ------
1000
500
100          0.852     0.852
50           0.826     0.827

Table: Précision et rappel de l'arbre de décision J48 sur le dataset Reuters
traité par racinisation en fonction du nombre d'attributs sélectionnés par la
méthode des Chi-Carrés.

## SVM

Le SVM (avec SMO) est particulièrement facile et rapide à entrainer et offre des
résultats très surprenant performant. Nous avons donc appliqué la technique sur les
_datasets_ complets.

Classe  Précision Rappel
------  --------- ------
acq     0.925     0.896
coffee  0.997     0.997
earn    0.982     0.954
gold    0.87      0.588
heat    1         0.5
housing 1         0.571

Table: Précision et rappel de la classification par un SVM (SMO) pour le
dataset Reuters.

Classe  Précision Rappel
------  --------- ------
acq     0.93      0.904
coffee  1         0.6
earn    0.979     0.952
gold    0.875     0.618
heat    1         0.5
housing 1         0.572

Table: Reuters stemmé et classifié par un SVM (SMO)

Effectuer la racinisation a un impact négligable sur la performance du SMO et
le coût est considérablement plus élevé que de rouler l'algorithme sur le
_dataset_ complet.

Classe Précision Rappel
------ --------- ------


Table: OHSUMED

## Perceptron

Le perceptron est très coûteux à entrainer, alors nous n'avons pas pu
l'appliquer sur les _datasets_ complets.

Voici les résultats pour 5, 10 et 30 couches de neurones cachées:

Couches Précision Rappel
------- --------- ------
5       0.356     0.469
10      0.6       0.606
30      0.823     0.829

Table: Précision et rappel moyens pour chaque classe du dataset OHSUMED
classifié à l'aide d'un perceptron multi-couche dont 1000 attributs ont été
sélectionnés avec la méthode des Chi-Carrés.

Tous les perceptrons se sont concentrés à maximiser les classes les plus
populées (Aucune, earn et acq dans le cas de Reuters) et n'offrent aucune
précision ni rappel pour les autres.

Couches Précision Rappel
------- --------- ------
3       0.797     0.795
4       0.8       0.801
5       0.802     0.802

Table: Précision et rappel moyens pour ...

On remarque le perceptron plafonne après 5 couches.

# Exploration libre

Pour l'exploration libre, nous avons tenté d'améliorer la performance de la
classification du dataset Reuters.

 - TF/IDF
 - sélection d'attributs optimale
 - stemming
 - tester les modèles différents (MultiClassSVM)
 -

Reuters TF/IDF avec racinisation

Le Bayes Naïf donne les résultats suivants:

Classe  Précision Rappel
------  --------- ------
acq     0.93      0.904
coffee  1         0.6
earn    0.979     0.952
gold    0.875     0.618
heat    1         0.5
housing 1         0.572

