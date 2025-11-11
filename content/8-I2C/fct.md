+++
title = 'Fonctionnement'
date = 2024-11-21T11:28:38-05:00
draft = true
weight = 81
+++

Le fonctionnement du protocole I2C s'appuie sur des principes qu'on retrouve dans les communications digitales "sérielles".

#### Horloge et données
Il se base sur 2 signaux: SCL (l'horloge ou "clock") et SDA (les données ou "data"). L'horloge sert à déterminer la fréquence à laquelle des données sont envoyées, et les données sont des valeurs de 1 bit envoyées à chaque période de l'horloge.

Un exemple simple: supposons que vous voulez transmettre la lettre "a" à l'aide de I2C. Cette lettre correspond à l'octet `01100001` en ASCII, où chaque bit sera une impulsion électrique envoyée à chaque période de la fréquence définie par l'horloge :

![sdascl](/420-314/images/sdascl.png)

#### Adressage
Un contrôleur ("master") peut envoyer et recevoir des signaux de plusieurs périphériques ("slave"); il peut y avoir jusqu'à 127 périphériques. Pour s'y retrouver, chaque périphérique se fait attribuer une adresse.

La commande `i2cdetect -y 1` permet de voir les adresses des périphériques connectés:

```
info@pi:~ $ i2cdetect -y 0

     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- 2a -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- 48 -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- -- 
```

## Opérations "bitwise"
Le protocole I2C requiert souvent qu'on se représente les données sous forme de bits et d'octets. Par exemple, on pourrait avoir des composants où, pour activer une fonctionnalité particulière, on doit envoyer la séquence de 4 bits `0101` (5) à l'adresse `0x4a` (74). Dans ce contexte, il est important de savoir se représenter les données numériques au format binaire, et aussi de comprendre le fonctionnement des opérateurs *bitwise*, qui servent à effectuer des opérations sur les bits individuels des nombres binaires. 

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
- 1 ET 1 = 1
- 1 ET 0 = 1
- 0 ET 1 = 1
- 0 ET 0 = 0

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
- 1 ET 1 = 0
- 1 ET 0 = 1
- 0 ET 1 = 1
- 0 ET 0 = 0

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




#### Exemple d'utilisation
Imaginons une composante électronique, par exemple une caméra dont les spécifications techniques décrivent trois paramètres de configuration possibles: un mode couleur ou noir et blanc, un mode haute définition et un filtre pour augmenter le contraste. Ces paramètres correspondent chacun à 1 bit dans une séquence de 3 bits :
+ bit 1: Filtre activé / filtre désactivé
+ bit 2: Haute définition / basse définition
+ bit 3: Couleur / noir et blanc

> Attention: On les ordonne de droite à gauche.

Donc, la valeur de cette séquence correspond à une combinaison des paramètres de configuration:
+ `101`: Couleur, basse définition, filtre activé
+ `010`: Noir et blanc, haute définition, filtre désactivé
+ `001`: Noir et blanc, basse définition, filtre activé
+ etc.

Cette valeur doit être écrite à l'adresse mémoire `0x20`; ensuite, lorsque les données seront lues, l'image aura les propriétés définies par la configuration.

Imaginons qu'on a une fonction pour configurer notre caméra dont les paramètres sont des valeurs booléennes:
```python
configCamera(oCamera,activerFiltre=True, activerHauteDef=True, activerCouleur=True)
```
Cette fonction peut être définie comme suit:
```python
def configCamera(oCam,onFilt,onHDef,onCoul):
    config = 0b000
    config = config | (onFilt << 0) # Premier bit : filtre
    config = config | (onHDef << 1) # Deuxième bit : HD
    config = config | (onCoul << 2) # troisième bit : couleur
    oCam.write(0x20, config)
```
Puisque les valeurs **True** ou **False** en python correspondent respectivement à `0b1` et `0b0`, le décalage place cette valeur au bon endroit dans la séquence. Par exemple, si `onHDef = True`, le décalage donne `010`; si `onHDef = False`, le décalage donne `000`. Cette opération permet de représenter les trois valeurs booléennes en une séquence de 3 bits.

Ensuite, pour appliquer ces valeurs, on doit les combiner dans une seule séquence. Par exemple si on veut que le filtre (`001`) et que la couleur (`100`) soient activés, on devra envoyer `101` à la caméra. L'opérateur OU logique permet de faire cette combinaison. En effet si la valeur initiale de *config* est de 0:

+ `config | (onFilt << 0)` vaut `000 | 001` = 001
+ `config | (onHDef << 1)` vaut `001 | 000` = 001
+ `config | (onCoul << 2)` vaut `001 | 100` = 101 

Ce qui permet d'écrire la valeur qui correspond à la configuration souhaitée à l'adresse `0x20`.
