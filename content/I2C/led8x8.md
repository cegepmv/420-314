+++
title = 'Matrice LED 8x8'
date = 2024-11-21T11:28:46-05:00
draft = false
weight = 73
+++

La matrice de LED founie dans le kit KS0522 utilise le protocole de communication I2C, mais puisque celle-ci est en "output" et donc n'envoie pas de signal analogique au RaspberryPi, on n'a pas besoin du convertisseur ADC pour l'utiliser. Elle est équipée d'une puce HT16K33, conçue spécialement pour contrôler l'affichage dans des matrices de LED de différentes dimensions. 

À partir de RaspberryPi il existe deux manières de lui envoyer des données: 
+ Le module `adafruit_ht16k33`
+ Le module `adafruit_bus_device`, plus générique

Dans cette section nous verrons les deux méthodes.

## adafruit_ht16k33

Ce module définit de nombreuses classes permettant de contrôler facilement divers types de matrices de LED. Il permet d'éviter d'avoir à gérer les adresses mémoire de chacune des LED séparément.

Pour l'uitiliser, il faut d'abord installer le module `ht16k33` comme suit:
```bash
pip3 install adafruit-circuitpython-ht16k33
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



## adafruit_bus_device

On utilisera la classe `I2CDevice` pour communiquer avec la matrice de LED. Les méthodes ici sont plus génériques que celles de la section précédente car elles permettent de contrôler n'importe quel senseur ou actuateur utilisant I2C. Nous utiliserons les deux suivantes:

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



