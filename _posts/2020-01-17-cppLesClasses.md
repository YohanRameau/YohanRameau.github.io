---
layout: post
title:  "Programmation en C++: Introduction"
date:   2021-01-17  18:00:00 +0100
categories: programmation cpp
tags: programmation cpp
---

# Les classes en Cpp

## Introduction

L'un des grands apports de Cpp par rapport au langage C est la programmation objet.

Bien que Cpp ne soit pas un langage purement orienté objet et que l'on puisse construire par exemple des fonctions en dehors de classe, le langage possède plusieurs outils pour créer du code respectant les principes de la programation objet.

### Qu'est ce que la programmation objet (Quelques notions)

En programmation, un **objet** est un élément qui est constitué de plusieurs valeurs (des champs) variant au cours du programme.

Pour faire varier les valeurs de notre objet on utilise une **interface**.

L'un des gros avantages de la programmation est de **restreindre les changements d'états** de nos objets par l'utilisateur en décidant de lui fournir ou non une interface. 
C'est le principe de **l'encapsulation**: les valeurs de nos objets sont masqués et les changements de valeurs sont *invisibles* pour l'utilisateur.

Lorsque l'on créer un élément d'une classe, on dit que l'on créer une **instance** de cette classe.

## La Programmation Objet en Cpp

### Définition d'une classe, de ses attributs et de ses fonctions-membre

```cpp
    class foo
    {
    public:
        void methode (int arg1, int arg2)
        {
            //
        }
        int get_member2() const 
        { 
            return _member2; 
        }
        void set_member1(int value)
        {
            _member1 = value;
        }
    private:
        int:         _member1 = 0;
        std::string  _member2;   
    }; 
```

#### public et privée

En Cpp pour respecter le principe de l'encapsulation, le code d'une classe est séparée en deux parties:
- la partie publique accessible en dehors de la classe
- et la partie privée accessible uniquement en interne de la classe.

Il faut donc choisir minutieusement, quelles méthodes/champs de la classe sera publique ou privée afin de pouvoir contrôler au maximum les changements d'états (et de prévenir des bugs difficiles à résoudre).

#### Les attributs 

Ce sont les variables qui constituent la classe. Dans notre exemple ce sont _membre1 et _membre2. Ils constituent l'état de l'objet.

    Pour accéder et modifier la valeur d'un attribut on passe par des getters et des setters, mes méthodes permettent de laisser les attributs dans le bloc private. Nous verrons plus tards, qu'il vaut mieux éviter d'implémenter des setters dans une classe lorsque ce n'est pas justifié.

#### Les fonctions-membre 

Ce sont les fonctions définit à l'intérieur de la classe comme get_value. Elles permettent à une instance de la classe d'intéragir et de voir ses états changés en fonction d'autres éléments du programme. Ils constituent l'interface de l'objet.

#### Const-Correctness 

Les fonctions-membre qui ne doivent pas changer la valeur des attributs doivent avoir avoir le mot-clé **const** placé à la fin de leurs définition. Cela va permettre au compilateur de vérifier que ces fonctions-membre ne modifient aucun attribut de l'instance. 

### Constructeur

Il est plus agréable de pouvoir initialiser plusieurs attributs lors de l'instanciation d'une classe. Cela est très utile pour les attributs qui doivent garder une valeur fixe pendant le programme car cela permet de ne pas avoir besoin de setter pour leur attribuer une valeur. 

    Les setters d'un attribut doivent-être définit seulement quand c'est nécessaire, généralement il vaut mieux éviter.

#### Syntaxe d'une classe avec un constructeur

```cpp
class Person
{
public:
    Person()
    {}
    Person(const std::string& name, unsigned int age)
        : _name { name }
        , _age  {age}
    {
        std::cout << "Person named " << name << " has just been created!" << std::endl;
    }

private:
    std::string _race;
    std::string _name;
};
```
Le constructeur est quasiment une fonction-membre sans type de retour ayant le même identificateur que sa classe. 

    On utilise un unsigned int pour l'age car unsigned int correspond aux entiers non signé.


#### Instanciation 

Il y a plusieurs syntaxes d'instanciations possible en C++.

```cpp
Person person {"Yohan", 23};
Person person ("Yohan", 23);
Person person {};
Person person "Yohan"; // Si on peut initialiser avec un seul paramètre.
```

#### Constructeur par défaut

Dans notre classe Person il y a deux constructeurs. Le premier constructeur : `` Personn() {} `` Est un constructeur par défaut, s'il n'est pas définit le compilateur oblige à instancier notre classe. 
En d'autre terme il est impossible de faire ça : ``Person person;`` . Les attributs de la fonctions prendront comme valeur, leurs initialisation s'ils sont initialiser lors de leurs définitions, ou leur initialisation par défaut. (par exemple "" pour les string, valeur indéfinit pour les type primitifs).

## Pour conclure

Cpp dispose de tous les outils nécessaire pour faire de la programmation objet proprement. Au moment de définir une classe, ses attributs et ses fonctions-membres il faut bien réfléchir pour définir ce qui sera privée et public. 

Les attributs qui ne sont pas censer changer au cours du programme, ne doivent pas avoir de setter. Le but est de respecter au maximum l'encapsulation.