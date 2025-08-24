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
<!--
{{% expand "Réponses" %}}
```python
lettre = "e"
mot = input("Entrez un mot: ")
i = 0
compteur = 0

while i < len(mot):
    if mot[i] == "e":
        compteur += 1
    i += 1

print(lettre,"apparaît",compteur,"fois.")
```
{{% /expand %}}
-->
2. Demander à l'utilisateur d'entrer deux nombres et afficher tous les nombres entre les deux (inclus)
<!--
{{% expand "Réponses" %}}
```python
n1 = int(input("Entrez un nombre: "))
n2 = int(input("Entrez un autre nombre: "))
while n1 <= n2:
    print(n1)
    n1+=1
```
{{% /expand %}}
-->
3. Demander d'entrer un mot et affichez-le sans le premier et le dernier caractère. Par exemple, le mot "python" serait affiché "ytho"..
<!--
{{% expand "Réponses" %}}
```python
mot = input("Entrez un mot: "))
print(mot[1:-1])
```
{{% /expand %}}
-->
4. Demander d'entrer un nombre et dites s'il est premier. Vous pouvez utiliser l'algorithme suivant:
```
Pour chaque nombre N entre 1 et le nombre saisi
    Si le nombre saisi est divisible par N
        Il n'est pas premier
```
<!--
{{% expand "Réponses" %}}
```python
n = int(input("Entrez un nombre: "))
i = 2
est_premier = True

while i < n:
    if n % i == 0:  
        est_premier = False
        break
    i+=1

if est_premier:
    print(n,"est premier")
else:
    print(n,"n'est pas premier")
```
{{% /expand %}}
-->
