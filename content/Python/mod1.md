+++
title = 'Module 1'
date = 2025-08-18T11:22:48-04:00
draft = false
weight = 11
+++
<!--
- IDE à utiliser?
- print hello world, mettre dans une variable : pas de type
- truc = "10+3", 10+3, 10-3, 10*3... il manque quoi? : Division. / vs //. 
- 10>3?? : éval des opérateurs. Opérateurs booléens.
- Revenons à hello: print 2 lignes, 3 lignes... 500 lignes? boucles while
- print(x+y) vs. print(x,y)
- indentation en python: les ":", les erreurs d'indentation (démo)
- conditions
-->

Pour ce module et les suivants, nous allons utiliser *IDLE*, un éditeur de code simple inclus avec la distribution du langage python.

## Afficher un message
Pour afficher quelque chose à l'écran en python, on utilise la fonction `print()`:
```python
print("Salut")
```
Cette fonction peut prendre n'importe quel type d'expression en argument:
```python
a = 10
b = 5
print(a) ## Affiche 10
print(a + b) ## Affiche 15
print("abc" + "def") ## Affiche "abcdef"
```
La fonction `print()` peut également prendre plusieurs arguments de types différents lorsqu'ils sont séparés d'une virgule:
```python
a = 100
b = "bonjour"
print(a,b) ## Affiche "100 bonjour"
```

## Types
En python les variables peuvent être déclarées sans qu'on spécifie leur type:
```python
a = 23       ## Type int
b = "abcdef" ## Type str
c = 3.1416   ## Type float
```
Mais attention, une fois qu'elles ont été affectées, leur type ne change pas sauf si on utilise une fonction de conversion ou qu'on leur affecte une nouvelle valeur:
```python
a = 23       
b = "abcdef" 

## Cause une erreur car on ne peut pas concaténer un int et un str
print(a + b)  

## Pas d'erreur car on change le type de a
print(str(a) + b)

## Pas d'erreur car a est un str
a = "xyz"
print(a + b)
```

Les fonctions de conversion sont `int()`, `float()` et `str()`.

## Opérateurs
Lorsqu'une expression contient un opérateur, le langage de programmation va *évaluer* l'expression: c'est-à-dire qu'il doit lui **donner une valeur**.

Cette valeur dépend de l'opérateur utilisé:
- Un opérateur *arithmétique* (`+`, `-`, `*`, `/`, `/`/, `^`, `%`) calcule le résultat de l'opération 
- Les opérateurs de comparasion (`<`, `>`, `==`, `!=`) déterminent si l'expression est *vraie* ou *fausse* à partir de variables de n'importe quel type
- Les opérateurs logiques (`or`, `and`) déterminent si l'expression est *vraie* ou *fausse* à partir de variables ou d'expression qui sont elles-mêmes vraies ou fausses.

## Structures de contrôle
Les structures de contrôle en python n'utilisent pas de délimiteur comme `{ }` en java ou javascript. C'est l'indentation qui détermine si des lignes de code font partie d'un bloc:
```python
if a < 100:
    print("a est plus petit que 100")
elif a > 100:
    print("a est supérieur à 100")
else:
    print("a est égal à 100")
```

Remarquez aussi qu'on n'utilise pas de parenthèses autour de la condition:
```python
i = 0
while i < 10:   ## Pas de parenthèses autour de "i < 10"
    print(i)
    i += 1
```

#### Exercices
1. Faire un programme où vous définissez deux variables (`n1` et `n2`) correspondant à deux nombres entiers. Le programme doit afficher tous les nombres entre *n1* et *n2*.  
2. Faire un programme qui affiche les 20 premiers multiples de 7 (7, 14, 21, 28...)
3. Faites un programme où vous définissez une variable entière nommée `n`. Ensuite, affichez un carré du caractère "*" dont les côtés ont la taille *n*. Par exemple, si `n = 5`, votre programme devrait afficher ceci:
```
  *****
  *****
  *****
  *****
  *****
```
4. Traduire le programme Java suivant en python:
```java
public class Exercice4 {
 
 public static void main(String[] args) {
   int a, b, temp;
   
   a = 15;
   b = 27;
   
   System.out.println("Avant : a, b = " + a + ", " + b);
   
   temp = a;
   a = b;
   b = temp;   
   
   System.out.println("Après : a, b = " + a + ", " + b);
 }
}
```  
5. Traduire le programme Java suivant en python:
```java
import java.util.Scanner;

public class Exercice5 {
    public static void main(String[] args) {
        int nombre = 0; 
        
        for (int i = 1; i <= 4; i++) {
            for (int j = 1; j <= 4; j++) {
                for (int k = 1; k <= 4; k++) {
                    if (k != i && k != j && i != j) {
                        nombre++; 
                        System.out.println(i + "" + j + "" + k); 
                    }
                }
            }
        }
        
        System.out.println(nombre + " nombres générés.");
    }
}
```

