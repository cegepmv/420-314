+++
title = 'Matrice LED 8x8'
date = 2024-11-21T11:28:46-05:00
draft = false
weight = 83
+++

La matrice de LED founie dans le kit KS0522 utilise le protocole de communication I2C, mais puisque celle-ci est en "output" et donc n'envoie pas de signal analogique au RaspberryPi, on n'a pas besoin du convertisseur ADC pour l'utiliser. Elle est équipée d'une puce HT16K33, conçue spécialement pour contrôler l'affichage dans des matrices de LED de différentes dimensions. 

Dans cette section nous verrons comment utiliser le module `adafruit_bus_device` pour la contrôler.

<!--
## adafruit_ht16k33

Ce module définit de nombreuses classes permettant de contrôler facilement divers types de matrices de LED. Il permet d'éviter d'avoir à gérer les adresses mémoire de chacune des LED séparément.

Pour l'uitiliser, il faut d'abord installer le module `ht16k33` comme suit:
```bash
sudo pip3 install adafruit-circuitpython-ht16k33 --break-system-packages
```
La référence:

https://docs.circuitpython.org/projects/ht16k33/en/latest/api.html#adafruit_ht16k33.matrix.Matrix8x8


Le courant envoyé au module doit être de 3.3V.

La librairie définit une classe `matrix` qui est utilisable comme une liste en python. Cette liste est un tableau de 8x8 dont les éléments peuvent avoir la valeur 0 (éteint) ou 1 (allumé).

### Exemple
Pour allumer un point aux coordonnées [2,2] de la matrice:

```python
import board
import busio
from time import sleep
from adafruit_ht16k33 import matrix

i2c = busio.I2C(board.SCL, board.SDA)
mat = matrix.Matrix8x8(i2c)

mat[2,2] = 1
sleep(1)
mat.fill(0) # Eteint toutes les LED
```

### Propriétés
La classe `matrix` a les propriétés suivantes:

###### blink_rate : int
Valeur de 0 à 3:
+ 1: 4 fois parseconde 
+ 2: 2 fois par seconde 
+ 3: 1 fois par seconde

```python
mat.blink_rate = 2
```

###### brightness : float
Valeur entre 0 et 1; change la luminosité de l'ensemble des points.

```python
mat.brightness = 0.2
```
###### rows : int, columns : int
Donnent le nombre de rangées et de colonnes de la matrice. Lecture seule
```python
print(mat.columns)
```

### Méthodes
###### fill(int)
Le paramètre peut avoir une valeur de 0 ou 1. Éteint ou allume l'ensemble des points.

```python
mat.fill(1)
```

###### shift(int, int, bool)
Permet de décaler le point selon les valeurs des deux premiers paramètres. Si le booléen est *Vrai*, le point qui dépasse la limite de la matrice apparaîtra du côté opposé.
```python
mat.shift(1,3,True)
```

###### shift_left(bool), shift_right(bool), shift_up(bool), shift_down(bool)
Décale le point de 1 dans le sens spécifié. Si le booléen est *Vrai*, le point qui dépasse la limite de la matrice apparaîtra du côté opposé.
```python
mat[6,7]
mat.shift_up()
mat.shift_right(True)
```
-->


## adafruit_bus_device

On utilisera la classe `I2CDevice` pour communiquer avec la matrice de LED. Cette classe comprend les deux méthodes suivantes::

###### readinto(buf)
Lit les données sur le composant I2C (dans notre cas, la matrice de LED) et les écrit dans la variable désignée par `buf`. Ces données sont au format binaire.

###### write(buf)
Écrit sur le composant I2C les données contenues dans la variable `buf`.

Le référence:

https://docs.circuitpython.org/projects/busdevice/en/latest/api.html

### Exemple 1
Dans le programme suivant on allume une série de LED sur chaque rangée successivement:
```python
import board
import busio
from time import sleep
import adafruit_bus_device.i2c_device as i2c_device

i2cBus = busio.I2C(board.SCL, board.SDA)
module = i2c_device.I2CDevice(i2cBus, 0x70)

octetAffiche = 165 #10100101
for i in range(0,16,2):
    module.write(bytes([i])) # Spécifier l'adresse qui sera utilisée
    module.write(bytes([i,octetAffiche])) # Écrire les données à l'adresse
    sleep(1)
    module.write(bytes([i,0])) # Écrire les données (vides) à l'adresse
```

> Il est aussi possible de représenter les nombres directement en binaire dans les programmes python: il s'agit de les précéder de `0b`. Par exemple dans le programme précédent on peut remplacer `octetAffiche = 165` par `octetAffiche = 0b10100101`.

`0x70` est l'adresse que le RaspberryPi utilise pour référer à la matrice de LED. Chaque composante I2C a une adresse standard qui peut aller de 0x08 à 0x77: vous pouvez la voir en utilisant la commande `i2cdetect -y 1` dans linux.

Chaque rangée de la matrice est représentée par un octet: la valeur de cet octet détermine quelles LED seront allumées. Par exemple, pour allumer la première LED, l'octet sera `10000000`, ce qui correspond au nombre 128. Le nombre 129 est `10000001` en binaire, donc si on écrit 129 ce sont les deux LED des extrémités qui s'allumeront, etc.

Lorsqu'on veut écrire nos 8 bits de données pour allumer les LED dans une rangée, on doit spécifier à quelle adresse écrire ces 8 bits.Notre matrice a seulement besoin de 8 octets pour représenter l'état des 64 LED. Cependant la puce que la matrice utilise est faite pour fonctionner avec les matrice 16x16. En conséquence, plusieurs adresses sont inutiles. Les seules adresses qui nous sont utiles sont 0x00, 0x02, 0x04, 0x06, 0x08, 0x0a, 0x0c, 0x0e. Le tableau suivant illustre les adresses utilisables:

![ledgrid](/420-314/images/ledgrid.png?width=400px)

## Exemple 2
Dans le programme suivant on allume chaque LED de la première rangée une après l'autre:
```python
import board
import busio
from time import sleep
import adafruit_bus_device.i2c_device as i2c_device

i2cBus = busio.I2C(board.SCL, board.SDA)
module = i2c_device.I2CDevice(i2cBus, 0x70)

for octet in [1,2,4,8,16,32,64,128]:
    module.write(bytes([0])) # Spécifier l'adresse qui sera utilisée
    module.write(bytes([0,octet])) # Écrire les données à l'adresse
    sleep(1)
    module.write(bytes([0,0])) # Écrire les données (vides) à l'adresse
```

## Exemple 3
Il est possible de lire dans la composante I2C les données à une adresse spécifique. Dans le programme suivant on allume une série de LED dans la même rangée et ensuite on lit la valeur de la rangée:
```python
import board
import busio
from time import sleep
import adafruit_bus_device.i2c_device as i2c_device

i2cBus = busio.I2C(board.SCL, board.SDA)
module = i2c_device.I2CDevice(i2cBus, 0x70)

# Ecrire une valeur
octet = 45
module.write(bytes([0]))
module.write(bytes([0,octet])) 

# Lire la valeur
donnees = bytearray(1) # Créer la liste d'octets
module.write(bytes([0]))
module.readinto(donnees)
print("Valeur binaire:",bin(donnees[0]))
```
La méthode `readinto()` a une séquence d'octets, créée par la fonction `bytearray()`, comme argument. Les données lues sont écrites dans cette séquence d'octets.

Avant d'appeler `readinto()`, le protocole I2C requiert qu'on envoie à la composante l'adresse qu'on veut lire avec `write()`.

## Opérations "bitwise"
Le protocole I2C requiert souvent qu'on se représente les données sous forme de bits et d'octets. Par exemple, on pourrait avoir des composants où, pour activer une fonctionnalité particulière, on doit envoyer la séquence de 4 bits `0101` (5) à l'adresse `0x4a` (74). Dans ce contexte, il est important de savoir se représenter les données numériques au format binaire, et aussi de comprendre le fonctionnement des opérateurs *bitwise*, qui sont utiles pour effectuer des opérations sur les bits individuels des nombres binaires. 

Il y a 4 opérateurs bitwise en python: 
+ `&` : ET logique
+ `|`: OU logique
+ `<<` : Décalage vers la gauche
+ `>>` : Décalage vers la droite

#### ET logique
Le ET logique correspond à faire une opération de conjonction sur **chacun des bits** correspondants de deux nombres. Le résultat vaut 1 si et seulement si les deux bits valent 1:
- 1 ET 1 = 1
- 1 ET 0 = 0
- 0 ET 1 = 0
- 0 ET 0 = 0

Dans l'exemple suivant, 235 ET 173 = 169:
```
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
1 0 1 0 1 0 0 1  (169)
```
En python, on peut effectuer cette opération avec le symbole `&`:
```
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 & n2
169
>>> bin(n1 & n2)
'0b10101001'
```

#### OU logique
Le OU logique correspond à faire une opération de disjonction sur chacun des bits correspondants de deux nombres. Le résultat vaut 1 lorsqu'au moins un des deux bits vaut 1:
- 1 OU 1 = 1
- 1 OU 0 = 1
- 0 OU 1 = 1
- 0 OU 0 = 0

Par exemple, 235 OU 173 = 239:
```
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
1 1 1 0 1 1 1 1  (239)
```
En python, on peut effectuer cette opération avec le symbole `|`:
```
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 | n2
239
>>> bin(n1 | n2)
'0b11101111'
```


#### OU exclusif (XOR)
Pour le OU exclusif, les deux bits correspondants doivent être différents pour que le résultat soit 1:
- 1 XOR 1 = 0
- 1 XOR 0 = 1
- 0 XOR 1 = 1
- 0 XOR 0 = 0

Dans l'exemple suivant, 235 XOR 173 = 70: 
```
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
0 1 0 0 0 1 1 0  (70)
```
En python, on peut effectuer cette opération avec le symbole `^`:
```
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 ^ n2
70
>>> bin(n1 ^ n2)
'0b1000110'
```

#### Décalage ("bit shift")
Les opérations de décalage consistent à "glisser" la séquence de bits vers la gauche ou la droite. Concrètement cela correspond soit à ajouter un ou plusieurs 0 à la fin de la séquence (décalage à gauche), soit à supprimer un certain nombre de bits à la fin de la séquence (décalage à droite).

Dans l'exemple suivant, on décale de 1 vers la gauche le nombre 15; le résultat est 30:
```
01111 (15)
11110 (30)
```
En python, l'opérateur utilisé pour le décalage à gauche est `<<`:
```
>>> 15 << 1
30
>>> bin(15);bin(30)
'0b1111'
'0b11110'
```

Dans l'exemple suivant, on décale de 1 vers la droite le nombre 54; le résultat est 27:
```
110110 (54)
011011 (27)
```
En python, l'opérateur utilisé pour le décalage à droite est `>>`:
```
>>> 54 >> 1
27
>>> bin(54);bin(27)
'0b110110'
'0b11011'
```

{{% notice info "Remarque" %}}
Étant donné que chaque position dans un nombre binaire est une puissance de 2, à chaque décalage vers la gauche de 1 bit, on multiplie par 2 :
```
10 << 3 = 80 =  10 x 2³
```
Inversement, à chaque décalage à droite de 1 bit on divise (division entière) par 2:
```
67 >> 3 = 8 = 67 // 2³
```
{{% /notice %}}


### Exercices
1. Faites une fonction nommée `allumerTout(moduleI2C)` prenant une instance de I2CDevice en argument qui allume toutes les LED.
{{% expand "Solution" %}}
```python
def allumerTout(mod):
    for i in range(0,16,2):
        mod.write(bytes([i]))
        mod.write(bytes([i,255]))
```
{{% /expand %}}


2. Faites une fonction nommée `remplir(moduleI2C,allume)` qui allume toutes les LED si `allume` (un booléen) est Vrai, et les éteint si `allume` est Faux.
<!--
{{% expand "Solution" %}}
```python
def remplir(mod,on):
    for i in range(0,16,2):
        mod.write(bytes([i]))
        if on:
            mod.write(bytes([i,255]))
        else:
            mod.write(bytes([i,0]))
```
{{% /expand %}}
-->

3. Faites un programme semblable à celui de l'exemple 2 plus haut, mais qui allume la première LED de chaque rangée (au lieu de chaque LED d'une même rangée)
<!--
{{% expand "Solution" %}}
```python
(...)
for addr in [0,2,4,6,8,10,12,14]:
    module.write(bytes([addr])) # Spécifier l'adresse qui sera utilisée
    module.write(bytes([addr,255])) # Écrire les données à l'adresse
    sleep(1)
    module.write(bytes([addr,0])) # Écrire les données (vides) à l'adresse
(...)
```
{{% /expand %}}
-->
4. Faites une fonction nommée `rangee(moduleI2C,ligne)` qui allume toutes les LED de la ligne passée (un entier de 0 à 7). Utilisez l'opérateur `<<` pour convertir ce nombre entier dans un numéro de rangée possible (0, 2, 4, 6, 8, 10, 12, 14).
<!--
{{% expand "Solution" %}}
```python
def rangee(mod,rg):
    i = rg << 1
    mod.write(bytes([i]))
    module.write(bytes([i,255]))
```
{{% /expand %}}
-->

5. Faites une fonction nommée `allumer(moduleI2C,ligne,colonne)` qui allume une LED à la ligne et à la colonne passées (des entiers de 0 à 7). Utilisez l'opérateur `<<` pour calculer l'adresse de la rangée et aussi pour déterminer quelle LED allumer.
<!--
{{% expand "Solution" %}}
```python
def allumer(mod,rg,col):
    adr = rg << 1
    octet = 1 << col
    mod.write(bytes([adr]))
    mod.write(bytes([adr,octet]))
```
{{% /expand %}}
-->
6. Faites un programme qui allume plusieurs LED sur toutes les rangées (avec la fonction `random.randint()`) puis affichez à la console les valeurs (en binaire) de chaque rangée.
<!--
{{% expand "Solution" %}}
```python
import board
import busio
from time import sleep
import adafruit_bus_device.i2c_device as i2c_device
from random import randint

i2cBus = busio.I2C(board.SCL, board.SDA)
module = i2c_device.I2CDevice(i2cBus, 0x70)

for i in range(0,8):
    # Allumer les LED d'un rangée au hasard
    octet = randint(0,255)
    rg = i << 1
    module.write(bytes([rg]))
    module.write(bytes([rg,octet])) 

    # Afficher la valeur de la rangée
    donnees = bytearray(1)
    module.write(bytes([rg]))
    module.readinto(donnees)
    print(donnees[0],bin(donnees[0]))
```
{{% /expand %}}
-->
7. Si on appelle plusieurs fois la fonction `allumer()` du numéro 4 pour la même rangée, seul une LED s'allume à la fois. Modifiez la fonction pour que les LED déjà allumées ne s'éteignent pas. Un indice: l'opérateur bitwise `|` est utile ici.
<!--
{{% expand "Solution" %}}
```python
def allumer(mod,rg,col):
    adr = rg << 1
    octet = 1 << col

    mod.write(bytes([adr]))
    donnees = bytearray(1)
    module.readinto(donnees)
    
    octet = donnees[0] | octet
    mod.write(bytes([adr]))
    module.write(bytes([adr,octet]))
```
{{% /expand %}}
-->
