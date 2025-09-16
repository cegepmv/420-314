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
Pour ajouter _un élément_ à une liste existante, on utilise `append()`:
```python
nombres = [56,11,-109,66]
nombres.append(999)
print(nombres)      ## Affiche [56,11,-109,66,999]
```
Pour ajouter _tous les éléments d'une liste_ à une autre liste, la méthode est `extend()`:
```python
liste1 = [56,11,-109,66]
liste2 = ["allo","bye"]
liste1.extend(liste2)
print(liste1)       ## Affiche [56,11,-109,66,"allo","bye"]
```

## Boucles _for_
En python, la boucle _for_ doit être utilisée avec une **liste** ou une **chaîne de caractères**. Elle prend la forme `for VARIABLE in LISTE`, ou `in` est suivi de la liste ou de la chaîne de caractères:
```python
liste = [1,2,3,4,5]
for nombre in liste:
    print(nombre + 10)
```
Le programme suivant utilise FOR pour parcourir une chaîne de caractères et voir si elle contient un chiffre:
```python
mot = input("Entrez un mot: ")
for caract in mot:
    if caract.isnumeric():
        print("Il y a le chiffre", caract, "dans le mot!")
        break
```

#### Exercices
1. Demander d'entrer un mot, puis une lettre, et ensuite afficher le nombre de fois que la lettre apparaît dans le mot. 
{{% expand "Réponse" %}}
```python
mot = input("Entrez un mot: ")
lettre = input("Entrez une lettre: ")
compt = 0

for caract in mot:
    if caract == lettre:
        compt += 1

print(lettre,"apparaît",compt,"fois dans",mot)

```
{{% /expand %}}
2. Faites un programme qui demande un mot à l'utilisateur et crée une liste où chaque élément est une lettre du mot, à l'envers. Par exemple, si le mot est "python", la liste sera ['n','o','h','t','y','p']
{{% expand "Réponse" %}}
```python
mot = input("Entrez un mot: ")
liste = []
i = 1
while i <= len(mot):
    liste.append(mot[-i])
    i+=1

print(liste)
```
ou encore
```python
mot = input("Entrez un mot: ")
liste = list(mot[::-1])
print(liste)
```
{{% /expand %}}


  