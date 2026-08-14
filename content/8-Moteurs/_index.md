+++
title = 'Moteurs'
date = 2026-06-17T14:34:42-04:00
weight = 8
pre = "8. "
draft = false
+++

{{% notice warning %}}
Notes pas encore adaptées aux ESP-32, nous allons devoir acheter, soit une source de courant externe ou un condensateur.
{{% /notice %}}

![dcmotor3v](/420-314/images/dcmotor3v.png)

## Fonctionnement
Les moteurs DC ("courant direct") sont les plus simples des moteurs électriques. Ils ont deux pôles (positif et négatif), et le sens de leur rotation change lorsqu'on inverse la polarité du courant sur ces pôles. 


## Contrôleur 
Un moteur DC ne peut pas être connecté directement sur les broches GPIO de l'ESP32 : l'intensité maximale recommandée sur celles-ci est d'environ 12 à 20mA, alors qu'un petit moteur de 3V-6V nécessite au moins 500mA. Tenter de le brancher directement grillerait instantanément la broche de votre microcontrôleur. 

Il faut donc utiliser une alimentation électrique séparée (comme la broche `VBUS` / 5V de la carte connectée en USB, ou une pile externe), et contrôler ce courant à l'aide du GPIO via un composant d'interface.

Plusieurs composantes électroniques peuvent être utilisées à cette fin, mais une des plus populaires est le contrôleur L293D (un double pont en H). 

![l293d](/420-314/images/l293d.png)

Le courant électrique destiné au moteur traverse ce circuit intégré. Lorsqu'on le connecte à l'ESP32, il permet d'utiliser les GPIO comme des interrupteurs logiques pour activer le moteur et inverser facilement son sens de rotation.

Utiliser un contrôleur a d'autres avantages :
- L'ESP32 fournit des signaux logiques de 3.3V, mais certains moteurs ont besoin d'une tension plus élevée. Le contrôleur L293D supporte une tension moteur allant jusqu'à 36V.
- On peut connecter et contrôler indépendamment deux moteurs DC avec un seul L293D.
- Il inclut des diodes de protection internes qui diminuent le risque de dommages électriques dus aux retours de courant (forces contre-électromotrices).

### Broches
![l293dpins](/420-314/images/l293dpinout.png)


## Exemple
Comme on le voit dans le programme suivant, on contrôle le sens de la rotation à partir des deux broches `INPUT` (`IN1` et `IN2`) : inverser leurs valeurs logiques inversera la rotation. Si les deux valeurs sont identiques (0 et 0, ou 1 et 1), le moteur ne tournera pas.

La broche `ENABLE` (`EN1`) agit comme un interrupteur principal : elle bloque ou laisse passer le courant vers le moteur. En MicroPython, nous utilisons le module `machine.Pin`.

```python
from machine import Pin
from Pins import Pins 
import time

# Broches GPIO (Adaptez les numéros selon vos connexions sur l'ESP32)
EN1 = Pin(Pins.A1, Pin.OUT)
IN1 = Pin(Pins.A2, Pin.OUT)
IN2 = Pin(Pins.A3, Pin.OUT)

try:
    # Définir le sens de rotation
    IN1.value(0)
    IN2.value(1)

    # Activer le moteur (Envoyer le courant)
    EN1.value(1)
    time.sleep(5)

finally:
    # Toujours arrêter le moteur à la fin pour la sécurité
    IN1.value(0)
    IN2.value(0)
    EN1.value(0)

```

Pour exécuter ce programme, vous devez faire les connexions correspondantes entre votre carte ESP32, le L293D et le moteur.

## Exercices

### 1. Changement de direction

Faites un programme qui fait tourner le moteur 1 seconde dans un sens et 1 seconde dans l'autre sens, puis qui se termine en arrêtant le moteur.

{{% expand "Solution" %}}

```python
from machine import Pin
from Pins import Pins 
import time

EN1 = Pin(Pins.A1, Pin.OUT)
IN1 = Pin(Pins.A2, Pin.OUT)
IN2 = Pin(Pins.A3, Pin.OUT)

try:
    # Activer l'alimentation du moteur
    EN1.value(1)

    # Sens horaire pendant 1 seconde
    IN1.value(0)
    IN2.value(1)
    time.sleep(1)

    # Sens antihoraire pendant 1 seconde
    IN1.value(1)
    IN2.value(0)
    time.sleep(1)
    
finally:
    # Arrêt sécuritaire
    IN1.value(0)
    IN2.value(0)
    EN1.value(0)

```

{{% /expand %}}

### 2. Contrôle de vitesse (PWM)

En utilisant la classe `PWM` de MicroPython sur la broche `ENABLE`, démarrez le moteur à sa vitesse maximale puis diminuez-la d'environ 10% à chaque seconde jusqu'à l'arrêt.

> **Note MicroPython :** Contrairement au Pi où le PWM variait de 0 à 255, le PWM natif de MicroPython utilise une résolution de 10 bits par défaut, soit des valeurs de **cycle de service (duty cycle)** allant de **0 à 1023**. Nous fixons également une fréquence standard de 1000 Hz.

{{% expand "Solution" %}}

```python
from machine import Pin, PWM
from Pins import Pins 
import time

# Configuration des broches de direction
IN1 = Pin(Pins.A2, Pin.OUT)
IN2 = Pin(Pins.A3, Pin.OUT)

# Configuration de la broche de vitesse en PWM (Fréquence de 1kHz)
en1_pwm = PWM(Pin(Pins.A1), freq=1000)

try:
    # Sens de rotation
    IN1.value(0)
    IN2.value(1)

    # Valeur maximale du duty cycle en MicroPython (10 bits = 1023)
    duty = 1023

    while duty > 0:
        en1_pwm.duty(duty)
        duty -= 102  # Diminution d'environ 10%
        if duty < 0:
            duty = 0
        time.sleep(1)

finally:
    # Arrêt et désactivation du PWM
    IN1.value(0)
    IN2.value(0)
    en1_pwm.duty(0)
    en1_pwm.deinit()

```

{{% /expand %}}

### 3. Démarrage progressif

Changez le programme du numéro précédent pour qu'il commence à une vitesse de zéro et augmente sa vitesse de rotation de 10% à toutes les secondes.

**Que remarquez-vous lors des premières secondes ?**

{{% expand "Solution" %}}
*Réponse attendue de l'étudiant : On remarque que le moteur ne commence pas à tourner dès que la valeur dépasse 0. Il faut atteindre un certain seuil de tension (souvent autour de 30% à 40% du duty cycle, soit vers 300-400) pour que le moteur surmonte sa friction interne et commence réellement à bouger. Il émet simplement un sifflement aigu au début.*
{{% /expand %}}
