+++
title = 'GPIO'
date = 2024-08-01T18:05:56-04:00
draft = false
weight = 31
+++

GPIO signifie "General Purpose Input/Output" et désigne la série de broches métalliques qui servent à envoyer ou recevoir des signaux électriques sur un microcontrôleur.

Le RaspberryPi en comprend 40:
+ 2 broches pour un courant de 3.3V
+ 2 broches pour un courant de 5V
+ 8 broches pour le courant négatif ("ground")
+ 28 broches génériques

![gpio-p1](/420-314/images/GPIO_P1.png)

> Parmi les 28 broches génériques, certaines donnent accès à des fonctionnalités plus spécifiques. Pour plus de détails: https://pinout.xyz/

Les broches sont numérotées, ce qui permet de les identifier dans les programmes qui utilisent le Pi pour contrôler des composantes électroniques. Mais attention: le numéro de broche ne correspond pas au numéro GPIO. Par exemple, la broche `29` est associée à `GPIO 5`. 

Dans un programme, c'est le numéro du GPIO qui doit être utilisé (pas le numéro de la broche).


## Utiliser le GPIO avec des modules Keystudio
Dans l'exemple suivant, nous allons connecter un module LED sur le Pi et le contrôler par un programme. 

Vous aurez besoin des composantes suivantes:

| | |
|:--|--|
| Module LED blanche | ![mod1led](/420-314/images/mod1led.png?width=150px) |
| 3 connecteurs F-F | ![3jumpff](/420-314/images/3jumpff.png?width=150px) |

Connectez le module LED au Pi comme suit:
+ Broche `V` sur broche `2` (5V)
+ Broche `G` sur broche `9` (Ground)
+ Broche `S` sur broche `11` (GPIO 17)

IMAGE

Lancez ensuite le programme suivant sur votre Pi:
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

La ligne `pi = pigpio.pi()` sert à initialiser la librairie *pigpio*.

La ligne `pi.set_mode(17,pigpio.OUTPUT)` dit à pigpio que `GPIO 17` sera utilisé pour *envoyer* un signal (les broches GPIO peuvent aussi *recevoir* des signaux électriques).

La ligne `pi.write(17,1)` permet d'envoyer un signal au `GPIO 17`. Avec l'argument `0`, le programme cesse d'envoyer le signal.

{{% notice warning "Remarque" %}}
Avec une LED ordinaire, c'est en envoyant un courant positif qu'on l'allume. Dans le cas des modules Keystudio, il faut toujours les alimenter avec un courant positif, et c'est le signal envoyé sur la broche `S` (contrôlé par le GPIO) qui agit comme un interrupteur.
{{% /notice %}}

#### Exercices
1. Branchez le module "buzzer" sur la broche `GPIO 18`, et faites-la émettre un son durant 2 secondes.
2. Faites un programme qui émet un son chaque fois que la LED est allumée. Un nombre passé au programme définit le nombre de fois par seconde où la LED s'allume (utilisez `sys.argv[]` dans votre programme). Par exemple, pour l'appel suivant la LED et le `buzzer`s'allumeront 4 fois par seconde:
```python
python exercice.py 4
```  

## Utiliser le GPIO avec des composantes de base
Les broches 1, 2, 4 et 17 du Pi envoient un courant continu de 3.3V ou 5V. 

Le signal envoyé par le Pi sur les broches GPIO est aussi un courant électrique de 3.3V et 35.2mA. Cela signifie qu'on peut utiliser ces broches pour contrôler un signal électrique lorsu'on veut utiliser le Pi avec des composantes électroniques ordinaires.

Dans ce qui suit, nous allons alimenter une LED ordinaire en utilisant le GPIO dans un programme.

Connectez votre _Pi_ à la plaquette de prototypage comme suit:
+ Broche 9 (Ground) dans la rangée "-"
+ Broche 11 (GPIO 17) dans la rangée "+"

> Sachant qu'une LED ordinaire doit recevoir un courant de 20mA, quelle est idéalement la résistance qu'on doit utiliser?

Posez ensuite une LED et une résistance sur la plaquette, puis lancez le programme de l'exemple précédent.

{{% notice warning "Remarque" %}}
Lorsque c'est possible, il est préférable d'utiliser les modules Keystudio plutôt que les composantes électroniques de base, car les risques de surcharge ou de court-circuits sont plus faibles.
{{% /notice %}}

## Senseurs et actuateurs
Les modules Keystudio, comme les composantes électroniques ordinaires, peuvent être utilisés de deux manières. Le *Pi* peut y envoyer un signal, comme avec une LED ou le *buzzer*, mais il peut aussi recevoir un signal comme avec un bouton, un détecteur de température ou de luminosité, etc.

Les modules "output", qui recoivent un signal du *Pi*, sont généralement nommés ***actuateurs***.

Les modules "input", qui envoient un signal vers le *Pi*, sont nommés ***senseurs***.

#### Capter le signal du module _Button Switch_
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


#### Capter le signal d'un bouton poussoir simple
Dans l'exemple suivant nous allons utiliser un bouton poussoir comme interrupteur d'un circuit et afficher sont état (0 ou 1) à la ligne de commande.

Posez le bouton sur la plaquette et reliez une de ses pattes à la broche du GPIO 4 sur votre Pi:

IMAGE

Le GPIO 4 sera utilisé en mode INPUT puisqu'il doit détecter si le bouton est appuyé. Mais il peut aussi fournir un courant de 3.3V au circuit. Il y a donc 2 possibilités pour la 2e broche du bouton:
+ On le connecte à une broche GND du Pi, donc GPIO 4 fournit le courant
+ On le connecte à une broche 3.3V du Pi qui fournit le courant, donc GPIO 4 agit comme GND

Dans le programme, il faudra appeler la fonction `set_pull_up_down()` et lui passer un argument correspondant à la connexion choisie.

##### GPIO fournit 3.3V
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

##### GPIO est GND
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


## Références
+ GPIO: https://pinout.xyz/
+ Module *pigpio*: https://abyz.me.uk/rpi/pigpio/python.html
+ Modules Keystudio: https://docs.keyestudio.com/projects/KS0522/en/latest/KS0522.html


