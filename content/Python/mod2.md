+++
title = 'Module 2'
date = 2025-08-18T11:22:51-04:00
draft = false
weight = 12
+++

## Chaînes de caractères
Il est facile de déclarer des chaînes de caractères en python:
```python
s = "abcdef"
```

Elles peuvent ensuite être menipulées comme des objets, avec leurs méthodes spécifiques:
```python
s1 = "abcdef"
s2 = s1.upper()
print(s2)       ## Affiche "ABCDEF"
```

Les chaînes de caractères ont plusieurs méthodes utiles (voir [ce site](https://www.codecademy.com/learn/learn-python-3/modules/learn-python3-strings/cheatsheet) pour plus d'informations).

## "Slicing"
Python comprend une manière très utile de sélectionner les parties d'une chaîne de caractères. On appelle cette méthode "Slicing", et elle consiste à utiliser les indices des caractères dans la chaîne.

Par exemple, pour sélectionner un caractère spécifique:
```python
s = "abcdef"
print(s[0]) ## Affiche "a"
print(s[2]) ## Affiche "c"
```
Si on précède l'indice du signe `-`, on compte à partir de la fin (le dernier caractère correspond à `[-1]`):
```python
s = "abcdef"
print(s[-1]) ## Affiche "f"
print(s[-4]) ## Affiche "c"
```
Pour sélectionner plusieurs caractères entre deux indices, on les séparer par `:`. Le premier indice est inclus du résultat, le deuxième est exclus:
```python
s = "abcdef"
print(s[2:4]) ## Affiche "cd"
print(s[1:-1]) ## Affiche "bcde"
```
Finalement, si on omet un des deux indices avec `:`, tout le reste de la chaîne sera sélectionné:
```python
s = "abcdef"
print(s[:4]) ## Affiche "abcd"
print(s[2:]) ## Affiche "cdef"
```
## Saisie de texte
La fonction `input()` sert à entrer du texte à partir de la ligne de commande. Par exemple:
```python
s = input("SVP entrez votre nom: ")
print("Bonjour " + s)
```
Attention, cette fonction retourne toujours une chaîne de caractères. Si on veut saisir un nombre, il faudra la convertir:
```python
n1 = input("SVP entrez un nombre: ")
n2 = input("SVP entrez un autre nombre: ")
print("La somme est", int(n1) + int(n2))  ## Sans int(), ce sera une concaténation
```

#### Exercices
1. Demander à l'utilisateur d'entrer un mot et afficher le nombre de fois que la lettre "e" apparaît dans le mot
2. Demander à l'utilisateur d'entrer deux nombres et afficher tous les nombres entre les deux
3. Demander d'entrer un nombre et dire s'il est pair ou impair.
4. Demander d'entrer un nombre et dites s'il est premier. Vous pouvez utiliser l'algorithme suivant:
```
Pour chaque nombre N entre 1 et le nombre saisi
    Si le nombre saisi est divisible par N
        Il n'est pas premier
```
<!--
- Strings et input(), split(), slicing
- 4 exercices 30 min

n = int(input("nombre: "))
i = 2
prems = True
while i < n:
    if n % i == 0:
        prems = False
        print(n,i)
        break
    i += 1

if prems:
    print("Premier")
else:
    print("Pas prems")
-->