---
title: IFT3335 --- Travail pratique 2
lang: french
author:
 - Vincent Antaki
 - Guillaume Poirier-Morency
---

\abstract

Malheureusement, il nous est impossible de condenser tous les résultats dans
5 page avec cette largeur de texte.

Bonne lecture!

# Bayes naïf avec Reuters et OHSUMED

L'application du filtre `StringToWordVector` est assez lente, nous avons donc
sauvegardé les résultats intermédiaires avant de procéder à l'application des
algorithmes d'apprentissage.

Une fois segmentés, le dataset Reuters contient 32870 attributs et le dataset
OHSUMED en contient 50400.

Les données de Reuters ont été traitée avec une quantité raisonnable de mémoire
et de temps, tandis que pour pouvoir traiter les données d'OHSUMED, il a fallu
définir une limite maximale d'utilisation de mémoire de 8GB.

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

Table: Précision et rappel du Bayes naïf pour chaque classe du dataset Reuters.

Le modèle a pris 150.17 secondes à construire avec les données de Reuters.

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

Table: Précision et de rappel par classe du classifieur Bayes naïf entrainé sur
l'entièreté des données du dataset OHSUMED.

Le modèle a pris 282.32 secondes à construire avec les données d'OHSUMED.

# Sélection d'attributs

La sélection d'attributs a été appliqué sur l'ensemble d'entraînement, la
validation croisée étant trop coûteuse à calculer.

L'algorithme de scorage _Ranker_ a été utilisé pour effectuer la sélection des
attributs.

Nous avons effectués nos test avec l'idée d'estimer le nombre d'attributs
à sélectionner pour maximiser l'efficacité du classifieur en tentant de trouver
un maximum local ou un environ.

## Gain d'information

\# attributs Précision Rappel
------------ --------- ------
1000         0.81      0.8
500          0.807     0.796
750          0.807     0.801
825          0.807     0.801
910          0.811     0.802

Table: Précision et rappel moyens du Bayes naïf selon le nombre d'attributs
sélectionné par gain d'information pour le dataset Reuters.

Le nombre d'attributs qui maximise la précision et le rappel du Bayes
naïf est d'environ 910. On remarquera que l'efficacité est sensiblement la même
que lorsque l'on traite l'entièreté des données. Par contre, le temps de calcul
est beaucoup plus raisonnable.

\# attributs Précision Rappel
------------ --------- ------
1000         0.669     0.454
500          0.677     0.469
250          0.688     0.505
125          0.701     0.564
6            0.728     0.747
5            0.743     0.763
4            0.619     0.73

Table: Précision et rappel moyens du Bayes naïf selon le nombre d'attributs
sélectionné par gain d'information sur le dataset OHSUMED.

Notre optimal empirique d'attributs sélectionné par gain d'information sur le
dataset OHSUMED est 5.

On remarque qu'une sélection très restreinte d'attribut avec le gain
d'information augmente la performance du classfieur de Bayes naïf lorsque
entrainé sur OHSUMED. On peut supposer que cela est dû au fait qu'OHSUMED est
une collection d'articles spécialisés.

## Méthode des chi carrés

\# attributs Précision Rappel
------------ --------- ------
1000         0.811     0.8
500          0.812     0.797
750          0.811     0.8
875          0.813     0.803

Table: Précision et rappel moyen du Bayes naïf selon le nombre d'attributs
sélectionnés par la méthode des chi carrés pour le dataset Reuters.

Par la méthode des chi carrés sur un classifieur de Bayes naïf, nous avons
obtenu une meilleure performance pour les données de Reuter avec une sélection
de 875 attributs. On remarquera que la performance est légèrement supérieur au
deux dernières méthodes présentées.

\# attributs Précision Rappel
------------ --------- ------
1000         0.673     0.46
500          0.68      0.536
250          0.698     0.652
125          0.719     0.703
62           0.745     0.74
31           0.783     0.785

Table: Précision et rappel moyen du Bayes naïf selon le nombre d'attributs
sélectionné par la méthode des chi carrés sur le dataset OHSUMED.

On constate qu'une sélection d'environ 30 attributs offre une classification
optimale pour le Bayes naïf à l'aide de la méthode des chi carrés pour le
dataset OHSUMED. De plus, la précision et le rappel est légèrement meilleur
qu'avec la méthode par gain d'information.

Il semble y avoir un petit ensemble de termes qui sont fortement discriminants
pour le _dataset_ OHSUMED, contrairement à Reuters où une bonne classification
doit considérer une quantité considérable de termes.

La méthode par les chi carrés offre de meilleures métriques pour les deux
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

On constate un gain considérable pour la classe `coffee` dont la précision
passe de 0.282 à 0.415. De manière générale, les résultats sont égaux ou
légèrement inférieurs.

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

Table: Précision et rappel moyens du Bayes naïf pour le dataset Reuters traité
par racinisation et sélectionné par la méthode des chi carrés.

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

La sélection de 10 attributs est un maximum local pour le gain d'information.

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

La sélection de 20 attributs constitue un maximum local pour la méthode des
chi carrés pour le dataset OHSUMED traité par racinisation.

Encore une fois, sur les données racinisées, la méthode des chi carrés offre de
meilleures métriques que le gain d'information.

# Arbre de décision, SVM et Perceptron

Tous les algorithmes demandés on été exécuté à l'aide de racinisation, des
quantités optimales d'attributs ainsi que tout l'ensemble de données lorsque
c'était réaliste.

## Arbre de décision

\# attributs Précision Rappel
------------ --------- ------
100          0.852     0.852
50           0.826     0.827

Table: Précision et rappel de l'arbre de décision J48 sur le dataset Reuters
traité par racinisation en fonction du nombre d'attributs sélectionnés par la
méthode des chi carrés.


\# attributs Précision Rappel
------------ --------- ------
1000         0.777     0.781
100          0.794     0.795
50           0.8       0.801
25           0.796     0.795
20           0.796     0.796

Table: Précision et rappel de l'arbre de décision J48 sur le  dataset OHSUMED
traité par racinisation en fonction du nombre d'attributs sélectionnés avec la
méthode des chi carrés.

\# attributs Précision Rappel
------------ --------- ------
1000         0.879     0.88
500          0.883     0.885
100          0.852     0.852
50           0.826     0.827

Table: Précision et rappel de l'arbre de décision J48 sur le dataset Reuters
traité par racinisation en fonction du nombre d'attributs sélectionnés par la
méthode des chi carrés.

La classification par arbre de décision est largement meilleur que le Bayes
naïf. Dans la plupart des cas, on observe des gains d'environ 8% autant pour la
précision que le rappel.

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

Table: Précision et rappel de la classification par SMO pour l'ensemble de
données Reuters traité avec racinisation.

Effectuer la racinisation a un impact négligable sur la performance du SMO et
le coût est considérablement plus élevé que de rouler l'algorithme sur le
_dataset_ complet.

Classe      Précision Rappel
------      --------- ------
Hyperplasia 0.981     0.983
Mitosos     0.986     0.989
Necrosis    0.969     0.971
Pediatrics  0.984     0.986
Pregnancy   0.946     0.949
Rats        0.957     0.958

Table: Précision et rappel moyens de la classification par SMO pour le dataset
OHSUMED.

La classification par SMO est excellente pour les données d'OHSUMED.

Le SVM classifie particulièrement bien les textes dans un contexte de
séparation binaire. Le fait qu'il roule sur un grand ensemble d'attributs est
à son avantage, car il peut facilement compenser les pénalités des dimensions
qu'il ne peut pas séparer distinctement.

## Perceptron

Le perceptron est très coûteux à entrainer, alors nous n'avons pas pu
l'appliquer sur les ensembles de données complets.

Voici les résultats pour 5, 10 et 30 couches de neurones cachées:

Couches Précision Rappel
------- --------- ------
5       0.356     0.469
10      0.6       0.606
30      0.823     0.829

Table: Précision et rappel moyens de la classification par un perceptron
multi-couche pour le dataset Reuters traité avec racinisation et dont 1000
attributs ont été sélectionnés avec la méthode des chi carrés.

Tous les perceptrons se sont concentrés à maximiser les classes les plus
populées (Aucune, earn et acq dans le cas de Reuters) et n'offrent aucune
précision ni rappel pour les autres.

Couches Précision Rappel
------- --------- ------
3       0.797     0.795
4       0.8       0.801
5       0.802     0.802
10      0.801     0.801
20      0.8       0.8
30      0.801     0.795

Table: Précision et rappel moyens du perceptron pour différents nombre de
couches du dataset OHSUMED traité avec racinisation et dont 20 attributs ont
été sélectionnés par la méthode des chi carrés.

On remarque le perceptron plafonne après 5 couches, même que les résultats se
dégradent légèrement.

De manière générale, il semble que le perceptron reste coincé dans un maximum
local, car il ne considère qu'une petite partie de l'information fournie.

# Exploration libre

Pour l'exploration libre, nous avons tenté d'améliorer la performance de la
classification du dataset Reuters.

Tout d'abord, nous avons utilisé une _stoplist_ afin d'exclure les mots communs
en anglais comme les déterminants ou les chiffres.

La transformation TF/IDF ajoute deux facteurs au poids d'un mot:

 - la fréquence du terme considéré dans le document;
 - la fréquence du terme considéré dans le corpus.

Un mot fréquent dans un document aura un poids plus considérable, tandis qu'un
mot fréquent dans l'ensemble du corpus sera pénalisé.

Classe Précision Rappel
------ --------- ------
Aucune  0.888     0.826
earn    0.858     0.866
housing 0.75     0.857
acq     0.763     0.862
coffee  0.51     0.714
gold    0.684     0.765
heat    1         0.75
Moyenne 0.849     0.844

Table: Précision et rappel du Bayes naïf sur l'ensemble de données de Reuters
traité avec une _stoplist_, TF/IDF, racinisation et dont 1000 attributs ont été
sélectionnés par la méthode des chi carrés.

L'application des transformation a permis d'aller chercher environ 5% de plus
en précision et 4.5% de plus en rappel par rapport aux meilleurs résultats
obtenu avec le classifieur Bayésien naïf.

Puisque les arbres de décisions J48 ont été les meilleurs à classifier les
documents, nous avons essayé quelques variantes afin de voir si elles
permetteraient d'obtenir de meilleures métriques.

Classifieur              Précision Rappel
-----------              --------- ------
J48                      0.881     0.882
REPTree                  0.856     0.858
RandomForest (10 arbres) 0.893     0.889
RandomForest (15 arbres) 0.902     0.899
RandomForest (20 arbres) 0.909     0.906
RandomTree               0.797     0.8
DecisionStump            0.584     0.692
RBFNetwork               0.87      0.864

Table: Précision et rappel moyens de différents classifiers de la catégorie
arbre de décision.

Étrangement, des valeurs nulles sont observées pour la classe _heat_ dans le
classifieur REPTree. Les résultats sont toutefois légèrement supérieurs au
Bayes naïf.

L'arbre J48 a encore une fois démontré sont efficacité pour classer les données
de Reuters et frôle les 90% de précésion et de rappel. Le mieux observé jusqu'à
présent était de 82.6% avec une sélection d'attributs optimisée.

Le classifieur RandomForest est particulièrement rapide à entrainer et fournit
les meilleurs résultats de précision et de rappel.

