+++
title = 'Actuateurs'
date = 2024-08-12T18:41:12-04:00
draft = false
weight = 42
+++

Su un _Pi_, les composantes électroniques peuvent être utilisés de deux manières. Le *Pi* peut y envoyer un signal, comme avec une LED ou le *buzzer*, mais il peut aussi recevoir un signal, comme avec un bouton, un détecteur de température ou de luminosité, etc.

Les composantes "input", qui envoient un signal vers le *Pi*, sont nommés ***senseurs***.

Les composantes "output", qui recoivent un signal du *Pi*, sont généralement nommés ***actuateurs***.

Dans cette section nous verrons comment utiliser le GPIO pour **envoyer** un signal (en _output_) à des composantes.

## Composantes éectroniques
Les broches 1, 2, 4 et 17 du Pi envoient un courant continu de 3.3V ou 5V. 

Le signal envoyé par le Pi sur les broches GPIO est aussi un courant électrique de 3.3V, mais beaucoup plus faible. Cela signifie qu'on peut utiliser ces broches pour contrôler un signal électrique lorsqu'on veut utiliser le Pi avec des composantes électroniques ordinaires.

Dans ce qui suit, nous allons alimenter une LED simple en utilisant le GPIO dans un programme.

Connectez votre _Pi_ à la plaquette de prototypage comme suit:
+ Broche 9 (Ground) dans la rangée "-"
+ Broche 11 (GPIO 17) dans la rangée "+"

> Sachant qu'une LED ordinaire doit recevoir un courant de 20mA, quelle est idéalement la résistance qu'on doit utiliser?

Posez ensuite une LED et une résistance sur la plaquette, puis lancez le programme de l'exemple précédent.

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(17,pigpio.OUTPUT)

pi.write(17,1)
time.sleep(1)
pi.write(17,0)
time.sleep(1)
```


La ligne `pi = pigpio.pi()` sert à initialiser la librairie *pigpio*.

La ligne `pi.set_mode(17,pigpio.OUTPUT)` dit à pigpio que `GPIO 17` sera utilisé pour *envoyer* un signal (les broches GPIO peuvent aussi *recevoir* des signaux électriques).

La ligne `pi.write(17,1)` permet d'envoyer un signal au `GPIO 17`. Avec l'argument `0`, le programme cesse d'envoyer le signal.

<!-- J'ai du faire sudo systemctl start pigpiod -->
<!-- OT: Il faut aussi faire 'enable' comme dans https://cegepmv.github.io/420-314/prep/config/index.html#_pigpio_-->

{{% notice style="green" title="Attention!" %}}
Si le programme ne trouve pas le module _pigpio_, il faut:
1. Démarrer le service _pigpiod_ sur le Pi
2. L'activer pour qu'il démarre en même temps que le Pi.
Les deux commandes sont celles-ci:
```
sudo systemctl start pigpiod
sudo systemctl enable pigpiod
```
{{% /notice %}}

## Modules Keystudio
Lorsque c'est possible, il est préférable d'utiliser les modules Keystudio plutôt que les composantes électroniques de base, car les risques de surcharge ou de court-circuits sont plus faibles.

Dans l'exemple suivant, nous allons connecter un module LED sur le Pi et le contrôler par un programme. 

Vous aurez besoin du module "LED" Keystudio suivant:

![mod1led](/420-314/images/mod1led.png?width=200px) 

Connectez le module LED au Pi comme suit:
+ Broche `V` sur broche `2` (5V)
+ Broche `G` sur broche `9` (Ground)
+ Broche `S` sur broche `11` (GPIO 17)


Lancez une nouvelle fois le programme de l'exemple précédent.

{{% notice warning "Remarque" %}}
Avec une LED ordinaire, c'est en envoyant un courant positif qu'on l'allume. Dans le cas des modules Keystudio, il faut toujours les alimenter avec un courant positif, et c'est le signal envoyé sur la broche `S` (contrôlé par le GPIO) qui agit comme un interrupteur.
{{% /notice %}}

## Exercices
1. Faire un programme qui fait clignoter la LED Keystudio à chaque seconde
2. Connectez une LED normale sur une autre broche GPIO et faites un programme qui fait clignoter les deux LED en alternance (lorsqu'une est éteinte, l'autres est allumée)
{{% expand "Solution 1." %}}
```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(17,pigpio.OUTPUT)

while True:
    pi.write(17,1)
    time.sleep(1)
    pi.write(17,0)
    time.sleep(1)
```
{{% /expand %}}
{{% expand "Solution 2." %}}
```python
import pigpio
import time

LED1 = 17
LED2 = 21

pi = pigpio.pi()
pi.set_mode(LED1,pigpio.OUTPUT)
pi.set_mode(LED2,pigpio.OUTPUT)

while True:
    pi.write(LED1,1)
    pi.write(LED2,0)
    time.sleep(1)
    pi.write(LED1,0)
    pi.write(LED2,1)
    time.sleep(1)
```
{{% /expand %}}