---
layout: post
title:  "Rendu en images de synthèse : Introduction"
date:   2021-01-19  22:00:08 +0200
categories: Informatique Rendu Traitement Image Cpp
tags: Informatique Rendu Traitement Image Cpp
---

# Introduction du rendu en image de synthèse.*
------------------------------
## Définition 

Il s'agit de créer informatiquement une image devant représenter un environnement virtuel de manière photoréaliste.
C'est un processus de vectoriation, on part de primitive graphique constituant la photo pour obtenir une image à partir d'outil (algorithmique et technique).
---------------------------------
## Image numérique

C'est un ensemble de pixel constitué d'un ensemble de canaux de couleurs (en général 3 caneaux bleue, rouge, vert).
Une image a une résolution (nombre de pixel de la hauteur/la longueur).
L'image numérique est l'objectif final, la résolution impacte sur la rapidité et l'efficacité des algorithmes
--------------------------------
## La caméra : Le point de vue du rendu 
La caméra permet de capturer les primitives permettant de constituer une image, elle est positionné sur notre scène virtuel.
-------------------------------------
## Deux problématiques majeures

- Qu'est ce qui est potentiellement visible depuis mon point de vue ?

- Qu'est ce que je vois depuis mon point de vue ?
------------------------------
## La scène (l'environnement virtuel)  

Pour reconstituer les objets, on utilise des **primitives graphiques**, tout l'enjeu du rendu est de savoir comment seront positionnées ces primitives dans la scène et quels modélisations géométriques auront-elles. 
De plus, savoir comment des éléments tels que la lumières et  les ombres devront être gérés posent des problèmes de modélisation physique qui ne sont pas à négliger. Associer au moteur de rendu on a souvent un moteur de modélisation physique pour résoudre cela.
-----------------------------------------
## L'apparence 

L'apparence de la scène est ce qui va permettre d'approcher le **photoréalisme**.
L'apparence est donné par les lumières qui doivent-être positionner dans la scène. En effet il existe plusieurs types de lumières (isotrope,, anisotrope) et elles peuvent prendre différentes forme (surfacique, volumique). L'intéraction entre cette dernière et la matière a aussi une grande inflluence sur le rendu, on parle de **modélisation colorimétrique**. 
---------------------------------------
## Les cas d'études 

### Illumination locale:
Pour déterminer la couleur d'un objet, on a finalement besoin que de ses caractéristiques locales (propriété de couleur ,orientation de la l'objet) et d'une suite d'élément finis (les lumières de la scènes). 

C'est un modèle simple et rapide mais ne prenant pas en compte toutes les subtilités de l'illumination possible amenant au photoréalisme.

### Illumination globale:
Pour aller plus loin on utilise l'illumination globale, en ajoutant des ombres, de la reflexion et de la réfraction de lumière.
----------------------------------------------
## Stratégie du rendu

Il y a deux stratégie très différente pour obtenir un rendu, la rastérisation qui agit polygone par polygone et le lancer de rayon qui agit pixel par pixel.

### Rastérisation 
On agit sur chaque élément constitutif de la scène.
On part de la scène et on envoie tout sur la caméra en mesurant l'impact de chaque primitive physique sur l'image finale. 

### Lancer de rayon 
On part de l'image et on regarde au niveau des rayons projeté ce qui se passe au niveau de la scène,
on va s'interesser à ce qui se passe sur la scène à partir d'un pixel de l'image.
-----------------------------

## Pour conclure

Le **rendu** est la création informatique d'une image à partir de divers procédé afin d'obtenir un résultat le plus **photoréaliste** possible.
Pour arriver à obtenir une image, il y a plusieurs problématique à résoudre en particulier la gestion du **point de vue**, le positionnement des **primitives graphiques** et les **stratégies de rendu** qui seront utilisées.