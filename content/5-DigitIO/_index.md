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

---

## Capter le signal du module *Button Switch* (ex: Keyestudio)

Dans l'exemple suivant, nous allons connecter un module de bouton pré-assemblé à l'Arduino Nano ESP32 et afficher son état.

Ce type de module intègre déjà sa propre résistance. Il envoie un signal correspondant à `0` lorsqu'on l'appuie et `1` lorsqu'on le relâche.

Connectez le module à la carte comme suit :

* Broche **V** (VCC) sur la broche **3V3** de la carte (Alimentation 3.3V)
* Broche **G** (GND) sur une broche **GND** de la carte
* Broche **S** (Signal) sur la broche **D3** 

Exécutez ensuite le programme suivant :

```python
import machine
from pins import Pins 
import time

# Le module possède sa propre résistance, pas besoin de spécifier PULL_UP/DOWN
bouton_module = machine.Pin(Pins.D3, machine.Pin.IN)

while True:
    print(bouton_module.value())
    time.sleep(0.5)

```

Lorsque vous exécutez ce programme, une série de "1" s'affiche en continu, et des "0" apparaissent lorsque vous cliquez sur le bouton.

---

## Exercices

1. Faites un programme qui affiche "0" une seule fois lorsqu'on clique sur le bouton du module, et qui affiche "1" une seule fois lorsqu'on le relâche.

{{% expand "Solution 1." %}}

```python
import machine
from pins import Pins 
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




2. Faites un programme qui affiche "click" seulement lorsqu'un click est complété (Appuyé puis relaché).

{{% expand title="Solution 2"%}}
```python
import machine
from pins import Pins 
import time


bouton = machine.Pin(Pins.D5, machine.Pin.IN)
cur = bouton.value()
prev = cur

while True:
    cur = bouton.value()
    if prev == 0 and cur == 1:
        print("click")
    prev = cur
    time.sleep(0.1) 
```
{{% /expand %}}

3. Faites un programme qui compte le nombre de click et qui s'arrête après 10 clicks. En premier afficher les clicks à chaque fois et remarquer que si vous `spammer` certain click ne sont pas comptabilisé. Retirer le `print`et compter vous même les clicks et vous constaterez qu'il n'y a plus de bug. 

{{% expand title="Solution 3" %}}
```py
import machine
from pins import Pins 
import time

# On configure le GPIO 5 en ENTREE avec la résistance de PULL_UP activée
bouton = machine.Pin(Pins.D5, machine.Pin.IN)
cur = bouton.value()
prev = cur
compteur = 0
while True:
    cur = bouton.value()
    if prev == 0 and cur == 1:
        compteur += 1
        if compteur == 10:
            break
    prev = cur
    time.sleep(0.01) 
```
{{% /expand %}}
{{% expand title="Solution async" %}}
```py
import uasyncio as asyncio
from machine import Pin

btn = Pin(Pins.D5, Pin.IN, Pin.PULL_UP)
print_buffer = []
count = 0
done_event = asyncio.Event()  # <-- L'objet événement

async def printer_worker():
    while True:
        if print_buffer:
            msg = print_buffer.pop(0)
            print(msg)
        await asyncio.sleep_ms(5)

async def button_task():
    global count
    last_state = 1
    
    while count < 10:
        current_state = btn.value()
        if last_state == 1 and current_state == 0:
            count += 1
            print_buffer.append(f"Click: {count}")
            
            if count >= 10:
                done_event.set()  # <-- Réveille la tâche en attente
                break
                
            await asyncio.sleep_ms(50)
            
        last_state = current_state
        await asyncio.sleep_ms(2)

async def main():
    print("Prêt (async/Event)...")
    asyncio.create_task(printer_worker())
    asyncio.create_task(button_task())
    
    # Bloque proprement la coroutine main sans consommer de CPU (pas de while/sleep)
    await done_event.wait()
    
    # Purge finale du buffer pour afficher le dernier print
    await asyncio.sleep_ms(200)
    print("10 clicks atteints !")

asyncio.run(main())
```
{{% /expand %}}

4. la LED doit rester allumée tant que le bouton est maintenu enfoncé, et s'éteindre dès qu'il est relâché.

5. Implémentez un interrupteur : à chaque nouveau click, changez l'état de la LED (si éteinte -> allumée ; si allumée -> éteinte).


## Comprendre l'état flottant et les résistances *Pull-up* / *Pull-down*

Lorsqu'on connecte un simple bouton poussoir à une broche GPIO configurée en entrée (`IN`), une question physique se pose : **quel est l'état du signal lorsque le bouton n'est pas enfoncé ?**

Si la broche est connectée à rien d'autre qu'au bouton, elle se trouve dans un **état flottant** (*floating*). Le microcontrôleur risque alors d'interpréter des parasites électriques ambiants comme des changements d'état aléatoires (des 0 et des 1 intempestifs).

Pour résoudre ce problème, on utilise des **résistances de tirage** :

### 1. Résistance *Pull-down* (Tirage vers le bas)

* **Principe :** La broche est reliée à la masse (`GND`) via une résistance. Par défaut, le signal lu est donc **`0`**.
* **Action :** Lorsque l'on appuie sur le bouton, on connecte la broche à l'alimentation (`3.3V`), le signal bascule à **`1`**.

### 2. Résistance *Pull-up* (Tirage vers le haut) — *La plus courante*

* **Principe :** La broche est reliée à l'alimentation (`3.3V`) via une résistance interne ou externe. Par défaut, le signal lu est donc **`1`**.
* **Action :** Lorsque l'on appuie sur le bouton, on connecte la broche à la masse (`GND`), le signal bascule à **`0`**.

---

### Configuration en MicroPython

L'ESP32 intègre directement des résistances de tirage logicielles, ce qui évite d'avoir à en ajouter physiquement sur votre breadboard. On peut les activer directement dans le constructeur `machine.Pin` :

```python
import machine
from pins import Pins 
import time

# Configuration d'une broche en entrée avec une résistance Pull-Up interne
# Par défaut, le bouton renverra 1 (relâché) et 0 (pressé)
bouton = machine.Pin(Pins.D5, machine.Pin.IN, machine.Pin.PULL_UP)

while True:
    print(bouton.value())
    time.sleep(0.2)

```

> **Résumé des modes disponibles :**
> * `machine.Pin.PULL_UP` : Active la résistance de tirage au +3.3V (repos à `1`).
> * `machine.Pin.PULL_DOWN` : Active la résistance de tirage au GND (repos à `0`).
> * *(Aucun paramètre)* : Désactive les résistances internes (utilisé si le module externe possède déjà sa propre résistance, comme le module *Button Switch* vu précédemment).
