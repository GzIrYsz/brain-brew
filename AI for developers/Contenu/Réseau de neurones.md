---
tags:
  - note
  - computer-science
year: 2023
---

## Origine biologique

La réflexion se fait grâce au cerveau. Le cortex cérébral est composé majoritairement de **neurones**. Ils sont très nombreux (presque 100 milliards chez l'Homme). Cellules sont très fragiles et sont protégées par les cellules gliales qui n'ont cependant aucun rôle dans la réflexion.
On sait que les neurones communiquent entre eux par impulsions électriques. Les capteurs (œil, oreille, peau…) envoient des impulsions électriques aux neurones via les nerfs, qu'ils traitent et transmettent ou non aux autres cellules.
Chaque neurone possède autour de son cœur (appelé **soma**) :
- des **dendrites**, qui sont ses entrées
- un long **axone**, qui lui sert de sortie

Les signaux électriques arrivent donc au soma en suivant les dendrites puis sont traités (selon l'intensité du signal, la somme des impulsions) et le neurone envoie ou non une impulsion le long de son axone pour transmettre un signal aux dendrites d'autres neurones.
Chaque neurone est donc une entité très simple, faisant un travail sur les impulsions reçues pour choisir ou non d'en envoyer une en sortie. La puissance du cerveau se situe dans le nombre de neurones et les interconnexions entre eux.
Les réseaux de neurones artificiels s'inspirent de ce simple neurone biologique.

## Machine Learning

Le Machine Learning comprend l'ensemble des techniques permettant de faire apprendre quelque chose à un algorithme via des exemples sans programmation directe de sa résolution.
La majorité des techniques de ML sont des algorithmes purement mathématiques, on trouve cependant d'autres techniques liées à l'intelligence artificielle comme par exemple les réseaux de neurones (Deep Learning).

### Formes d'apprentissage et exemples

Pour résoudre des problèmes en ML, on distingue trois types d'apprentissage :
- l'apprentissage non supervisé
- l'apprentissage supervisé
- l'apprentissage par renforcement #go-deeper 

Dans le cadre de l'apprentissage non supervisé, il n'y a pas de résultat attendu. On cherche ici à faire du **clustering** : diviser un ensemble de données en éléments similaires. L'apprentissage supervisé quand à lui est divisé en deux tâches principales :
- la régression
- la classification

La régression consiste à prédire une valeur à partir d'un grand nombre d'autres données similaires. La classification consiste à classifier un élément selon des **motifs**.

## Neurone formel et perceptron

### Principe

Un neurone formel (neurone artificiel) reçoit des entrées et fournit des sorties à partir de ces dernières ainsi que de caractéristiques :
- des poids (qui pondèrent chacune des entrées)
- une fonction d'agrégation qui calcule une valeur unique à partir des entrées et des poids correspondants à chacune de ces entrées
- un seuil
- une fonction d'activation qui associe à chaque valeur agrégée une sortie qui dépend du seuil

Dans le cadre d'un neurone, la principale difficulté consiste en l'apprentissage des poids (le seuil peut être vu comme un poids "spécial"). Les fonction d'agrégation et d'activation sont principalement choisies pour réaliser cet apprentissage.
Dans le cas d'une régression, il n'est pas possible de dépasser la linéarité. Cela est possible avec les neurones grâce à la fonction d'activation (si elle n'est pas linéaire), au nombre de neurones, ainsi qu'aux différentes connexions entre ces derniers.

### Réseaux de type "perceptron"

Les perceptrons sont les réseaux de neurones le plus simple possible. Ils consistent juste en un ensemble d'entrées ainsi qu'un ensemble de neurones donnant une sortie. Dans le cadre d'une régression, il n'y a qu'un seul neurone donnant un réel en sortie et dans le cas d'une classification, il y a une sortie par classe et c'est la sortie la plus forte (avec la plus grande valeur) qui définit la classe choisie par le réseau.
Dans un réseau de neurones, chaque neurone effectue l'agrégation et l'activation avec des poids et un seuil différent pour chaque neurone.

### Fonctions d'agrégation et d'activation

#### Fonction d'agrégation

Le but de cette fonction est de transformer l'ensemble des entrées et des poids en une valeur qui sera utilisée par la fonction d'activation. Il existe différentes fonctions d'activation et les plus courantes sont :
- la somme pondérée
- le calcul de distance

Pour la somme pondérée, il s'agit juste de la somme de toutes les valeurs multipliées par leur poids, soit :
$$\sum_{i = 1}^n E_i * W_i$$
Cette opération est déjà celle réalisée lors d'une régression linéaire. La deuxième fonction est celle du calcul de distance. Cette fonction compare les entrées aux poids et calcule la distance entre eux. Elle se calcule comme suit :
$$\sqrt{\sum_{i = 1}^n (E_i - W_i)^2}$$
Il est possible d'imaginer d'autres fonctions d'agrégation. Le but est d'associer une seule valeur à l'ensemble des entrées et des poids grâce à une fonction linéaire.

#### Fonction d'activation

Le but de cette fonction est de comparer la valeur fournie par la fonction d'agrégation et de la comparée avec le seuil. Les principales fonctions d'activation sont :
- la fonction signe
- la fonction sigmoïde
- la fonction gaussienne
- la fonction ReLU
- la fonction Softmax

La fonction signe test