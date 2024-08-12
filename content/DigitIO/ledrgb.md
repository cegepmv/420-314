+++
title = 'LED RGB'
date = 2024-08-11T18:31:43-04:00
draft = false
weight = 34
+++

Les LED RGB ("Light Emitting Diode Red, Green, Blue") sont des diodes électroluminescentes qui peuvent émettre de la lumière dans différentes couleurs en mélangeant la lumière rouge, verte et bleue à des intensités variables. 

Dans n'importe quel type de diode, on nomme **anode** la broche *positive* et **cathode** la broche *négative*. Par exemple pour une LED simple:

![ledpoles](/420-314/images/ledpoles.png?width=300px)

Dans le cas d'une LED RGB, les choses sont un peu différentes. Le fait qu'elles aient une source d'alimentation pour 3 couleurs permet de regrouper des broches.

Les LED RGB à **anode commune** et à **cathode commune** sont deux configurations différentes pour ce type de LED; elles diffèrent par la manière dont elles sont connectées électriquement:

![rgbtypes](/420-314/images/rgbtypes.png?width=400px)

## Cathode commune
Dans une *LED RGB à cathode commune*, les cathodes (-) de chaque LED interne sont connectées ensemble et sortent sous forme d'une seule broche commune (la LED RGB dans *TinkerCAD* est de ce type).

Pour allumer une couleur spécifique sur une LED RGB à cathode commune, il faut connecter la broche de cette couleur à un courant positif et connecter la cathode commune au courant négatif (*ground*).

## Anode commune
Dans une *LED RGB à anode commune*, les anodes (+) de chaque LED interne (rouge, verte et bleue) sont connectées ensemble et sortent sous forme d'une seule broche commune.

Pour allumer une couleur spécifique sur une LED RGB à anode commune, il faut connecter l'anode commune à un courant positif et connecter la ou les broches de la couleur souhaitée au courant négatif (*ground*).

> La LED RGB du module Keystudio KS0522 est une LED à anode commune. Il faut donc envoyer un courant positif sur l'anode et les broches qu'on veut garder éteintes, et un courant négatif sur les broches qu'on veut allumer.

## Matériel requis

| | |
|:--|--|
| Module "RGB LED" | ![ksrgbled](/420-314/images/ksrgbled.png?width=150px) |
| 4 connecteurs F-F | ![3jumpff](/420-314/images/3jumpff.png?width=150px) |

## Connexions

Dans cet exemple nous utiliserons les broches suivantes du *RaspberryPi*:
+ broche 17 (courant 3,3V)
+ broche 11 (GPIO 17, rouge)
+ broche 13 (GPIO 27, vert)
+ broche 15 (GPIO 22, bleu)
  
## Programme `rgb1.py`

Le programme suivant aura pour effet d'allumer la LED selon le cycle suivant: 1 seconde en rouge, 1 seconde en vert et 1 seconde en bleu.

```python
import pigpio
import time

R,G,B=17,27,22

pi = pigpio.pi()
pi.set_mode(R,pigpio.OUTPUT)
pi.set_mode(G,pigpio.OUTPUT)
pi.set_mode(B,pigpio.OUTPUT)

# Allumer R, éteindre G et B
pi.write(R,0)
pi.write(G,1)
pi.write(B,1)
time.sleep(1)

# Allumer G, éteindre R et B
pi.write(R,1)
pi.write(G,0)
pi.write(B,1)
time.sleep(1)

# Allumer B, éteindre R et G
pi.write(R,1)
pi.write(G,1)
pi.write(B,0)
time.sleep(1)

# Éteindre tout
pi.write(R,1)
pi.write(G,1)
pi.write(B,1)
```

#### Exercices
1. Faites un programme qui allume la LED en rouge lorsqu'on appuie le bouton et l'éteint lorsqu'on le relâche.
2. Faites un programme qui allume la LED en rouge lorsqu'on clique une fois et l'éteint lorsqu'on clique une deuxième fois.
3. Faites un programme qui cycle entre les 3 couleurs rouge, vert et bleu, où la couleur change à chaque clic.
4. Si on mélange les couleurs R, G et B, on obtient ces couleurs:
   + R + G = jaune
   + R + B = violet
   + G + B = cyan
   + R + G + B = blanc
   
Faites un programme similaire au précédent, mais qui cycle entre rouge, jaune, vert, cyan, bleu, violet, blanc.


{{% expand "Solution 1." %}}
```python
import pigpio

R,G,B = 17,27,22
btn = 23

def allumer():
    pi.write(R,0)
    pi.write(G,1)
    pi.write(B,1)

def eteindre():
    for pin in [R,G,B]: pi.write(pin,1)

pi = pigpio.pi()

pi.set_mode(btn,pigpio.INPUT)
pi.set_mode(R,pigpio.OUTPUT)
pi.set_mode(G,pigpio.OUTPUT)
pi.set_mode(B,pigpio.OUTPUT)

dernier = 1 
while True:
    signal = pi.read(btn)
    if signal != dernier:
        if signal == 0: 
            allumer()
        else: eteindre()
        
    dernier = signal
```
{{% /expand %}}
{{% expand "Solution 2." %}}
```python
import pigpio
import time

R,G,B = 17,27,22
btn = 23

def allumer():
    pi.write(R,0)
    pi.write(G,1)
    pi.write(B,1)

def eteindre():
    for pin in [R,G,B]: pi.write(pin,1)

def changerEtat(e):
    if e == "off":
        allumer()
        return "on"
    else:
        eteindre()
        return "off"

pi = pigpio.pi()

pi.set_mode(btn,pigpio.INPUT)
pi.set_mode(R,pigpio.OUTPUT)
pi.set_mode(G,pigpio.OUTPUT)
pi.set_mode(B,pigpio.OUTPUT)

eteindre()
etat = "off"

dernier = 1 
while True:
    signal = pi.read(btn)
    if signal != dernier:
        if signal == 0:
            etat = changerEtat(etat)
    dernier = signal
    time.sleep(0.1)
```
{{% /expand %}}
{{% expand "Solution 3." %}}
```python
import pigpio
import time

R,G,B = 17,27,22
btn = 23

def rouge():
    pi.write(R,0)
    pi.write(G,1)
    pi.write(B,1)

def vert():
    pi.write(R,1)
    pi.write(G,0)
    pi.write(B,1)

def bleu():
    pi.write(R,1)
    pi.write(G,1)
    pi.write(B,0)

def eteindre():
    for pin in [R,G,B]: pi.write(pin,1)

def changerEtat(e):
    if e == "off" or e == "b":
        rouge()
        return "r"
    elif e == "r":
        vert()
        return "v"
    elif e == "v":
        bleu()
        return "b"

pi = pigpio.pi()

pi.set_mode(btn,pigpio.INPUT)
pi.set_mode(R,pigpio.OUTPUT)
pi.set_mode(G,pigpio.OUTPUT)
pi.set_mode(B,pigpio.OUTPUT)

eteindre()
etat = "off"

dernier = 1 
while True:
    signal = pi.read(btn)
    if signal != dernier:
        if signal == 0:
            etat = changerEtat(etat)
    dernier = signal
    time.sleep(0.1)
```
{{% /expand %}}
{{% expand "Solution 4." %}}
```python
import pigpio
import time

R,G,B = 17,27,22
btn = 23

def rouge():
    pi.write(R,0)
    pi.write(G,1)
    pi.write(B,1)

def vert():
    pi.write(R,1)
    pi.write(G,0)
    pi.write(B,1)

def bleu():
    pi.write(R,1)
    pi.write(G,1)
    pi.write(B,0)

def jaune():
    pi.write(R,0)
    pi.write(G,0)
    pi.write(B,1)

def mauve():
    pi.write(R,0)
    pi.write(G,1)
    pi.write(B,0)

def cyan():
    pi.write(R,1)
    pi.write(G,0)
    pi.write(B,0)

def eteindre():
    for pin in [R,G,B]: pi.write(pin,1)

def changerEtat(e):
    if e == "off" or e == "m":
        rouge()
        return "r"
    elif e == "r":
        jaune()
        return "j"
    elif e == "j":
        vert()
        return "v"
    elif e == "v":
        cyan()
        return "c"
    elif e == "c":
        bleu()
        return "b"
    elif e == "b":
        mauve()
        return "m"
    

pi = pigpio.pi()

pi.set_mode(btn,pigpio.INPUT)
pi.set_mode(R,pigpio.OUTPUT)
pi.set_mode(G,pigpio.OUTPUT)
pi.set_mode(B,pigpio.OUTPUT)

eteindre()
etat = "off"

dernier = 1 
while True:
    signal = pi.read(btn)
    if signal != dernier:
        if signal == 0:
            etat = changerEtat(etat)
    dernier = signal
    time.sleep(0.1)
```
{{% /expand %}}
