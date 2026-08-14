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
1. Faites un programme qui demande un mot à l'utilisateur et crée une liste où chaque élément est une lettre du mot, à l'envers. Par exemple, si le mot est "python", la liste sera ['n','o','h','t','y','p']
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

1. Afficher le résultat de ces opérations sans écrire la réponse explicitement en dur : additionner 50 et 34, soustraire 50 à 34, multiplier 4 par 80, diviser 80 par 4, modulo de 50 par 3. Puis, à partir d'une variable i = 9, ajoutez 1 et affichez, puis retirez 2 et affichez.
{{% expand "Réponse" %}}

```python

print(50 + 34)
print(50 - 34)
print(4 * 80)
print(80 / 4)
print(50 % 3)

i = 9
i += 1
print(i)
i -= 2
print(i)
```
{{% /expand %}}

1. À partir d'une variable demo = 0, effectuez les opérations suivantes sans écrire le résultat explicitement : assignez 8, augmentez de 100, réduisez de 46, multipliez par 5, divisez par 2 (division entière) et assignez le modulo 4 de sa valeur actuelle. Affichez la valeur à chaque étape.
{{% expand "Réponse" %}}

```python

demo = 0

demo = 8
print(demo)

demo += 100
print(demo)

demo -= 46
print(demo)

demo *= 5
print(demo)

demo //= 2
print(demo)

demo %= 4
print(demo)
```
{{% /expand %}}

1. Enregistrez dans des variables et affichez si 44 est égal à 66, si 44 n'est pas égal à 66, si 44 est plus grand que 66, si 44 est plus petit ou égal à 66. Ensuite, avec est_sante = True et est_abordable = False, affichez les résultats des combinaisons logiques ET, ET avec négation, et OU.
{{% expand "Réponse" %}}

```python

comp1 = (44 == 66)
comp2 = (44 != 66)
comp3 = (44 > 66)
comp4 = (44 <= 66)

print(comp1)
print(comp2)
print(comp3)
print(comp4)

est_sante = True
est_abordable = False

print(est_sante and est_abordable)
print(not est_sante and est_abordable)
print(est_sante or est_abordable)
```
{{% /expand %}}

1. Créez une liste de taille 5 remplie de nombres aléatoires entre 0 et 100, puis affichez chaque valeur une seule ligne. Ex: 34 65 0 26 55
{{% expand "Réponse" %}}

```python

import random

tableau = []
for _ in range(5):
    tableau.append(random.randint(0, 100))

for valeur in tableau:
    print(valeur, end=" ")
```
{{% /expand %}}

1. Reprenez la liste générée à l'exercice précédent et affichez-la sous un format esthétique entouré de crochets.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(5)]

# Affichage direct du format de liste en Python
print(tableau)
```
{{% /expand %}}

1. Reprenez l'affichage de l'exercice précédent et ajoutez une ligne indiquant la somme de tous les éléments de la liste.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(5)]

print(tableau)

somme = 0
for num in tableau:
    somme += num

print("La somme des éléments du tableau :", somme)
```
{{% /expand %}}

1. Créez une liste de taille 5 avec des nombres aléatoires entre 0 et 100, affichez-la, puis trouvez et affichez le maximum et le minimum sans utiliser les fonctions natives max() et min().
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(5)]
print(tableau)

maximum = tableau[0]
minimum = tableau[0]

for num in tableau:
    if num > maximum:
        maximum = num
    if num < minimum:
        minimum = num

print("Le maximum est :", maximum)
print("Le minimum est :", minimum)
```
{{% /expand %}}

1. Créez une liste de taille 5 avec des nombres aléatoires entre 0 et 100, affichez-la, puis inversez l'ordre de ses éléments à l'aide d'une boucle (sans utiliser de fonctions natives d'inversion) et affichez-la à nouveau.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(5)]
print(tableau)

n = len(tableau)
for i in range(n // 2):
    # Échange des éléments opposés
    tableau[i], tableau[n - 1 - i] = tableau[n - 1 - i], tableau[i]

print(tableau)
```
{{% /expand %}}

1. Créez une liste de taille 5 remplie de nombres aléatoires entre 0 et 100, affichez-la, puis calculez et affichez la moyenne exacte de ses éléments.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(5)]
print(tableau)

somme = 0
for num in tableau:
    somme += num
moyenne = somme / len(tableau)

print("La moyenne des éléments du tableau est :", moyenne)
```
{{% /expand %}}

1. Supprimez les doublons de la liste [5, 1, 2, 2, 1, 4, 5, 6, 6, 7, 8] en utilisant un algorithme qui vérifie pour chaque élément s'il est présent dans les indices précédents, puis affichez la nouvelle liste.
{{% expand "Réponse" %}}

```python

tableau = [5, 1, 2, 2, 1, 4, 5, 6, 6, 7, 8]
nouveau_tableau = []

for num in tableau:
    if num not in nouveau_tableau:
        nouveau_tableau.append(num)

print(nouveau_tableau)
```
{{% /expand %}}

1. Fusionnez deux listes [1, 2, 3] et [4, 5, 6] dans une seule et unique liste, puis affichez le résultat.
{{% expand "Réponse" %}}

```python

liste1 = [1, 2, 3]
liste2 = [4, 5, 6]

liste_fusionnee = liste1 + liste2
print("Tableau fusionné :", liste_fusionnee)
```
{{% /expand %}}

1. Créez une liste de taille 10 avec des nombres aléatoires entre 0 et 100, affichez-la, puis comptez et affichez le nombre de valeurs paires qu'elle contient.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(10)]
print(tableau)

nb_pairs = 0
for num in tableau:
    if num % 2 == 0:
        nb_pairs += 1

print("Il y a", nb_pairs, "nombres pairs dans ce tableau")
```
{{% /expand %}}

1. Écrivez un programme qui initialise 2 listes d'entiers, les affiche, puis compare si elles sont identiques (même taille et mêmes éléments aux mêmes index) ou différentes.
{{% expand "Réponse" %}}

```python

liste1 = [5, 1, 2, 2, 1, 4, 5, 6, 6, 7]
liste2 = [5, 1, 2, 2, 1, 4, 5, 6, 6, 7]

print(liste1)
print(liste2)

if liste1 == liste2:
    print("Identique")
else:
    print("Différent")
```
{{% /expand %}}

1. Comptez à l'aide d'une boucle le nombre de voyelles (a, e, i, o, u, y) présentes dans la liste de caractères suivante : ['a', 'b', 'e', 'f', 'i', 'o', 'u', 'p'].
{{% expand "Réponse" %}}

```python

caracteres = ['a', 'b', 'e', 'f', 'i', 'o', 'u', 'p']
voyelles = ['a', 'e', 'i', 'o', 'u', 'y']
compt = 0

for char in caracteres:
    if char in voyelles:
        compt += 1

print("Il y a", compt, "voyelles dans le tableau.")
```
{{% /expand %}}

1. Créez une liste de 10 000 nombres aléatoires entre 0 et 10 inclusivement, puis déterminez et affichez le nombre d'occurrences de chaque chiffre de 0 à 10.
{{% expand "Réponse" %}}

```python

import random

# Génération des 10 000 nombres
tableau = [random.randint(0, 10) for _ in range(10000)]

# Initialisation des compteurs pour les chiffres de 0 à 10
frequences = [0] * 11

for num in tableau:
    frequences[num] += 1

for chiffre in range(11):
    print(chiffre, ":", frequences[chiffre])
```
{{% /expand %}}

1. Créez une liste de taille 10 avec des nombres aléatoires entre 0 et 100, affichez-la, puis triez-la en implémentant l'algorithme du tri par sélection (trouver le minimum restant et l'échanger). Affichez la liste triée.
{{% expand "Réponse" %}}

```python

import random
tableau = [random.randint(0, 100) for _ in range(10)]
print(tableau)

for i in range(len(tableau)):
    indice_min = i
    for j in range(i + 1, len(tableau)):
        if tableau[j] < tableau[indice_min]:
            indice_min = j
    # Échange
    tableau[i], tableau[indice_min] = tableau[indice_min], tableau[i]

print(tableau)
```
{{% /expand %}}

1. Créez une fonction qui calcule un montant de rabais à partir d'un prix et d'un pourcentage de rabais donnés en paramètres, puis testez-la avec un affichage.
{{% expand "Réponse" %}}

```python

def calculer_rabais(prix, taux_rabais):
    return prix * (taux_rabais / 100)

prix_test = 100
rabais_test = 20
montant_rabais = calculer_rabais(prix_test, rabais_test)

print(f"Le rabais de {rabais_test}% sur {prix_test}$ est de {montant_rabais}$")
```
{{% /expand %}}

1. Créez une fonction qui affiche un reçu structuré et aligné en prenant en paramètre le nom de l'item, le prix, le pourcentage de rabais et le taux de taxation.
{{% expand "Réponse" %}}

```python

def afficher_recu(nom_item, prix, rabais_pourcent, taxe_pourcent):
    montant_rabais = prix * (rabais_pourcent / 100)
    prix_avant_taxes = prix - montant_rabais
    montant_taxe = prix_avant_taxes * (taxe_pourcent / 100)
    total = prix_avant_taxes + montant_taxe
    
    print(f"{nom_item:<22}{prix:>7.2f}$")
    print(f"rabais({rabais_pourcent}%){'-':>11}{montant_rabais:>7.2f}$")
    print(f"prix avant taxes      {prix_avant_taxes:>7.2f}$")
    print(f"taxe({taxe_pourcent}%){'':>12}{montant_taxe:>7.2f}$")
    print("-" * 30)
    print(f"Total                 {total:>7.2f}$")

afficher_recu("xbox", 500.0, 20, 16)
```
{{% /expand %}}

1. Créez une fonction qui prend une liste de caractères en paramètre et affiche si elle contient un palindrome ou non. Testez avec ['L','a','v','a','l'].
{{% expand "Réponse" %}}

```python

def verifier_palindrome_liste(liste_chars):
    # Convertir en minuscules pour ignorer la casse
    liste_sans_casse = [c.lower() for c in liste_chars]
    
    if liste_sans_casse == liste_sans_casse[::-1]:
        print("Le mot est un palindrome.")
    else:
        print("Le mot n'est pas un palindrome.")

verifier_palindrome_liste(['L','a','v','a','l'])
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une liste d'entiers et un entier cible, et qui renvoie True si l'entier cible est dans la liste, ou False sinon.
{{% expand "Réponse" %}}

```python

def contient_element(tableau, cible):
    for num in tableau:
        if num == cible:
            return True
    return False

print(contient_element([10, 20, 30, 40], 30))
```
{{% /expand %}}

1. Enregistrez l'alphabet en minuscules dans une variable, affichez sa longueur, passez-le en majuscules pour l'afficher, et trouvez la position humaine (index + 1) de la lettre 'f'.
{{% expand "Réponse" %}}

```python

alphabet = "abcdefghijklmnopqrstuvwxyz"

print("L'alphabet a", len(alphabet), "lettres")
print(alphabet.upper())

position_f = alphabet.find('f') + 1
print(f"Le f est à la {position_f}e position dans l'alphabet")
```
{{% /expand %}}

1. Écrivez un texte contenant des apostrophes et des guillemets doubles en utilisant un seul print().
{{% expand "Réponse" %}}

```python

# Utilisation de triples guillemets pour éviter les conflits de caractères
print("""L'apostrophe peut briser le code.
Les guillemets "Sont dangereux".""")
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une chaîne de caractères (ex: "Hello World"), compte combien de voyelles elle contient et affiche le résultat.
{{% expand "Réponse" %}}

```python

def compter_voyelles(chaine):
    voyelles = "aeiouyAEIOUY"
    compt = 0
    for char in chaine:
        if char in voyelles:
            compt += 1
    print(chaine)
    print("Le nombre de voyelles dans la chaîne est :", compt)

compter_voyelles("Hello World")
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une chaîne de caractères (ex: "Java") et l'inverse à l'aide d'une boucle, sans utiliser de méthode magique ou de pas négatif [::-1].
{{% expand "Réponse" %}}

```python

def inverser_chaine(chaine):
    chaine_inversee = ""
    for char in chaine:
        chaine_inversee = char + chaine_inversee
    print(f"{chaine} inversée donne : {chaine_inversee}")

inverser_chaine("Java")
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une chaîne de caractères (ex: "radar"), vérifie s'il s'agit d'un palindrome et affiche un message en conséquence.
{{% expand "Réponse" %}}

```python

def est_palindrome_chaine(chaine):
    if chaine == chaine[::-1]:
        print("La chaîne est un palindrome.")
    else:
        print("La chaîne n'est pas un palindrome.")

est_palindrome_chaine("radar")
```
{{% /expand %}}

1. Créez une fonction qui supprime tous les espaces d'une chaîne de caractères passée en paramètre (ex: "Hello World") et affiche le résultat.
{{% expand "Réponse" %}}

```python

def supprimer_espaces(chaine):
    chaine_sans_espace = chaine.replace(" ", "")
    print("La chaîne sans espaces est :", chaine_sans_espace)

supprimer_espaces("Hello World")
```
{{% /expand %}}

1. Créez une fonction qui extrait et affiche le premier et le dernier caractère d'une chaîne de caractères donnée.
{{% expand "Réponse" %}}

```python

def premier_et_dernier(chaine):
    print("Le premier caractère est :", chaine[0])
    print("Le dernier caractère est :", chaine[-1])

premier_et_dernier("Java")
```
{{% /expand %}}

1. Convertissez une chaîne de caractères donnée en majuscules puis en minuscules, et affichez les deux versions.
{{% expand "Réponse" %}}

```python

def convertir_casse(chaine):
    print("Chaîne en majuscules :", chaine.upper())
    print("Chaîne en minuscules :", chaine.lower())

convertir_casse("java")
```
{{% /expand %}}

1. Prenez la chaîne "banana", remplacez toutes les occurrences de 'a' par 'o' et affichez le résultat.
{{% expand "Réponse" %}}

```python

chaine = "banana"
nouvelle_chaine = chaine.replace('a', 'o')
print("La chaîne après remplacement est :", nouvelle_chaine)
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une chaîne de caractères et un caractère cible, compte le nombre d'occurrences du caractère et affiche le résultat.
{{% expand "Réponse" %}}

```python

def compter_caractere(chaine, cible):
    compt = chaine.count(cible)
    print(f"Le caractère '{cible}' apparaît {compt} fois dans la chaîne.")

compter_caractere("Hello World", 'o')
```
{{% /expand %}}

1. Extrayez une sous-chaîne de l'indice 0 à 5 (exclus) de la chaîne "Hello World" en utilisant le découpage de chaînes (slicing).
{{% expand "Réponse" %}}

```python

chaine = "Hello World"
sous_chaine = chaine[0:5]
print("La sous-chaîne extraite est :", sous_chaine)
```
{{% /expand %}}

1. Créez une fonction qui compare deux chaînes de caractères passées en paramètres et affiche un message si elles sont égales en ignorant la casse.
{{% expand "Réponse" %}}

```python

def comparer_sans_casse(chaine1, chaine2):
    if chaine1.lower() == chaine2.lower():
        print("Les chaînes sont égales (en ignorant la casse).")
    else:
        print("Les chaînes sont différentes.")

comparer_sans_casse("Hello", "hello")
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une liste de chaînes de caractères ["Java", "Python", "C++", "JavaScript"] et affiche toutes ces chaînes séparées par un espace sur la même ligne.
{{% expand "Réponse" %}}

```python

def afficher_liste_mots(liste_mots):
    print(" ".join(liste_mots))

afficher_liste_mots(["Java", "Python", "C++", "JavaScript"])
```
{{% /expand %}}

1. Créez une fonction qui prend en paramètre une liste de caractères ['a', 'b', 'c', 'd', 'e'] et utilise une boucle for pour reproduire l'affichage exact : [ a, b, c, d, e ].
{{% expand "Réponse" %}}

```python

def afficher_format_caracteres(liste_chars):
    resultat = "[ "
    for char in liste_chars:
        resultat += char + ", "
    # Retirer la dernière virgule en trop et fermer le crochet
    resultat = resultat[:-2] + " ]"
    print(resultat)

afficher_format_caracteres(['a', 'b', 'c', 'd', 'e'])
```
{{% /expand %}}

1. Calculez la somme des valeurs ASCII des caractères présents dans la liste ['A', 'B', 'C', 'D'] à l'aide d'une boucle.
{{% expand "Réponse" %}}

```python

liste_chars = ['A', 'B', 'C', 'D']
somme_ascii = 0

for char in liste_chars:
    somme_ascii += ord(char)

print("La somme des valeurs ASCII est :", somme_ascii)
```
{{% /expand %}}

1. Créez une liste de caractères ['a', 'b', 'c', 'a', 'd']. Remplacez toutes les occurrences de 'a' par 'x' à l'aide d'une boucle et affichez le résultat.
{{% expand "Réponse" %}}

```python

liste_chars = ['a', 'b', 'c', 'a', 'd']

for i in range(len(liste_chars)):
    if liste_chars[i] == 'a':
        liste_chars[i] = 'x'

print("Tableau après remplacement :", liste_chars)
```
{{% /expand %}}

1. Créez une liste contenant les 26 lettres de l'alphabet de 'a' à 'z' à l'aide d'une boucle et de la fonction chr(), puis affichez-la.
{{% expand "Réponse" %}}

```python

alphabet = []
# 97 est le code ASCII pour 'a'
for i in range(97, 97 + 26):
    alphabet.append(chr(i))

print(alphabet)
```
{{% /expand %}}

1. Inversez l'ordre des éléments de la liste ['a', 'b', 'c', 'd', 'e'] à l'aide d'une boucle et affichez le résultat.
{{% expand "Réponse" %}}

```python

liste_chars = ['a', 'b', 'c', 'd', 'e']
gauche = 0
droite = len(liste_chars) - 1

while gauche < droite:
    liste_chars[gauche], liste_chars[droite] = liste_chars[droite], liste_chars[gauche]
    gauche += 1
    droite -= 1

print("Tableau inversé :", liste_chars)
```
{{% /expand %}}

1. Créez une variable de type float à 9.78, convertissez-la explicitement en int (cast), puis affichez les valeurs avant et après conversion.
{{% expand "Réponse" %}}

```python

val_initiale = 9.78
val_convertie = int(val_initiale)

print("Valeur initiale (double) :", val_initiale)
print("Valeur convertie (int) :", val_convertie)
```
{{% /expand %}}

1. Créez une variable int à 7 et un float à 3.5. Additionnez-les pour exploiter la conversion implicite et affichez le résultat.
{{% expand "Réponse" %}}

```python

entier = 7
flottant = 3.5
resultat = entier + flottant

print("Résultat de l'addition :", resultat)
```
{{% /expand %}}

1. Créez une liste d'entiers non triés de manière fixe dans le code, triez-la dans l'ordre croissant et affichez l'état de la liste avant et après le tri.
{{% expand "Réponse" %}}

```python

liste = [50, 30, 20, 40, 10]
print("ArrayList avant le tri :", liste)

liste.sort()
print("ArrayList après le tri :", liste)
```
{{% /expand %}}

1. Multipliez un float de 10.5 par un int de 3, affichez le résultat, puis convertissez explicitement ce résultat en int avant de l'afficher à nouveau.
{{% expand "Réponse" %}}

```python

v1 = 10.5
v2 = 3
produit = v1 * v2
print("Résultat de la multiplication (double) :", produit)

produit_entier = int(produit)
print("Résultat après conversion en int :", produit_entier)
```
{{% /expand %}}

1. Calculez la moyenne des valeurs de la liste [80, 90, 70, 85, 100] et affichez le résultat formaté avec exactement deux décimales.
{{% expand "Réponse" %}}

```python

notes = [80, 90, 70, 85, 100]
moyenne = sum(notes) / len(notes)

print(f"La moyenne des notes est : {moyenne:.2f}")
```
{{% /expand %}}

1. Convertissez la liste de chaînes de caractères ["10", "20", "30", "40", "50"] en une liste d'entiers numériques, puis affichez le résultat.
{{% expand "Réponse" %}}

```python

liste_strings = ["10", "20", "30", "40", "50"]
liste_ints = []

for item in liste_strings:
    liste_ints.append(int(item))

print("Tableau :", liste_ints)
```
{{% /expand %}}

1. Créez une fonction qui prend deux chaînes de caractères au format date ISO en paramètres et renvoie le nombre de jours exacts séparant ces deux dates de manière exclusive. Testez avec "2022-12-01" et "2023-01-15".
{{% expand "Réponse" %}}

```python

from datetime import datetime

def jours_entre_dates(date_str1, date_str2):
    d1 = datetime.strptime(date_str1, "%Y-%m-%d")
    d2 = datetime.strptime(date_str2, "%Y-%m-%d")
    # Soustraction absolue, moins 1 jour pour l'exclusivité
    difference = abs((d2 - d1).days) - 1
    print(f"Il y a {difference} jour entre le {date_str1} et le {date_str2} (exclusivement)")

jours_entre_dates("2024-02-04", "2024-02-06")
```
{{% /expand %}}

1. Créez une liste contenant un ensemble de nombres aléatoires désordonnés, implémentez une fonction pour calculer la moyenne, puis une fonction pour calculer la médiane (en triant la liste au préalable).
{{% expand "Réponse" %}}

```python

def calculer_statistiques(liste_nombres):
    print("Liste des nombres :", liste_nombres)
    
    # Moyenne
    moyenne = sum(liste_nombres) / len(liste_nombres)
    
    # Médiane
    liste_triee = sorted(liste_nombres)
    n = len(liste_triee)
    if n % 2 != 0:
        mediane = float(liste_triee[n // 2])
    else:
        milieu1 = liste_triee[(n // 2) - 1]
        milieu2 = liste_triee[n // 2]
        mediane = (milieu1 + milieu2) / 2.0
        
    print(f"\nMoyenne des nombres : {moyenne:.2f}")
    print(f"Médiane des nombres : {mediane:.1f}")

calculer_statistiques([22, 56, 33, 44, 12, 77, 11, 50])
```
{{% /expand %}}

1. Calculez la variance et l'écart-type de la liste de valeurs numériques [12.5, 15.2, 19.0, 14.3, 10.7] à l'aide de formules mathématiques bouclées et affichez les résultats.
{{% expand "Réponse" %}}

```python

import math

valeurs = [12.5, 15.2, 19.0, 14.3, 10.7]
print("Valeurs :", valeurs)

# Moyenne
moyenne = sum(valeurs) / len(valeurs)

# Variance
somme_ecarts_carre = 0
for x in valeurs:
    somme_ecarts_carre += (x - moyenne) ** 2
variance = somme_ecarts_carre / len(valeurs)

# Écart-type
ecart_type = math.sqrt(variance)

print(f"\nVariance : {variance}")
print(f"Écart-type : {ecart_type:.3f}")
```
{{% /expand %}}

1. Prenez la liste d'âges de population [12, 45, 32, 19, 58, 60, 28, 16] et séparez-la de manière logicielle en trois sous-groupes : moins de 18 ans (mineurs), entre 18 et 50 ans inclusivement (adultes), et plus de 50 ans (seniors). Affichez les sous-groupes obtenus.
{{% expand "Réponse" %}}

```python

ages = [12, 45, 32, 19, 58, 60, 28, 16]

mineurs = []
adultes = []
seniors = []

for age in ages:
    if age < 18:
        mineurs.append(age)
    elif age <= 50:
        adultes.append(age)
    else:
        seniors.append(age)

print("Mineurs :", sorted(mineurs))
print("Adultes :", sorted(adultes))
print("Seniors :", sorted(seniors))
```
{{% /expand %}}