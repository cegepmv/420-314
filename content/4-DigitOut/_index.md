+++
title = 'Sortie digital'
date = 2026-06-12T17:15:00-04:00
draft = false
weight = 4
pre = "4. "
+++

Sur un *ESP32*, les composantes électroniques peuvent être utilisées de deux manières. L'*ESP32* peut y envoyer un signal, comme avec une LED ou un *buzzer*, mais il peut aussi recevoir un signal, comme avec un bouton, un détecteur de température ou de luminosité, etc.

Les composantes "input", qui envoient un signal vers l'*ESP32*, sont nommées ***senseurs***.

Les composantes "output", qui reçoivent un signal de l'*ESP32*, sont généralement nommées ***actuateurs***.

Dans cette section, nous verrons comment utiliser le GPIO pour **envoyer** un signal (en _output_) à des composantes.

## Composantes électroniques

![Image pins](/420-314/images/pins-esp32.png)
*Figure 1 : Plan de correspondance des broches de l’Arduino Nano ESP32. Source : [Arduino Docs](https://docs.arduino.cc/tutorials/nano-esp32/pin-setup/) (Consulté en 2026).*

GPIO signifie "General Purpose Input/Output" et désigne la série de broches métalliques qui servent à envoyer ou recevoir des signaux électriques sur un microcontrôleur.

L'Arduino Nano ESP32 en comprend 30 (réparties sur deux rangées) :
+ 1 broche de sortie pour un courant de 3.3V
+ 1 broche d'entrée/sortie pour la tension brute d'alimentation (VBUS)
+ 2 broches pour le courant négatif ("ground" / GND)
+ 22 broches génériques d'E/S (compatibles 3.3V, courant max ~40mA par broche)

{{% notice style="warning" title="Correspondance des broches" %}}
Contrairement à d'autres cartes, l'Arduino Nano ESP32 a deux numérotations. Les étiquettes imprimées sur la carte (ex. : `D2`, `A0`) correspondent au standard Arduino. En **MicroPython**, `machine.Pin.board` permet de désigner les broches par ces noms de carte; les numéros GPIO natifs du processeur ESP32-S3 sont différents. Par exemple, la broche notée `D2` correspond au `GPIO 5`.
{{% /notice %}}

En *MicroPython*, on peut utiliser le nom de la broche imprimé sur la carte avec `machine.Pin.board`. Par exemple, `machine.Pin.board.D2` désigne la broche `D2` de la carte :

```python
import machine
import time

# Configuration de la broche D2 en mode SORTIE
pin = machine.Pin(machine.Pin.board.D2, machine.Pin.OUT)

# On envoie un signal (3.3V) sur D2 durant 1 seconde
pin.value(1)
time.sleep(1)

# On coupe le signal (0V)
pin.value(0)

```

Dans ce programme, `machine.Pin.board.D2` désigne la broche `D2` de la carte, et la bibliothèque native `machine` permet d'interagir avec le matériel de l'ESP32.

{{% notice style="info" title="Astuce MicroPython" %}}
Utilisez `machine.Pin.board` pour désigner directement une broche par le nom imprimé sur la carte. Par exemple :
```python
import machine

pin = machine.Pin(machine.Pin.board.D2, machine.Pin.OUT)
```
{{% /notice %}}

{{% notice style="green" title="Référence" %}}

* Guide de configuration des broches (Pin Setup) : [Arduino Docs](https://docs.arduino.cc/tutorials/nano-esp32/pin-setup/)
* Documentation officielle de la bibliothèque `machine` : [MicroPython Docs](https://docs.micropython.org/en/latest/library/machine.html)
{{% /notice %}}

L'ESP32 est alimenté en 5V (via le port USB), mais ses broches de données (GPIO) fonctionnent exclusivement en **3.3V**. Les broches d'alimentation `3V3` fournissent un courant continu de 3.3V, et la broche `5V` (VBUS) fournit du 5V.

Le signal envoyé par l'ESP32 sur les broches GPIO est un courant électrique de 3.3V. Cela signifie qu'on peut utiliser ces broches pour contrôler un signal électrique lorsqu'on veut utiliser l'ESP32 avec des composantes électroniques ordinaires.

Dans ce qui suit, nous allons alimenter une LED simple en utilisant le GPIO dans un programme MicroPython.

Connectez votre *ESP32* à la plaquette de prototypage (breadboard) comme suit :
+ Broche **GND** (Ground) au rail négatif de la plaquette.
+ Broche **G4** (GPIO 4) à une rangée libre, et non au rail positif.

Branchez ensuite en série la résistance et la LED entre **G4** et **GND** (anode de la LED du côté de G4). Choisissez une résistance à partir de la tension directe de la LED et d'un courant qui respecte les limites recommandées du GPIO : $R = (3{,}3\text{ V} - V_f) / I$.

Posez ensuite une LED et une résistance sur la plaquette, puis lancez le programme de l'exemple suivant.

```python
from machine import Pin
import time

# Initialisation de la broche GPIO 4 en mode SORTIE
led = Pin(4, Pin.OUT)

# Allumer la LED
led.value(1)
time.sleep(1)

# Éteindre la LED
led.value(0)
time.sleep(1)

```

La ligne `from machine import Pin` sert à importer le module natif de MicroPython qui permet de contrôler les broches matérielles.

La ligne `led = Pin(4, Pin.OUT)` configure la broche `GPIO 4` pour *envoyer* un signal (les broches GPIO peuvent aussi *recevoir* des signaux électriques).

La ligne `led.value(1)` permet d'envoyer un signal haut (3.3V) au `GPIO 4`. Avec l'argument `0`, le programme coupe le signal (0V). *Note : On peut aussi utiliser `led.on()` et `led.off()`.*

{{% notice style="green" title="Astuce MicroPython" %}}
Pour que ce code s'exécute automatiquement dès que l'ESP32 est mis sous tension (sans être connecté à l'ordinateur), vous devez enregistrer votre script dans le microcontrôleur sous le nom de **`main.py`**.
{{% /notice %}}

## Modules Keyestudio

Lorsque c'est possible, il est préférable d'utiliser les modules Keyestudio plutôt que les composantes électroniques de base, car les risques de surcharge ou de court-circuits sont plus faibles.

Dans l'exemple suivant, nous allons connecter un module LED sur l'ESP32 et le contrôler par un programme.

Vous aurez besoin du module "LED" Keyestudio suivant :

Connectez le module LED à l'ESP32 comme suit :

* Broche `V` sur la broche `3V3` (ou 5V)
* Broche `G` sur la broche `GND` (Ground)
* Broche `S` sur la broche `G4` (GPIO 4)

Lancez une nouvelle fois le programme de l'exemple précédent.

{{% notice warning "Remarque" %}}
Avec une LED ordinaire, c'est en envoyant un courant positif qu'on l'allume. Dans le cas des modules Keyestudio, il faut toujours les alimenter en continu via `V` et `G`, et c'est le signal envoyé sur la broche `S` (contrôlé par le GPIO) qui agit comme un interrupteur logique.
{{% /notice %}}

## Exercices
1. Faire un programme qui demande à l'utilisateur d'entrer un nombre entier et clignote une LED à toutes les secondes le nombre de fois que l'utilisateur a indiqué.
1. Faire un programme qui fait clignoter la LED Keyestudio à chaque seconde (en boucle infinie).
1. Connectez une LED RGB sur une autre broche GPIO (ex: GPIO 5) et faites un programme qui fait clignoter les deux LED en alternance (lorsqu'une est éteinte, l'autre est allumée). La LED RGB doit être jaune.

{{% expand "Solution 1." %}}

```python
from machine import Pin
import time

led = Pin(4, Pin.OUT)
led.off()

while True:
    try:
        nombre = int(input("Combien de fois faire clignoter la LED ? "))
        if nombre < 0:
            print("Entrez un nombre positif ou nul.")
            continue
        break
    except ValueError:
        print("Entrez un nombre entier.")

for _ in range(nombre):
    led.on()
    time.sleep(1)
    led.off()
    time.sleep(1)
```

{{% /expand %}}

{{% expand "Solution 2." %}}

```python
from machine import Pin
import time

led = Pin(4, Pin.OUT)

while True:
    led.value(1)
    time.sleep(1)
    led.value(0)
    time.sleep(1)

```

{{% /expand %}}

{{% expand "Solution 3." %}}

```python

import machine
import time
# La broche 2 contrôle souvent la LED bleue intégrée
led = machine.Pin(machine.Pin.board.D6, machine.Pin.OUT)
R =  machine.Pin(machine.Pin.board.D3, machine.Pin.OUT)
G =  machine.Pin(machine.Pin.board.D4, machine.Pin.OUT)
B =  machine.Pin(machine.Pin.board.D2, machine.Pin.OUT)

R.value(1)
G.value(1)
B.value(1)
led.value(0)



while True:
    led.value(1)
    R.value(1)
    G.value(1)
    time.sleep(1) 

    led.value(0)
    R.value(0)
    G.value(0)   
    time.sleep(1) 

```

{{% /expand %}}
