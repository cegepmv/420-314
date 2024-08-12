+++
title = 'Senseurs'
date = 2024-08-12T18:41:05-04:00
draft = false
weight = 33
+++

Dans cette section nous verrons comment utiliser le GPIO pour traiter des signaux en entrée (ou _input_).


## Capter le signal du module _Button Switch_
Dans l'exemple suivant nous allons connecter un bouton au *RaspberryPi* et afficher son état sur la ligne de commande.

Le bouton envoit un signal correspondant à `0` lorsqu'on l'appuie et `1` lorsqu'on le relâche. On peut utiliser ces signaux pour contrôler l'exécution d'un programme.

Vous aurez besoin des composantes suivantes:

| | |
|:--|--|
| Module "Button Switch" | ![kspshbut](/420-314/images/kspshbut.png?width=150px) |
| 3 connecteurs F-F | ![3jumpff](/420-314/images/3jumpff.png?width=150px) |

Connectez le bouton au *Pi* comme suit:
+ Broche `V` sur broche `2` (5V)
+ Broche `G` sur broche `9` (Ground)
+ Broche `S` sur broche `16` (GPIO 23)

Exécutez ensuite le programme suivant:

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(23,pigpio.INPUT)

while True:
    print(pi.read(23),end="")
```

Remarquez que la méthode `set_mode()` met le GPIO 23 en mode "INPUT".

Lorsque vous exécutez ce programme, une série de "1" s'affiche en continu, et "0" s'affiche lorsque vous cliquez le bouton.


## Capter le signal d'un bouton poussoir simple
Dans l'exemple suivant nous allons utiliser un bouton poussoir comme interrupteur d'un circuit et afficher sont état (0 ou 1) à la ligne de commande.

Posez le bouton sur la plaquette et reliez une de ses pattes à la broche du GPIO 4 sur votre Pi:

IMAGE

Le GPIO 4 sera utilisé en mode INPUT puisqu'il doit détecter si le bouton est appuyé. Mais il peut aussi fournir un courant de 3.3V au circuit. Il y a donc 2 possibilités pour la 2e broche du bouton:
+ On le connecte à une broche GND du Pi, donc GPIO 4 fournit le courant
+ On le connecte à une broche 3.3V du Pi qui fournit le courant, donc GPIO 4 agit comme GND

Dans le programme, il faudra appeler la fonction `set_pull_up_down()` et lui passer un argument correspondant à la connexion choisie.

#### Possibilité 1: GPIO fournit 3.3V
La connexion est la suivante:

![pud_up](/420-314/images/pud_up.png)

Le programme suivant affiche **0** lorsqu'on appuie sur le bouton:
```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(4,pigpio.INPUT)
pi.set_pull_up_down(4,pigpio.PUD_UP)

while True:
    print(pi.read(4))
```

#### Possibilité 2: GPIO est GND
La connexion est la suivante:

![pud_down](/420-314/images/pud_down.png)

{{% notice warning "ATTENTION" %}}
Assurez-vous de bien connecter le bouton sur une broche **3.3V**. Vous pouvez endommager le Pi si vous connectez le bouton sur 5V.
{{% /notice %}}

Le programme suivant affiche **1** lorsqu'on appuie sur le bouton:

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(4,pigpio.INPUT)
pi.set_pull_up_down(4,pigpio.PUD_DOWN)

while True:
    print(pi.read(4))
```
#### Exercices

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




