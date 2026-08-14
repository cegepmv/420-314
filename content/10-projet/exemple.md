+++
title = 'Exemple'
date = 2026-06-19T20:34:52-04:00
draft = true
weight = 10
pre = "10. "
+++

# Step 1

```
Je vais utiliser un R.E sensor de keyeye studio sur un ESP32 en MicroPython.
```

C'est un excellent choix de matériel. Par "R.E sensor" (Rotary Encoder), vous faites référence à l'encodeur rotatif de Keyestudio (généralement le module **KS0013** ou équivalent du type KY-040).

Pour gérer un encodeur rotatif proprement sur un ESP32 en MicroPython, l'utilisation des **interruptions (IRQs)** est indispensable pour ne rater aucune impulsion lorsque vous tournez le bouton.

Voici comment connecter votre module et le code MicroPython pour le faire fonctionner.

### 1. Le Câblage (ESP32 $\rightarrow$ Keyestudio)

Le module Keyestudio possède généralement 5 broches :

* **GND** $\rightarrow$ GND de l'ESP32
* **VCC** $\rightarrow$ 3.3V (L'ESP32 fonctionne en 3.3V, ce qui est parfait pour ses entrées)
* **CLK** (ou A) $\rightarrow$ GPIO 12 (par exemple)
* **DT** (ou B) $\rightarrow$ GPIO 14 (par exemple)
* **SW** (le bouton poussoir) $\rightarrow$ GPIO 27 (Optionnel, si vous voulez détecter l'appui)

---

### 2. Le Code MicroPython

Ce script utilise une interruption sur la broche `CLK`. À chaque fois que le signal change, il compare l'état avec la broche `DT` pour savoir si vous tournez dans le sens horaire ou antihoraire.

```python
from machine import Pin
import time

# Configuration des broches
clk_pin = Pin(12, Pin.IN, Pin.PULL_UP)
dt_pin = Pin(14, Pin.IN, Pin.PULL_UP)
sw_pin = Pin(27, Pin.IN, Pin.PULL_UP)

# Variables de suivi
compteur = 0
dernier_etat_clk = clk_pin.value()
derniere_action_bouton = 0

def encodeur_interruption(pin):
    global compteur, dernier_etat_clk
    
    etat_actuel_clk = clk_pin.value()
    
    # On ne traite le changement que sur un flanc descendant (par exemple)
    if etat_actuel_clk != dernier_etat_clk and etat_actuel_clk == 0:
        # Si l'état de DT est différent de CLK, on tourne dans un sens
        if dt_pin.value() != etat_actuel_clk:
            compteur += 1
            print(class"Sens horaire -> Valeur :", compteur)
        else:
            compteur -= 1
            print("Sens antihoraire -> Valeur :", compteur)
            
    dernier_etat_clk = etat_actuel_clk

def bouton_interruption(pin):
    global compteur, derniere_action_bouton
    temps_actuel = time.ticks_ms()
    
    # Anti-rebond (Debounce) logiciel pour le bouton (200ms)
    if time.ticks_diff(temps_actuel, derniere_action_bouton) > 200:
        compteur = 0
        print("Bouton pressé ! Réinitialisation du compteur à 0.")
        derniere_action_bouton = temps_actuel

# Attacher les interruptions
clk_pin.irq(trigger=Pin.IRQ_FALLING | Pin.IRQ_RISING, handler=encodeur_interruption)
sw_pin.irq(trigger=Pin.IRQ_FALLING, handler=bouton_interruption)

print("Encodeur prêt. Tournez le bouton...")

# Boucle principale vide (les interruptions gèrent tout en arrière-plan)
while True:
    time.sleep(1)

```

### Quelques conseils pour votre projet :

* **Les résistances de Pull-up :** Le fait d'activer `Pin.PULL_UP` dans MicroPython assure la stabilité des signaux si le module Keyestudio n'a pas de résistances de pull-up intégrées sur toutes ses lignes.
* **L'anti-rebond (Debounce) :** Les encodeurs mécaniques génèrent des "bruits" électriques (rebonds) quand on les tourne. Si vous remarquez que le compteur saute parfois de deux chiffres d'un coup ou s'affole, il existe d'excellentes bibliothèques MicroPython dédiées aux encodeurs (comme celle de *Peter Hinch*) qui intègrent un anti-rebond matériel ou logiciel très poussé.

Quel type de projet ou de menu essayez-vous de contrôler avec cet encodeur ?

# Step 2

J'adapte et corrige le code :

