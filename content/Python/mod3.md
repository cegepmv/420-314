+++
title = 'Module 3'
date = 2025-08-18T11:22:54-04:00
draft = false
weight = 13
+++

## Listes
En python les listes sont déclarées à l'aide de crochets:
```python
nombres = [56,11,-109,66]
```
Les éléments peuvent être de types différents (et même être des listes elles-mêmes):
```python
liste = [100,"allo",2.33333,[1,2,3,4],"Z"]
```
On accède aux éléments de la liste avec leur indice:
```python
liste = [100,"allo",2.33333,[1,2,3,4],"Z"]
print(liste[0])     ## Affiche 100
print(liste[3])     ## Affiche [1,2,3,4]
print(liste[3][1])  ## Affiche 2
```
Les éléments des listes peuvent, comme les chaînes de caractères, être extraits par _slicing_:
```python
nombres = [1,2,3,4,5,6,7]
print(nombres[:4])      ## Affiche [1,2,3,4]
```

#### Méthodes des listes
Pour ajouter un élément à une liste existante, on utilise `append()`:
```python
nombres = [56,11,-109,66]
nombres.append(999)
print(nombres)      ## Affiche [56,11,-109,66,999]
```
Pour ajouter une liste à une autre, la méthode est `extend()`:
```python
liste1 = [56,11,-109,66]
liste2 = ["allo","bye"]
liste1.extend(liste2)
print(liste1)
```

## Boucles _for_
En python, la boucle _for_ doit être utilisée avec une liste ou une chaîne de caractères. Elle prend la forme `for VARIABLE in LISTE`, ou `in` est suivi de la liste ou de la chaîne de caractères. Quelques exemples:
```python
liste = [1,2,3,4,5]
for nombre in liste:
    print(nombre + 10)

mot = "bonjour"
for lettre in mot:
    if lettre in ['n','o','p']:  ## Autre utilisation de "in"
        print("Trouvé un " + lettre)
```

#### Exercices
1. Demander d'entrer un mot, puis une lettre, et ensuite afficher le nombre de fois que la lettre apparaît dans le mot. 
2. Faites un programme qui inverse une chaîne de caractères entrée par l'utilisateur. Par exemple, pour "salut", le programme affiche "tulas". N'utilisez pas `[::-1]`.


  