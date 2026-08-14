+++
title = 'Entrée analogique'
date = 2026-06-17T12:00:00-04:00
draft = false
weight = 7
pre = "7. "
+++

Dans cet exemple, nous allons utiliser le module de potentiomètre du kit *Keystudio* ("analog rotation sensor") pour générer un signal analogique, et nous verrons comment varier l'***atténuation*** pour modifier la plage des données reçues.

Contrairement au Raspberry Pi classique, l'ESP32 possède des convertisseurs analogique-numérique (ADC) intégrés. Plusieurs broches de l'ESP32 peuvent recevoir des signaux analogiques (toutes les broches qui commencent avec un A). Nous devons brancher la broche qui correspond au signal envoyé par le senseur (la broche "S") directement sur l'une de ces broches configurables en ADC.

Le potentiomètre doit donc être connecté comme suit :

* V : broche **3.3V** de l'ESP32
* G : broche **GND** de l'ESP32
* S : broche **A0** de l'ESP32

```python
from machine import ADC, Pin
from Pins import Pins 


# Initialisation sur A0
adc = ADC(Pin(Pins.A0))
# Configuration pour lire le 3.3V
adc.atten(ADC.ATTN_11DB)

while True:
    valeur = adc.read() # Lit entre 0 et 4095
    print("Valeur brute:", valeur)
```


## Interprétation des signaux analogiques

Lorsqu'on tourne le potentiomètre, le voltage envoyé par celui-ci vers l'ESP32 augmente ou diminue (selon le sens de rotation). L'ADC interne reçoit ce signal électrique et le traduit en valeur numérique.

Par défaut sous MicroPython, ce signal est une valeur stockée sur **12 bits** non signés (i.e. uniquement positif). Elle peut donc contenir 4096 valeurs distinctes, soit de `0` à `4095`.

Dans notre programme, nous configurons l'ADC pour lire ces données :

* `adc.read()` retourne les données brutes, une valeur entière entre 0 et 4095.


#### Atténuation (Plage de tension)

Le microcontrôleur ESP32 fonctionne nativement à une tension interne de 1.1V pour ses lectures ADC. Pour pouvoir lire des tensions plus élevées (comme le 3.3V fourni par la carte), le signal d'entrée doit être **atténué** (réduit) avant d'être mesuré.

MicroPython permet de configurer cette atténuation grâce à la méthode `atten()`. La constante d'atténuation passée en paramètre définit la tension maximale que la broche peut mesurer sans saturer.

Voici les configurations possibles de l'atténuation sur l'ESP32 :

| Constante MicroPython | Atténuation | Plage de tension mesurable |
| --- | --- | --- |
| `ADC.ATTN_0DB` | 0 dB | 0.0V à 1.1V |
| `ADC.ATTN_2_5DB` | 2.5 dB | 0.0V à 1.5V |
| `ADC.ATTN_6DB` | 6 dB | 0.0V à 2.2V |
| `ADC.ATTN_11DB` | 11 dB | 0.0V à 3.6V (idéal pour le 3.3V) |

> **Attention :** Il ne faut jamais appliquer une tension supérieure à 3.3V sur les broches de l'ESP32 sous peine d'endommager définitivement le microcontrôleur.

Si vous n'appliquez aucune atténuation (`ATTN_0DB`), la tension maximale lisible est de 1.1V. Si votre potentiomètre est branché sur le 3.3V, dès que vous dépasserez 1.1V en tournant le bouton, la valeur lue restera bloquée au maximum (`4095`). Pour utiliser toute la course du potentiomètre alimenté en 3.3V, il faut impérativement configurer l'atténuation à 11 dB (`ADC.ATTN_11DB`).

#### Précision et résolution

Puisque le signal est toujours converti sur 12 bits (4096 niveaux), changer l'atténuation modifie la précision de la mesure :

* Avec `ATTN_0DB` (max 1.1V), chaque unité numérique représente environ $1.1\text{V} / 4095 \approx 0.26\text{ mV}$.
* Avec `ATTN_11DB` (max 3.6V), chaque unité numérique représente environ $3.6\text{V} / 4095 \approx 0.87\text{ mV}$.

> Plus l'atténuation est faible, plus la **précision** sur les petites variations de tension est grande, mais plus la plage de tension maximale exploitable diminue.

---
## Exercices

1. Utilisez le potentiomètre pour faire allumer le module LED Keystudio : lorsque la valeur lue est supérieure à 2000, la LED s'allume; en dessous de 2000, elle s'éteint. On utilisera la broche **A1** pour la LED.
{{% expand "Solution" %}}

```python
from machine import Pin, ADC
from Pins import Pins 
import time

# Configuration des broches
LED = Pin(Pins.A1, Pin.OUT)
adc = ADC(Pin(Pins.A0))
adc.atten(ADC.ATTN_11DB)  # Plage jusqu'à ~3.6V

# Boucle principale
try:
    while True:
        valeur = adc.read()
        if valeur > 2000:
            LED.value(1)
        else:
            LED.value(0)
        time.sleep(0.1)

except KeyboardInterrupt:
    print("Programme interrompu.")
    LED.value(0)

```

{{% /expand %}}

2. Modifiez le programme de l'exercice précédent pour que le potentiomètre contrôle la luminosité de la LED en utilisant le PWM de l'ESP32.
{{% expand "Solution" %}}

```python
from machine import Pin, ADC, PWM
from Pins import Pins 
import time

# Configuration de la LED en PWM
# Note : Sur ESP32, le rapport cyclique (duty) en MicroPython va de 0 à 1023
led_pwm = PWM(Pin(Pins.A1), freq=1000)

# Configuration de l'ADC (0 à 4095)
adc = ADC(Pin(Pins.A0))
adc.atten(ADC.ATTN_11DB)

# Boucle principale
try:
    while True:
        val_brute = adc.read()
        
        # Conversion de l'échelle de l'ADC (0-4095) vers l'échelle PWM (0-1023)
        val_pwm = int(val_brute / 4095 * 1023)
        
        led_pwm.duty(val_pwm)
        print("ADC:", val_brute, "PWM:", val_pwm)
        time.sleep(0.1)

except KeyboardInterrupt:
    print("Programme interrompu.")
    led_pwm.deinit()

```

{{% /expand %}}

3. Connectez le bouton Keystudio à la broche **GPIO20** de votre ESP32 et alimentez-le en 3.3V. Modifiez votre programme pour que le bouton fonctionne comme une bascule (interrupteur ON/OFF) pour la LED. Le potentiomètre contrôle toujours la luminosité de la LED, mais uniquement lorsqu'elle est activée par le bouton.
{{% expand "Solution" %}}

```python
from machine import Pin, ADC, PWM
from Pins import Pins 
import time

# Configuration
led_pwm = PWM(Pin(Pins.A1), freq=1000)
bouton = Pin(20, Pin.IN, Pin.PULL_UP) # Utilisation de la résistance de tirage interne si nécessaire
adc = ADC(Pin(Pins.A0))
adc.atten(ADC.ATTN_11DB)

allume = False
dernier_etat_bouton = 1

# Boucle principale
try:
    while True:
        etat_bouton = bouton.value()
        time.sleep(0.05) # Anti-rebond (debounce) rapide
        
        # Détection d'un appui (front descendant si PULL_UP)
        if etat_bouton == 0 and etat_bouton != dernier_etat_bouton:
            allume = not allume
        dernier_etat_bouton = etat_bouton

        if allume:
            val_pwm = int(adc.read() / 4095 * 1023)
            led_pwm.duty(val_pwm)
        else:
            led_pwm.duty(0)

except KeyboardInterrupt:
    print("Programme interrompu.")
    led_pwm.deinit()

```

{{% /expand %}}

4. Dans cet exercice, c'est un senseur de luminosité qui contrôle si la LED s'allume ou s'éteint. Connectez le senseur de luminosité à la broche **GPIO34** de l'ESP32. Modifiez votre programme pour que le senseur de luminosité éteigne la LED lorsque la luminosité ambiante est inférieure à 50% de la plage maximale mesurable (0-4095). Le potentiomètre contrôle toujours la LED lorsqu'elle est allumée.
{{% expand "Solution" %}}

```python
from machine import Pin, ADC, PWM
from Pins import Pins 
import time

# Configuration
led_pwm = PWM(Pin(Pins.A1, Pin.OUT)), freq=1000)

adc_pot = ADC(Pin(Pins.A0))
adc_pot.atten(ADC.ATTN_11DB)

adc_lum = ADC(Pin(Pins.A2))
adc_lum.atten(ADC.ATTN_11DB)

MAX_VAL = 4095

# Boucle principale
try:
    while True:
        val_lum = adc_lum.read()
        
        # Si la luminosité est inférieure à 50% (environ 2047)
        if val_lum < (MAX_VAL / 2):
            val_pwm = int(adc_pot.read() / MAX_VAL * 1023)
            led_pwm.duty(val_pwm)
        else: 
            led_pwm.duty(0)
            
        time.sleep(0.1)

except KeyboardInterrupt:
    print("Programme interrompu.")
    led_pwm.deinit()

```

{{% /expand %}}

5. Quelle configuration d'atténuation (`atten`) serait optimale si le senseur de luminosité de la classe ne générait jamais une tension supérieure à 1.5V ? Modifiez votre code en conséquence pour augmenter la précision de la lecture de la luminosité.