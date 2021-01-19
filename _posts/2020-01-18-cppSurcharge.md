---
layout: post
title:  "Programmation en C++: La surcharge"
date:   2021-01-18  8:00:00 +0100
categories: programmation cpp
tags: programmation cpp classe objet
published: false
---

# La surcharge

## Définition

En programmation, la surcharge est un mécanisme permettant de donner plusieurs **définitions** différentes à un même **identificateur**. 

    ```cpp
        float add(float a, float b);
        int add(int a, int b);
    ```

nous verrons ici des surcharges très utiles lorsque l'on code en cpp.

## Le constructeur de copie

Supposons que nous avons définit une classe Foo. L'instruction suivante
```cpp
    Foo a {};
    Foo b = a
```
utilise un constructeur de copie implanter par défaut pour copier l'instance a vers  b. 

On peut surcharger le constructeur de copie pour définir le comportement lors d'une copie d'une instance vers une nouvelle instance.

```cpp
class Foo
{
    public: 
        Foo(const Foo& other)
        : _attribute1 { other._attribute1}
        , _attribute2 { ""}
        {
        }

    private:
        int         _attribute1;
        std::string _attribute2;
};
```

Ici, j'ai choisi d'uniquement copier le premier attribut et de donner la chaine vide au second lors d'une copie. On peut faire ce que l'on veut.

## Redéfinir un opérateur

On peut même redéfinir un opérateur (+, - , >>, <<, =, etc...), la syntaxe est assez singulière...

### +

```cpp
int operator+(const Foo& f1, const Foo& f2)
{
    return f1._attribute1 + f2.attribute1;
}

Foo f1 {1, "f1"};
Foo f2 {1, "f2"};
f1 + f2; // 2
```

### =
Comme avec la copie, l'assignation d'une classe peut se redéfinir.

```cpp
Foo& operator=(const Foo& other){
    if(this != &other)
    {
        _attribute_1 = other.attribute1;
        _attribute_2 = "";
    }
    return *this;
}

Foo a {1, "a"};
Foo b {2, "b"};
b = a; // b aura les attributs {1,""} ; Attention ! Il ne faut pas confondre l'assignation avec la copie vue plus haut.
```
Ici this est un pointeur vers la référence de l'instance de la classe. 
Un autre point important est la comparaison entre this et other, si other possedait un attribut alloué dynamiquement (Comme un tableau d'int par exemple), il ne faudrait pas que this et other partage la même référence vers ce tableau sans quoi les modifications de l'un impacterait les modifications de l'autre.

```cpp
class Bar
{
    public:
        Bar& operator=(const Bar& other){
        if(this != &other)
        {
            _attribute1 = new int[4]; // pas de other.attribute1;
            for(int i = 0; i < 4; i++){
                _attribute1[i] = other.attribute1[i];
            }                           
        }
    return *this;
}

    private:
        int* attribute1 = new int[4];  
};

```
