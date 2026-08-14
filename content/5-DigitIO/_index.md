+++
type = "chapter"
title = "Entrée digital"
date = 2026-06-12T16:54:59-04:00
draft = false
weight = 5
pre = "5. "
+++



Dans cette section nous verrons comment utiliser le GPIO pour traiter des signaux en entrée (ou _input_).

## Capter le signal d'un bouton poussoir simple
Dans l'exemple suivant, nous allons utiliser un bouton poussoir comme interrupteur d'un circuit et afficher son état (0 ou 1) dans la console.

Posez le bouton sur la plaquette et reliez une de ses pattes à la broche **D2** (qui correspond au **GPIO 5** en MicroPython).

La broche GPIO 5 sera utilisée en mode `INPUT` puisqu'elle doit détecter si le bouton est appuyé. En électronique, une broche en entrée ne doit jamais être "flottante" (connectée à rien), sinon elle capte les parasites ambiants. On utilise donc des résistances internes de **Pull-Up** ou **Pull-Down** pour fixer son état par défaut.

Il y a 2 possibilités de branchement pour la 2e broche du bouton :

#### Possibilité 1 : Circuit avec Pull-Up interne (Bouton relié au GND)
C'est la méthode la plus courante et la plus sûre. La broche est connectée au GND via le bouton. Lorsqu'on n'appuie pas, la résistance de Pull-Up interne force le signal à `1` (3.3V). Lorsqu'on appuie, le courant fuit vers la terre et le signal tombe à `0`.

La connexion est la suivante :
+ Une patte du bouton sur **D2** (GPIO 5)
+ L'autre patte du bouton sur une broche **GND**



Le programme suivant affiche **0** lorsqu'on appuie sur le bouton :
```python
import machine
from Pins import Pins 
import time

# On configure le GPIO 5 en ENTREE avec la résistance de PULL_UP activée
bouton = machine.Pin(5, machine.Pin.IN, machine.Pin.PULL_UP)

while True:
    print(bouton.value())
    time.sleep(0.1) # Petite pause pour ne pas surcharger la console

```

Remarquez que la configuration de la résistance se fait directement à la création de l'objet `machine.Pin`.

#### Possibilité 2 : Circuit avec Pull-Down interne (Bouton relié au 3.3V)

Ici, la broche est connectée au 3.3V via le bouton. Lorsqu'on n'appuie pas, la résistance de Pull-Down interne maintient le signal à `0`. Lorsqu'on appuie, le signal monte à `1`.

La connexion est la suivante :

* Une patte du bouton sur **D2** (GPIO 5)
* L'autre patte du bouton sur la broche **3V3**

{{% notice warning "ATTENTION" %}}
Assurez-vous de bien connecter le bouton sur la broche **3V3**. Bien que l'Arduino Nano ESP32 possède une broche *VBUS* ou *VIN* (5V), injecter du 5V sur une broche GPIO va endommager instantanément le processeur de votre carte.
{{% /notice %}}

Le programme suivant affiche **1** lorsqu'on appuie sur le bouton :

```python
import machine
from Pins import Pins 
import time

# On configure le GPIO 5 en ENTREE avec la résistance de PULL_DOWN activée
bouton = machine.Pin(5, machine.Pin.IN, machine.Pin.PULL_DOWN)

while True:
    print(bouton.value())
    time.sleep(0.1)

```

---

## Capter le signal du module *Button Switch* (ex: Keyestudio)

Dans l'exemple suivant, nous allons connecter un module de bouton pré-assemblé à l'Arduino Nano ESP32 et afficher son état.

Ce type de module intègre déjà sa propre résistance. Il envoie un signal correspondant à `0` lorsqu'on l'appuie et `1` lorsqu'on le relâche.

Connectez le module à la carte comme suit :

* Broche **V** (VCC) sur la broche **3V3** de la carte (Alimentation 3.3V)
* Broche **G** (GND) sur une broche **GND** de la carte
* Broche **S** (Signal) sur la broche **D3** (qui correspond au **GPIO 6** en MicroPython)

Exécutez ensuite le programme suivant :

```python
import machine
from Pins import Pins 
import time

# Le module possède sa propre résistance, pas besoin de spécifier PULL_UP/DOWN
bouton_module = machine.Pin(6, machine.Pin.IN)

while True:
    # end="" permet d'afficher les chiffres collés les uns aux autres
    print(bouton_module.value(), end="")
    time.sleep(0.05)

```

Lorsque vous exécutez ce programme, une série de "1" s'affiche en continu, et des "0" apparaissent lorsque vous cliquez sur le bouton.

---

## Exercices

1. Faites un programme qui affiche "0" une seule fois lorsqu'on clique sur le bouton du module (GPIO 6), et qui affiche "1" une seule fois lorsqu'on le relâche.
2. Faites un programme qui affiche seulement "clic" chaque fois qu'on clique sur le bouton.

{{% expand "Solution 1." %}}

```python
import machine
from Pins import Pins 
import time

bouton = machine.Pin(6, machine.Pin.IN)

dernier = 1
while True:
    signal = bouton.value()
    if signal != dernier: 
        print(signal)
    dernier = signal
    time.sleep(0.01) # Anti-rebond (debounce) logiciel

```

{{% /expand %}}

{{% expand "Solution 2." %}}

```python
import machine
from Pins import Pins 
import time

bouton = machine.Pin(6, machine.Pin.IN)

dernier = 1
while True:
    signal = bouton.value()
    # Si l'état a changé ET que l'état actuel est 0 (appui)
    if signal != dernier and signal == 0: 
        print("clic")
    dernier = signal
    time.sleep(0.01) # Anti-rebond (debounce) logiciel

```

{{% /expand %}}