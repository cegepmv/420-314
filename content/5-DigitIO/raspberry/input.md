+++
title = 'Senseurs'
date = 2024-08-12T18:41:05-04:00
draft = false
weight = 43
+++

Dans cette section nous verrons comment utiliser le GPIO pour traiter des signaux en entrée (ou _input_).

## Capter le signal d'un bouton poussoir simple
Dans l'exemple suivant nous allons utiliser un bouton poussoir comme interrupteur d'un circuit et afficher son état (0 ou 1) à la ligne de commande.

Posez le bouton sur la plaquette et reliez une de ses pattes à la broche du GPIO 4 sur votre Pi.

Le GPIO 4 sera utilisé en mode `INPUT` pour détecter si le bouton est appuyé. Une entrée ne fournit pas le courant comme une sortie; une résistance de rappel (*pull-up* ou *pull-down*) fixe son niveau logique lorsque le bouton est relâché. On peut câbler le bouton de l'une des deux façons suivantes :
+ Entre le GPIO 4 et GND, avec une résistance de rappel vers 3,3 V (`PUD_UP`).
+ Entre le GPIO 4 et 3,3 V, avec une résistance de rappel vers GND (`PUD_DOWN`).

Dans le programme, il faudra appeler la fonction `set_pull_up_down()` et lui passer un argument qui spécifie à laquelle des deux possibilités notre circuit correspond.

#### Possibilité 1 : bouton entre GPIO et GND (`PUD_UP`)
La connexion est la suivante:

![pud_up](/420-314/images/pull-up.png)

Le programme suivant affiche **1** lorsque le bouton est relâché et **0** lorsqu'on appuie dessus :
```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(4,pigpio.INPUT)
pi.set_pull_up_down(4,pigpio.PUD_UP)

while True:
    print(pi.read(4))
```
Remarquez que la méthode `set_mode()` met le GPIO 4 en mode "INPUT".


#### Possibilité 2 : bouton entre GPIO et 3,3 V (`PUD_DOWN`)
La connexion est la suivante:

![pud_down](/420-314/images/pull-down.png)

{{% notice warning "ATTENTION" %}}
Assurez-vous de bien connecter le bouton sur une broche **3.3V**. Vous pouvez endommager le Pi si vous connectez le bouton sur 5V.
{{% /notice %}}

Le programme suivant affiche **0** lorsque le bouton est relâché et **1** lorsqu'on appuie dessus :

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(4,pigpio.INPUT)
pi.set_pull_up_down(4,pigpio.PUD_DOWN)

while True:
    print(pi.read(4))
```

## Capter le signal du module _Button Switch_
Dans l'exemple suivant nous allons connecter un bouton au *RaspberryPi* et afficher son état sur la ligne de commande.

Le bouton envoie un signal correspondant à `0` lorsqu'on l'appuie et `1` lorsqu'on le relâche. On peut utiliser ces signaux pour contrôler l'exécution d'un programme.

Vous aurez besoin du module suivant:

![kspshbut](/420-314/images/keyestudio-push-button.png?width=150px)

Connectez le bouton au *Pi* comme suit:
+ Broche `V` sur une broche **3,3 V** du Pi
+ Broche `G` sur broche `9` (Ground)
+ Broche `S` sur broche `16` (GPIO 23)

N'alimentez pas ce module en 5 V lorsque sa sortie `S` est reliée directement au GPIO du Pi : les GPIO du Pi ne tolèrent pas une entrée de 5 V.

Exécutez ensuite le programme suivant:

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(23,pigpio.INPUT)

while True:
    print(pi.read(23),end="")
```

Lorsque vous exécutez ce programme, une série de "1" s'affiche en continu, et "0" s'affiche lorsque vous cliquez le bouton.


## Exercices (avec le module LED Keystudio)
1. Faites un programme qui affiche "0" une seule fois lorsqu'on clique le bouton, et qui affiche "1" une seule fois lorsqu'on le relâche.
2. Faites un programme qui affiche seulement "clic" chaque fois qu'on clique sur le bouton.

{{% expand "Solution 1." %}}
```python
import pigpio

pi = pigpio.pi()
pi.set_mode(23,pigpio.INPUT)

dernier = 1
while True:
    signal = pi.read(23)
    if signal != dernier: 
        print(signal)
    dernier = signal
```
{{% /expand %}}

{{% expand "Solution 2." %}}
```python
import pigpio

pi = pigpio.pi()
pi.set_mode(23,pigpio.INPUT)

dernier = 1
while True:
    signal = pi.read(23)
    if signal != dernier and signal == 0: 
        print("clic")
    dernier = signal
```
{{% /expand %}}
