+++
title = 'Sortie analogique'
date = 2026-06-12T20:34:09-04:00
draft = false
weight = 6
pre = "6. "

+++

La modulation à largeur d'impulsion (MLI, ou en anglais **PWM** pour *Pulse Width Modulation*), est une technique très utilisée dans le domaine de l'électronique. Elle permet de contrôler la puissance délivrée à une composante électrique en modulant la _durée_ des impulsions électriques. Dans ce cours, on l'utilisera de deux façons:
- Simuler un signal analogique
- Contrôler des moteurs

Dans cette section nous verrons comment fonctionne cette technique et comment le module natif `machine` de MicroPython permet d'utiliser le PWM pour simuler des signaux analogiques.

## Fonctionnement
Comme nous l'avons vu précédemment, un microcontrôleur comme l'ESP32 ne peut envoyer nativement que des signaux numériques via ses broches GPIO standard. Il y a deux états possibles : soit elles envoient un courant (3.3V), soit elles n'en envoient pas (0V). Il n'est pas possible de faire varier le voltage sur celles-ci de façon continue.



Le PWM permet de simuler des signaux analogiques envoyés ***à partir*** de l'ESP32 vers un actuateur (comme une LED ou un moteur).
Le principe est assez simple. Si on alterne très rapidement le courant entre 3.3V et 0V, la tension moyenne perçue dans le circuit va changer.

Si la durée de l'impulsion de 3.3V est égale à la durée du signal de 0V, la moyenne perçue sera de 1.65V (50% de 3.3V). Cette moyenne dépend du rapport entre le temps "haut" et le temps "bas" sur une **période** donnée. 

![pwm1](/420-314/images/pwm1.png)
On appelle *cycle de travail* (en anglais, **duty cycle**) cette proportion entre la durée de l'impulsion et la durée totale de la période. 
- Un *duty cycle* de 20% donnera une tension moyenne d'environ 0.66V (20% de 3.3V).
- Un *duty cycle* de 80% donnera environ 2.64V.

Un autre élément important est la **fréquence** du signal (le nombre de périodes par seconde), exprimée en Hertz (Hz). Pour une LED, une fréquence de 1000 Hz (1 kHz) est idéale pour que l'œil humain ne perçoive aucun clignotement.

## Exemple avec MicroPython
Pour utiliser le PWM en MicroPython, on utilise la classe `PWM` du module `machine`.

En MicroPython sur ESP32, la valeur du *duty cycle* par défaut est configurée sur **10 bits**, ce qui signifie qu'elle varie de **0 à 1023** :
- `0` correspond à 0% (0V)
- `511` correspond à 50% (1.65V)
- `1023` correspond à 100% (3.3V)

### Programme
Dans ce programme, on allume graduellement une LED connectée à la broche GPIO 21.

```python
from machine import Pin, PWM
from Pins import Pins 
import time

# Configuration de la broche GPIO 21 en mode PWM
led_pin = Pin(21, Pin.OUT)
pwm_led = PWM(led_pin)

# On fixe la fréquence à 1000 Hz (1 kHz)
pwm_led.freq(1000)

MAX = 1023 # La valeur maximale du duty cycle en 10 bits
cycle = 0  # Variable pour l'intensité lumineuse

try:
    while cycle < MAX:
        cycle += 5  # On augmente un peu plus vite (0 à 1023)
        if cycle > MAX:
            cycle = MAX
        
        # En MicroPython, on utilise la méthode duty()
        pwm_led.duty(cycle) 
        time.sleep(0.01)
        
    # Éteindre proprement la LED à la fin
    pwm_led.duty(0)
    pwm_led.deinit() # Désactive le PWM sur la broche

except KeyboardInterrupt:
    # Sécurité en cas d'arrêt du programme
    pwm_led.duty(0)
    pwm_led.deinit()

```

---

## Exercices

1. Modifiez le programme de l'exemple pour que la LED commence à 100% (maximale) et diminue graduellement d'intensité jusqu'à s'éteindre.
2. Faites un programme qui fait augmenter, puis diminuer graduellement la luminosité de la LED, et continue ce cycle à l'infini.
3. Faites un programme qui demande à l'utilisateur d'entrer un pourcentage de luminosité (0-100) au clavier et ajuste la LED en conséquence. *(Indice : vous devrez convertir le pourcentage 0-100 en une valeur de duty cycle allant de 0-1023)*.

### {{% expand "Solution 1." %}}

```python
from machine import Pin, PWM
from Pins import Pins 
import time

led_pin = Pin(21, Pin.OUT)
pwm_led = PWM(led_pin)
pwm_led.freq(1000)

cycle = 1023

try:
    while cycle > 0:
        cycle -= 5
        if cycle < 0:
            cycle = 0
        pwm_led.duty(cycle)
        time.sleep(0.01)
        
    pwm_led.duty(0)
    pwm_led.deinit()
    
except KeyboardInterrupt:
    pwm_led.duty(0)
    pwm_led.deinit()

```

{{% /expand %}}

### {{% expand "Solution 2." %}}

```python
from machine import Pin, PWM
from Pins import Pins 
import time

led_pin = Pin(21, Pin.OUT)
pwm_led = PWM(led_pin)
pwm_led.freq(1000)

cycle = 0

try:
    while True:
        # Phase ascendante
        while cycle < 1023:
            cycle += 5
            if cycle > 1023: cycle = 1023
            pwm_led.duty(cycle)
            time.sleep(0.005)
            
        # Phase descendante
        while cycle > 0:
            cycle -= 5
            if cycle < 0: cycle = 0
            pwm_led.duty(cycle)
            time.sleep(0.005)
    
except KeyboardInterrupt:
    pwm_led.duty(0)
    pwm_led.deinit()

```

{{% /expand %}}

### {{% expand "Solution 3." %}}

```python
from machine import Pin, PWM
from Pins import Pins 
led_pin = Pin(21, Pin.OUT)
pwm_led = PWM(led_pin)
pwm_led.freq(1000)

try:
    pourcentage = int(input("SVP entrez une valeur de 0-100 : "))
    
    if 0 <= pourcentage <= 100:
        # Règle de trois pour passer de [0-100] à [0-1023]
        valeur_duty = int((pourcentage * 1023) / 100)
        pwm_led.duty(valeur_duty)
        print(f"Luminosité réglée à {pourcentage}% (Duty MicroPython: {valeur_duty})")
    else:
        print("Valeur hors limites !")

except ValueError:
    print("Veuillez entrer un nombre entier valide.")

```

{{% /expand %}}

---

## Références utiles

* [Documentation officielle MicroPython - Classe PWM](https://docs.micropython.org/en/latest/library/machine.PWM.html)
* [U=RI | Qu'est-ce que la PWM?](https://www.youtube.com/watch?v=CSReyYwbGRY)


