---
layout: post
title:  "Programmation Réseau en Java: Les bases"
date:   2021-01-14  09:49:08 +0200
categories: programmation réseau java
tags: programmation réseau java udp
---
----------------

## Introduction

Nous verrons les différentes implémentations en Java permettant d'échanger des données via un réseau, avec différents protocoles (IP, UDP ou TCP) et différentes architectures (serveurs concurrents, entrées/sorties non bloquantes etc).
------------------------------------
## Transmission des données

Du point de vue OS, le bit est la valeur atomique. Pour se transmettre des données on se transmet donc des octets (séquences de 8 bits). 

Ces derniers peuvent correspondre à plusieurs types de données (chaînes de caractères, nombres etc...), peuvent être interprétés de plusieurs manières (BigEndian, LittleEndian) et peuvent donc avoir de multiples significations. C'est pour cette raison qu'il faudra indiquer comment interprêter la séquence d'octets transmise.

Les différents protocoles réseau, permettent d'indiquer comment traduire une séquence d'octet qui se balade à travers le réseau.

#### BigEndian
    L'octet de poids fort est en première position

#### LittleEndian  
    L'octet de poids faible est en première position

#### Charset 
    Jeux de caractères permettant de faire une correspondance avec une séquence de bit. (UTF8, ASCII, etc...)

    En fonction du charset utilisé, une séquence de bits n'aura pas du tout la même signification
        Par exemple: 61 correspond au caractère a en ASCII alors qu'il n'est associé à aucun caractère en UTF-8
-----------------------------------
## En Java

Historiquement Java manipule les séquences de bits grâce aux *byte array* ``byte[]``

Suite à des problèmes de performances, la librairie java.nio a été créée, elle permet d'utiliser des *ByteBuffer*, une classe manipulant les octets bien plus efficacement que les *byte array*.
-------------------------
## java.nio : new input/output

Cette librairie permet de gérer la mémoire en dehors du garbage collector, directement dans le systême ce qui permet un gain de performance.
-----------
### Le java.nio.ByteBuffer

Les ByteBuffer sont les remplaçant des ``byte array``, mais avec une utilisation différentes. Ils ont une taille **fixée** à l'avance et une zone de travail dans lequel s'effectuera différentes actions.

Ils sont plus rapide que les bytes array mais ils sont plus lent à l'allocation/desalocation car ils pointent vers une zone mémoire du système et ne sont pas directement gérés par la JVM et le Garbage Collector.
--------------------------
### la zone de travail 
    Correspond à deux indices :
    1. Position: le premier indice de la zone 
    2. Limit: le première indice en dehors de la zone.
Les différentes méthodes impliquant des ByteBuffer sont succeptible de modifier la position et/ou la limite. Il faut donc veiller à la cohérence de ces deux indices.  
-----------------------
### Création

```java 
ByteBuffer bb = ByteBuffer.allocate(1024); 
```
Le chiffre indiqué à l'allocation est la capacité du ByteBuffer. A ce moment, la *position* est 0 et la *limite* est la capacité.

L'objet est alloué sans passer par la JVM, les entrées/sorties seront donc beaucoup plus efficace mais l'allocation/desallocation sera plus lente.

Il existe une méthode `allocateDirect()` pour les ByteBuffer avec une grande durée de vie dans le programme.
--------------------------
### Accès

    - ``ByteBuffer put(Byte b)`` On écrit un octet à la *position* courante 
    - ``Byte get()`` lit et retourne l'octet a la position courante
  Un appel à l'une de ces deux méthodes réduira la zone de travail car *position* s'incrémentera.
  Si la zone de travail est vide `BufferOverFlowException` sera levé. 
-------------------
### Ecriture et Lecture

Un buffer est soit en lecture, soit en écriture.
Pour passer d'un mode à l'autre, ByteBuffer possède plusieurs méthodes.

#### Flip
`bb.flip()` permet de passer en mode lecture.
L'indice limite prendra la place de l'indice position et l'indice position sera égale à zéro.

#### Compact
`bb.compact()` permet de repasser en mode ecriture. La zone de mémoire est replacé intelligemment afin de ne pas écraser de la mémoire, en effet les données comprise entre la position et la limite vont être déplacé entre la position 0 et la limite qui sera placé à capacity. 
Toutes les données qui étaient initialement présente ici seront écrasés. 

#### Autre méthode 

- `remaining()` taille de la zone de travail
- `hasremaining()` permet de savoir si la zone de travail est non vide  
- `position()` position actuelle
- `position(int pos)` set pos
- `limit()` actuel limit
- `limit(int pos)` set limit
- `clear()` remet la position a 0 et limit a la taille de depart

#### Méthode pour les types primitif

`putInt()` écrit 4 octets d'un entier au début de la zone de travail avant de la réduire.
`getInt()` Lit 4 octets d'un entier au début de la zone de travail avant de la réduire.

Ces méthodes existent pour les autres types primitifs.
------------------------------
### java.nio.ByteOrder
Permet d'avoir un comportement standard pour les différents types de système. Par défaut en BigEndian qui est l'ordre le plus utilisé dans les protocoles réseau. Ils peut être modifié par `order(ByteOrder)`.
`ByteOrder.nativeOrder()`donne l'ordre utilisé par le système.
-----------------------------
### Encodage and decodage

Les charset comme vu ci-dessus indique un mapping d'un caractère vers un d'octet. La librairie que nous verrons ci dessous permet de les gérer.

    Encodage: Caractère -> Octets
    Décodage: Octets -> Caractères

#### java.nio.charset.Charset

Fournit des méthodes pour encoder et décoder selon un charset donné.

```Java

Charset charset = Charset.forName("UTF-8");
Charset charset = StandardCharsets.UTF_8;
ByteBuffer bb = charset.encode(String s);
CharBuffer cb = charset.decode(ByteBuffer bb);
```
L'encodage retourne un ByteBuffer contenant l'encodage selon le charset donné.

Pour décoder, il est indispensable d'avoir un ByteBuffer contenant tous les octets de l'encodage. Sinon le message ne sera pas correctement traduit.

### FileChannel

Les FileChannel permet de lire et d'écrire des octets depuis un fichier.

#### Lecture 

```Java
int fc.read(ByteBuffer bb) 
```
lit au plus la taille de la zone de travail du ByteBuffer.

`read()` retourne le nombre d'octets lu, -1 si le channel est fermé.
Il n'y a aucune garantie permettant de s'assurer que le ByteBuffer a été remplit. Cette méthode est bloquante jusqu'au dernier octet lu. 

#### Ecriture

```Java
int fc.write(ByteBuffer bb) 
```
Ecrit la totalité de la zone de travail du ByteBuffer dans le FileChannel. Comme read, cette méthode est également bloquante et retourne le nombre d'octet écrit.
------------------------
## Pour conclure

Java fournit pas mal de méthode pour gérer les suites d'octets, le ByteBuffer permet de les manipuler plus aisément et plus efficacement que les byte array. Cependant leurs utilisations différent et ils faut bien savoir gérer la zone de travail et ses modifications après les différents appel de méthodes.