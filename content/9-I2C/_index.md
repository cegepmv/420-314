+++
title = 'I2C'
date = 2026-06-14T11:32:05-04:00
draft = false
weight = 9
pre = "9. "
+++

---
```python
print("Scan du bus I2C en cours...")
appareils = i2c.scan()

if len(appareils) == 0:
    print("Aucun appareil I2C détecté ! Vérifiez le câblage et l'alimentation.")
else:
    print(f"Trouvé : {len(appareils)} appareil(s) I2C")
    for adresse in appareils:
        print(f"- Adresse décimale : {adresse} | Adresse Hexadécimale : {hex(adresse)}")
```
## Le protocole I2C sur Arduino Nano ESP32

Le fonctionnement du protocole I2C s'appuie sur des principes qu'on retrouve dans les communications numériques "série".

#### Horloge et données

Il se base sur 2 signaux : **SCL** (l'horloge ou *Clock*) et **SDA** (les données ou *Data*). L'horloge sert à déterminer la fréquence à laquelle les données sont envoyées, et les données sont des valeurs de 1 bit envoyées à chaque période de l'horloge.

Sur l'**Arduino Nano ESP32**, les broches I2C par défaut sont :

* **SCL** : Broche A5
* **SDA** : Broche A4

Un exemple simple : supposons que vous vouliez transmettre la lettre "a" à l'aide de l'I2C. Cette lettre correspond à l'octet `01100001` en ASCII, où chaque bit sera une impulsion électrique envoyée à chaque période de la fréquence définie par l'horloge.

#### Adressage et Scan en MicroPython

Un contrôleur (*Master* ou *Main*) peut envoyer et recevoir des signaux de plusieurs périphériques (*Target* ou *Client*). Il peut y avoir jusqu'à 127 périphériques sur le même bus. Pour s'y retrouver, chaque périphérique possède une adresse unique (souvent écrite en hexadécimal).

Avec le Raspberry Pi, on utilisait la commande système `i2cdetect -y 1`. Avec MicroPython, on utilise l'objet `I2C` de la bibliothèque `machine` pour scanner le bus directement dans le code :

```python
from machine import Pin, I2C

# Initialisation du bus I2C par défaut sur l'Arduino Nano ESP32
i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))

# Scan du bus pour trouver les adresses des périphériques connectés
peripheriques = i2c.scan()

print("Périphériques I2C trouvés :")
for adresse in peripheriques:
    print(f"Adresse décimale : {adresse} | Adresse hexadécimale : {hex(adresse)}")

```

Si un composant est connecté (par exemple un capteur à l'adresse `0x2a`), le script affichera :

```text
Périphériques I2C trouvés :
Adresse décimale : 42 | Adresse hexadécimale : 0x2a

```

---

## Opérations "bitwise" (bit à bit)

Le protocole I2C requiert souvent qu'on se représente les données sous forme de bits et d'octets. Par exemple, pour activer une fonctionnalité sur un composant, on doit parfois envoyer une séquence de bits bien précise à un registre donné. Il est donc indispensable de comprendre le fonctionnement des opérateurs *bitwise*, qui agissent directement sur les bits individuels des nombres.

Il y a 4 opérateurs principaux en Python / MicroPython :

* `&` : ET logique
* `|` : OU logique
* `^` : OU exclusif (XOR)
* `<<` et `>>` : Décalages de bits

#### ET logique (`&`)

Le ET logique compare **chacun des bits** correspondants de deux nombres. Le résultat vaut 1 si et seulement si les deux bits valent 1 :

* 1 ET 1 = 1
* 1 ET 0 = 0
* 0 ET 1 = 0
* 0 ET 0 = 0

Dans l'exemple suivant, 235 ET 173 = 169 :

```text
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
1 0 1 0 1 0 0 1  (169)

```

En MicroPython :

```python
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 & n2
169
>>> bin(n1 & n2)
'0b10101001'

```

#### OU logique (`|`)

Le OU logique compare chacun des bits. Le résultat vaut 1 lorsqu'**au moins un** des deux bits vaut 1 :

* 1 OU 1 = 1
* 1 OU 0 = 1
* 0 OU 1 = 1
* 0 OU 0 = 0

Par exemple, 235 OU 173 = 239 :

```text
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
1 1 1 0 1 1 1 1  (239)

```

En MicroPython :

```python
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 | n2
239
>>> bin(n1 | n2)
'0b11101111'

```

#### OU exclusif (`^` ou XOR)

Pour le OU exclusif, les deux bits correspondants doivent être **différents** pour que le résultat soit 1 :

* 1 XOR 1 = 0
* 1 XOR 0 = 1
* 0 XOR 1 = 1
* 0 XOR 0 = 0

Dans l'exemple suivant, 235 XOR 173 = 70 :

```text
1 1 1 0 1 0 1 1  (235)
1 0 1 0 1 1 0 1  (173)
---------------
0 1 0 0 0 1 1 0  (70)

```

En MicroPython :

```python
>>> n1 = 0b11101011
>>> n2 = 0b10101101
>>> n1 ^ n2
70
>>> bin(n1 ^ n2)
'0b1000110'

```

#### Décalage (*Bit Shift*)

Les opérations de décalage consistent à "glisser" la séquence de bits vers la gauche ou la droite.

* **À gauche (`<<`)** : On ajoute des `0` à droite (ce qui revient à multiplier par 2 pour chaque décalage).
* **À droite (`>>`)** : On supprime les bits de droite (ce qui revient à faire une division entière par 2).

Décalage de 1 vers la gauche du nombre 15 (résultat = 30) :

```text
01111 (15)
11110 (30)

```

```python
>>> 15 << 1
30

```

Décalage de 1 vers la droite du nombre 54 (résultat = 27) :

```text
110110 (54)
011011 (27)

```

```python
>>> 54 >> 1
27

```

> **Astuce mathématique :**
> À chaque décalage vers la gauche de $n$ bits, on multiplie par $2^n$ :
> `10 << 3` $\rightarrow 10 \times 2^3 = 80$
> Inversement, à chaque décalage à droite de $n$ bits, on divise par $2^n$ :
> `67 >> 3` $\rightarrow 67 // 2^3 = 8$

---

## Exemple concret en MicroPython

Imaginons une caméra I2C dont la configuration se fait sur 3 bits :

* **bit 0** : Filtre (1 = activé, 0 = désactivé)
* **bit 1** : Haute Définition (1 = activé, 0 = désactivé)
* **bit 2** : Couleur (1 = couleur, 0 = noir & blanc)

*Rappel : On compte les bits de droite à gauche (le bit 0 étant le bit de poids faible).*

Si on veut activer la **Couleur** et le **Filtre**, mais pas la HD, on veut obtenir la séquence `101`.

Voici comment nous écririons la fonction de configuration en **MicroPython**, en utilisant la méthode `writeto_mem()` de l'objet I2C (qui permet d'écrire une valeur à une adresse de registre spécifique) :

```python
from machine import Pin, I2C

# Configuration du bus
i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
ADRESSE_CAMERA = 0x20  # Adresse du composant I2C
REGISTRE_CONFIG = 0x00 # Supposons que le registre de config soit 0x00

def configCamera(onFilt, onHDef, onCoul):
    config = 0b000
    
    # Positionnement des bits au bon endroit
    config = config | (int(onFilt) << 0) # Bit 0
    config = config | (int(onHDef) << 1) # Bit 1
    config = config | (int(onCoul) << 2) # Bit 2
    
    # En MicroPython, on doit envoyer les données sous forme d'objet 'bytes' ou 'bytearray'
    donnees = bytes([config])
    
    # Écriture : i2c.writeto_mem(adresse_peripherique, adresse_registre, donnees)
    i2c.writeto_mem(ADRESSE_CAMERA, REGISTRE_CONFIG, donnees)
    print(f"Configuration envoyée : {bin(config)}")

# Test de la fonction : Filtre=True, HD=False, Couleur=True
configCamera(True, False, True)

```

### Pourquoi `bytes([config])` ?

Contrairement au Raspberry Pi où certaines bibliothèques acceptent des entiers bruts, l'API `machine.I2C` de MicroPython exige une structure de type tableau d'octets (`bytes` ou `bytearray`) pour envoyer des données, même s'il n'y a qu'un seul octet à transmettre !


Voici la réécriture complète de votre cours sur la matrice LED 8x8.

Le plus gros changement ici concerne la transition de **CircuitPython** (`board`, `busio`, `adafruit_bus_device`) vers le **MicroPython natif** standard de l'Arduino Nano ESP32. En MicroPython, on utilise directement la classe `machine.I2C`, ce qui rend le code beaucoup plus léger et évite d'avoir à installer de lourdes bibliothèques tierces, tout en conservant exactement la même logique de communication (méthodes `writeto` et `readfrom_mem`).


# MAtrice LED 8x8

La matrice de LED fournie dans le kit KS0522 utilise le protocole de communication I2C. Puisqu'elle agit uniquement en tant que périphérique de sortie (*output*) et n'envoie pas de signal analogique à l'Arduino, aucun convertisseur ADC n'est requis. Elle est équipée d'une puce **HT16K33**, conçue spécialement pour contrôler l'affichage de matrices de LED en gérant le multiplexage à notre place.

Dans cette section, nous verrons comment la contrôler directement en MicroPython.

Sur l'**Arduino Nano ESP32**, vous avez deux options pour brancher les broches I2C (**SDA** et **SCL**), selon la façon dont vous préférez repérer vos broches.

Voici où les connecter physiquement :

### Option 1 : Les broches analogiques classiques (Recommandé)

C'est la configuration standard que nous avons utilisée dans les exemples de code MicroPython :

* 🔌 **SDA** ➔ Se branche sur la broche **A4**
* 🔌 **SCL** ➔ Se branche sur la broche **A5**

### Option 2 : Les broches numériques dédiées

Si vous regardez le dessus de votre carte Arduino Nano ESP32, vous verrez des petites inscriptions directement imprimées sur le circuit imprimé (du côté des broches D11 / D12) :

* 🔌 **SDA** ➔ Se branche sur la broche marquée **SDA** (physiquement positionnée entre la broche *GND* et la broche *D12*)
* 🔌 **SCL** ➔ Se branche sur la broche marquée **SCL** (physiquement positionnée au bout de la rangée, juste après la broche *SDA*)

---

### N'oubliez pas l'alimentation !

Pour que votre matrice de LED ou votre périphérique fonctionne, il faut aussi brancher les deux broches d'alimentation :

* **VCC** ou **V+** du module ➔ Se branche sur la broche **3.3V** de l'Arduino.
* **GND** du module ➔ Se branche sur une broche **GND** (masse) de l'Arduino.


---

## Communication I2C native en MicroPython

Sur le Raspberry Pi, nous utilisions la bibliothèque d'Adafruit. Sur l'Arduino Nano ESP32 en MicroPython, nous utilisons directement le module natif `machine`. L'équivalent de l'initialisation et des méthodes `write` et `readinto` se fait très simplement grâce aux fonctions intégrées du bus I2C.

Nous utiliserons principalement ces deux méthodes de l'objet `I2C` :

###### writeto(adresse, buf)

Envoie les données contenues dans le tampon `buf` (qui doit être de type `bytes` ou `bytearray`) au composant I2C spécifié par son `adresse`.

###### readfrom_mem(adresse, registre, nbytes)

Lit un nombre `nbytes` d'octets à partir d'un `registre` spécifique du composant I2C, et retourne un objet `bytes`.

---

### Exemple 1 : Initialisation et affichage de lignes

Dans le programme suivant, on initialise la puce HT16K33 puis on affiche un motif sur chaque rangée successivement :

```python
from machine import Pin, I2C
from time import sleep

# Initialisation du bus I2C sur l'Arduino Nano ESP32 (Broches A5/A4 par défaut)
i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
adresse_matrice = 0x70  # Adresse I2C standard de la matrice

# Initialisation du contrôleur HT16K33
i2c.writeto(adresse_matrice, bytes([0x21])) # Activer l'oscillateur interne
i2c.writeto(adresse_matrice, bytes([0x81])) # Activer l'affichage (sans clignotement)
i2c.writeto(adresse_matrice, bytes([0xEF])) # Régler la luminosité au maximum (0xE0 à 0xEF)

octetAffiche = 0b10100101 # Équivalent à 165 en décimal

# On parcourt les adresses des rangées (de 0 à 14, de 2 en 2)
for i in range(0, 16, 2):
    # Envoi de [adresse_registre, valeur_octet]
    i2c.writeto(adresse_matrice, bytes([i, octetAffiche]))
    sleep(0.5)
    # On éteint la rangée
    i2c.writeto(adresse_matrice, bytes([i, 0x00]))

```

> **Représentation binaire :** En Python, précéder un nombre par `0b` permet de l'écrire directement en binaire. C'est beaucoup plus visuel pour dessiner sur une matrice !

Chaque rangée de la matrice est représentée par un octet (8 bits). Chaque bit correspond à une LED : `1` pour allumée, `0` pour éteinte. Par exemple, l'octet `0b10000001` (129) allumera la première et la dernière LED d'une rangée.

#### Cartographie de la mémoire (Memory Map)

Bien que notre matrice physique soit une 8x8 (64 LED), la puce HT16K33 est conçue pour gérer des matrices allant jusqu'à 16x16. Par conséquent, l'adressage saute une case sur deux. Les lignes paires (`0x00`, `0x02`, `0x04`, etc.) contrôlent nos 8 rangées, tandis que les lignes impaires sont ignorées :

---

### Exemple 2 : Animation bit à bit

Ici, on utilise le décalage de bits pour allumer chaque LED de la première rangée (adresse `0x00`), l'une après l'autre :

```python
from machine import Pin, I2C
from time import sleep

i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
adresse_matrice = 0x70

# Initialisation
i2c.writeto(adresse_matrice, bytes([0x21])) 
i2c.writeto(adresse_matrice, bytes([0x81])) 
i2c.writeto(adresse_matrice, bytes([0xEF])) 

# On décale le bit 1 vers la gauche de 0 à 7 fois
for decalage in range(8):
    octet = 1 << decalage # Génère successivement: 1, 2, 4, 8, 16, 32, 64, 128
    
    i2c.writeto(adresse_matrice, bytes([0x00, octet]))
    sleep(0.3)

i2c.writeto(adresse_matrice, bytes([0x00, 0x00]))

```

---

### Exemple 3 : Lire l'état de la matrice

MicroPython simplifie grandement la lecture de registres grâce à la méthode `readfrom_mem()`. Plus besoin de faire un `write` manuel de l'adresse avant de lire !

```python
from machine import Pin, I2C

i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
adresse_matrice = 0x70

# Écriture d'une valeur test à la rangée 0 (0x00)
octet_test = 45
i2c.writeto(adresse_matrice, bytes([0x00, octet_test])) 

# Lecture directe du registre 0x00 (on demande 1 octet)
donnees = i2c.readfrom_mem(adresse_matrice, 0x00, 1)

print("Valeur lue (décimal) :", donnees[0])
print("Valeur lue (binaire) :", bin(donnees[0]))

```

---

## Exercices (Adaptés pour MicroPython)

1. Faites une fonction nommée `allumerTout(i2c_bus)` prenant l'instance du bus I2C en argument qui allume toutes les LED de la matrice.
{{% expand "Solution" %}}

```python
def allumerTout(bus):
    for i in range(0, 16, 2):
        bus.writeto(0x70, bytes([i, 255]))

```

{{% /expand %}}

2. Faites une fonction nommée `remplir(i2c_bus, allume)` qui allume toutes les LED si `allume` (un booléen) est Vrai, et les éteint si `allume` est Faux.
{{% expand "Solution" %}}

```python
def remplir(bus, on):
    valeur = 255 if on else 0
    for i in range(0, 16, 2):
        bus.writeto(0x70, bytes([i, valeur]))

```

{{% /expand %}}

3. Faites un programme semblable à celui de l'exemple 2, mais qui allume la première LED de chaque rangée (au lieu de chaque LED d'une même rangée).
{{% expand "Solution" %}}

```python
# (...) Partie initialisation I2C inchangée (...)
for addr in range(0, 16, 2):
    i2c.writeto(0x70, bytes([addr, 128])) # 128 = 0b10000000 (première LED)
    sleep(0.5)
    i2c.writeto(0x70, bytes([addr, 0]))

```

{{% /expand %}}

4. Faites une fonction nommée `rangee(i2c_bus, ligne)` qui allume toutes les LED de la ligne passée (un entier de 0 à 7). Utilisez l'opérateur `<<` pour convertir ce numéro de ligne en adresse réelle de registre (0, 2, 4, ..., 14).
{{% expand "Solution" %}}

```python
def rangee(bus, ligne):
    reg = ligne << 1  # Multiplie par 2 pour obtenir l'adresse paire
    bus.writeto(0x70, bytes([reg, 255]))

```

{{% /expand %}}

5. Faites une fonction nommée `allumerUneLed(i2c_bus, ligne, colonne)` qui allume une unique LED aux coordonnées transmises (entiers de 0 à 7). Utilisez les opérateurs de décalage pour calculer le registre de la ligne et déterminer l'octet de la colonne.
{{% expand "Solution" %}}

```python
def allumerUneLed(bus, ligne, colonne):
    reg = ligne << 1
    octet = 128 >> colonne  # Décale le bit de poids fort vers la droite
    bus.writeto(0x70, bytes([reg, octet]))

```

{{% /expand %}}

6. Faites un programme qui allume un nombre aléatoire de LED sur toutes les rangées (avec `random.randint()`), puis lisez et affichez à la console la valeur binaire stockée dans chaque registre de la matrice.
{{% expand "Solution" %}}

```python
from machine import Pin, I2C
from random import randint
from time import sleep

i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
# (Ajouter l'initialisation de l'oscillateur 0x21, 0x81, 0xEF ici...)

# Génération et écriture de valeurs aléatoires
for ligne in range(8):
    reg = ligne << 1
    octet_aleatoire = randint(0, 255)
    i2c.writeto(0x70, bytes([reg, octet_aleatoire]))

# Lecture et affichage des registres
for ligne in range(8):
    reg = ligne << 1
    donnees = i2c.readfrom_mem(0x70, reg, 1)
    print(f"Registre {hex(reg)} | Valeur : {bin(donnees[0])}")

```

{{% /expand %}}

7. Si on appelle plusieurs fois la fonction `allumerUneLed()` de l'exercice 5 pour la même rangée, la LED précédente s'éteint. Modifiez la fonction pour préserver les LED déjà allumées. *Indice : Il faut d'abord lire l'état actuel de la ligne avec `readfrom_mem`, puis combiner l'ancien état avec la nouvelle LED via l'opérateur `|`.*
{{% expand "Solution" %}}

```python
def allumerUneLedPersistant(bus, ligne, colonne):
    reg = ligne << 1
    nouvelle_led = 128 >> colonne
    
    # 1. Lire l'état actuel du registre
    etat_actuel = bus.readfrom_mem(0x70, reg, 1)[0]
    
    # 2. Fusionner l'ancien état et le nouveau bit avec le OU logique
    nouvel_etat = etat_actuel | nouvelle_led
    
    # 3. Réécrire la nouvelle configuration globale de la ligne
    bus.writeto(0x70, bytes([reg, nouvel_etat]))

```

{{% /expand %}}