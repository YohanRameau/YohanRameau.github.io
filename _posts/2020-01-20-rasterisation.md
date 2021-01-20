---
layout: post
title:  "Rendu en images de synthèse : La rasterisation"
date:   2021-01-19  22:00:08 +0200
categories: Informatique Rendu Traitement Image Cpp rasterisation
tags: Informatique Rendu Traitement Image Cpp Ray rasterisation
---
# La rasterisation
--------------

## Définition

Aussi appelé **algorithme du Z-Buffer** il est l'algorithme le plus utilisé actuellement par les logiciels (en particulier dans tout ce qui est temps réel), le jeu vidéo, réalité virtuelle etc...

L'algorithme se base sur la **projection de polygones convexes**, généralement des **triangles** sur le tas de pixel constituant l'image, ces formes sont ensuites remplit.

La rasterisation tire son nom de l'anglais: to rasterize. 

------------

## Principe

Cet algorithme s'appuie principalement sur les **primitives graphiques** constituant une image. Dans ce cas les primitives graphiques sont des **polygones convexes** (plus généralement des triangles). Tous les triangles sont projetés sur l'image de pixel et les occlusions seront gérées grâce au **Z-buffer**, une image de profondeur. Ces notions seront détaillés juste après.
    Occlusion : Il peut arriver qu'il y a quelque chose que l'on souhaite voir mais qu'on ne peut pas pour des raisons diverses, on appelle ce phénomène l'occlusion.

------------------
## L'algorithme

### Transformation dans le repère de la caméra

Chaque primitive graphique va être transformé dans le **repère propre de la caméra**, ce procédé mathématique permet de déterminer les coordonnées des points relativement à la caméra

### Illumination

Détermination de la couleur de chaqu'un des sommets d'un triangle.

### Projection des primitives

On projette la primitive graphique sur l'image finale, elle comprend **le culling** (savoir si le triangle est dans le champs de vue de la caméra).

### Rasterisaton

Détermination de tous les pixels qui sont recouvert par la primitive graphique.

### Gestion des occlusion

On vérifie s'il n'existe pas une primitive graphique plus proche (par rapport à la caméra) que la primitive que l'on vient de projeté. Pour cette étape on utilise le Z-buffer pour **comparer les différentes profondeurs**.

    L'illumination peut se faire pixel par pixel, dans ce cas cette étape se situe entre la rasterisation et la gestion de l'occlusion

### L'insertion dans l'image

Toutes les pixels recouvert par une primitives et non occulté par d'autres primitives graphiques vont venir s'insérer sur l'image finale.

------------

## Le fonctionnement du Z-buffer

Le z-buffer va conserver la plus proche profondeur pour une image de profondeur.
Pour chaque pixel, on connait la primitive graphique la plus proche par rapport à la caméra.
A l'insertion d'un nouveau pixel, on va comparer sa profondeur pour savoir s'il est plus proche que celle présente actuellement dans le Z-Buffer, on va pouvoir modifier l'image en conséquence.
S'il est plus proche, cela signifie que la primitive graphique est plus proche que la précédente, s'il est plus éloigné cela veut dire qu'elle est occulté et qu'il n'est donc pas nécessaire de la représenter sur l'image.
Il est donc nécessaire de transmettre la profondeur de chaque pixels à insérer. 

------------

## Pipeline Graphique

OpenGL ou Direct3D permet de réaliser rapidement une image de synthèse en s'appuyant sur un pipeline graphique permettant la rasterisation.

---------------

## Pour conclure

La rasterisation est un algorithme basé sur la projection de primitives graphiques. Ces primitives sont analysés par l'algorithme puis traité pour déterminer leurs profondeurs et s'il subissent de l'occlusion ou non. La complexité de cette algorithme est fortement lié au nombre de polygones constituant la scène.