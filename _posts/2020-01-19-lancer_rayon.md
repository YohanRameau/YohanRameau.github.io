---
layout: post
title:  "Rendu en images de synthèse : Lancer de rayon"
date:   2021-01-19  22:00:08 +0200
categories: Informatique Rendu Traitement Image Cpp Ray Tracing
tags: Informatique Rendu Traitement Image Cpp Ray Tracing
---
# Le lancer de rayon
--------------

## Définition 

Aussi appelé **Ray Tracing** c'est un algorithme visant l'illumination globale de la scène en suivant les échanges de transferts énergétiques de la lumière. 

On suit les photons pour voir où ils rebondissent et connaître la couleur des objets que les rayons rencontrent, il est fondé principalement sur l'intersection des rayons et de la géométrie. C'est un algorithme qui n'est généralement pas en temps réel. 

Le rayon est lancé depuis la caméra vers un pixel et lorsque l'on rencontre un objet sa couleur peut être ainsi déterminée, c'est donc un algorithme dont la philosophie est orientée image et dont la complexité va dépendre du nombre de pixel de l'image. Un très grand nombre de pixel nécessitera un plus grand nombre de lancer de rayon.

## L'algorithme

1) Lancer un rayon -> Pour chaque pixel de l'image
2) Calcul de la première intersection entre le rayon et les objets
3) Transformation du rayon dans les repères locaux
4) Illumination    -> Calcul de la couleur de l'objet par rapport au lumières
5) Reconstituion du rendu final -> on récupère et restitue la couleur obtenue précédemment

C'est un algorithme qui peut être amélioré car il dépend du nombre de pixel et des objets de la scène. On peut optimiser les boucles pour plus de rapidité ou augmenter la complexité pour plus de photoréalisme .

### Génération des rayons

La génération de rayon la plus simple est de tirer des rayons au milieu de chaques pixels mais cela pose des problèmes de précision et peut créer de l'aliasing.
Pour parer cela, on peut tirer aléatoirement au sein d'un pixel voire lancer plusieurs rayon au sein d'un même pixel et de choisir la moyenne des couleurs obtenues. On appelle cette technique le Pixel Sampling correspondant à de l'antialiasing.

### Intersection
Pour calculer l'intersection du rayon avec un objet de la scène afin de déterminer sa couleur, on utilise les primitives graphiques.
Dans la rasterisation, c'est principalement des polygones et en particulier des triangles qui sont utilisés. Avec le lancer de rayon tout élèment dont l'intersection avec un rayon est calculable peut être choisit ( Sphère, cylindre, plan, carré, triangle, tore, ellipsoïde).
Pour déterminer l'intersection on va résoudre une équation cartésienne de la primitive + équation paramètrique du rayon.

On peut optimiser l'algorithme à l'étape de l'intersection en se libérant de la dépendance de la scène.
On va chercher à utiliser des stuctures de données accélératrice (Arbres ou grille uniformme).
Ces structures de données possèdent des algos pour traiter les rayons. 

### Calcul de l'illumination 

C'est la dernière étape. 

On va calculer l'éclairement pour chaque source de lumière de la scène.
Pour le lancer de rayon il est facile d'ajouter l'ombrage : on tire un rayon d'un point de lumière de la scène vers un point d'intersection et on regarde s'il est intercepté par des objets de la scène. S'il ne l'intercepte pas il y a une ombre.

On peut aussi suivre des directions de reflexions spéculaire pures (miroir) et les directions de transmission (sphère mettalique non opaque).

## Conclusion

Le lancer de rayon est un algorithme dont la philosophie est orienté **image** et qui est lié à chaque pixel. Ainsi, la complexité de l'algorithme peut-être relativement élevé bien qu'il existe des structures de données permettant d'accélerer l'algorithme. 

Le lancer de rayon se déroule en **trois étapes** et permet de générer relativement facilement des ombres en déduisant de la trajectoire de certains rayons les objets qui ne sont pas touchés par ces derniers.
