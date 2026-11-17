+++
title = 'Servomoteurs'
date = 2024-10-01T16:28:35-04:00
draft = true
weight = 62
+++

Un servomoteur positionnel est conçu pour atteindre une position angulaire commandée. Sa course varie selon le modèle; plusieurs servomoteurs courants couvrent environ 0 à 180 degrés. On s'en sert par exemple pour contrôler l’ouverture de fenêtres, déplacer des bras robotisés ou orienter des antennes et des caméras.

On contrôle les mouvements d'un servo-moteur en utilisant la technique PWM.

Dans cette section on utilisera le module *Micro Servo 9G*:

![microservo](/420-314/images/micro-servo.png)

Le fil rouge sert à l'alimentation, le brun à la masse et le jaune au signal de commande. Un servomoteur peut demander plus de courant que le Pi ne peut fournir : utilisez une alimentation adaptée si nécessaire et reliez sa masse à celle du Pi. Vérifiez aussi que l'entrée de commande du servo accepte un signal logique de 3,3 V.

## Contrôle des servos-moteurs
L'utilisation du PWM pour contrôler un servomoteur est soumise à quelques contraintes. La période du signal et la durée de l'impulsion à l'état haut sont définies par le fabricant; elles déterminent la position commandée.

<!-- Si on se fie aux [spécification techniques](http://www.ee.ic.ac.uk/pcheung/teaching/DE1_EE/stores/sg90_datasheet.pdf), -->
Pour l'exemple du *Micro Servo 9G*, on utilise un signal de période 20 ms et des impulsions de 0,5 ms à 2,5 ms (soit un cycle de travail de 2,5 % à 12,5 %). Ces valeurs et les angles correspondants dépendent du modèle : vérifiez la documentation du servo utilisé.

![pwm2](/420-314/images/pwm-2.png)

Les valeurs intermédiaires, par exemple -15°, +40°, etc. sont obtenues en faisant varier le cycle de travail entre 2.5% et 12.5%. Pour calculer le cycle de travail pour un angle donné, la logique est la suivante:

+ La plage de valeurs possibles en degrés est de 180 (de -90° à +90°)
+ La plage de valeurs possibles de cycle est de 10% (de 2.5% à 12.5%)
+ On rapporte l'angle recherché comme un pourcentage de 180
+ On rapporte ce pourcentage sur 10
+ On additionne 2.5 car la valeur minimale possible de cycle est 2.5%
  
Par exemple, pour un angle de +45°:
![calc_dc](/420-314/images/calc-dc.png)

+ Un angle de +45° correspond à 135° à partir de la valeur minimale du cycle de travail (-90° + 45°). 
+ 135 / 180 = 0.75
+ 0.75 x 10 = 7.5
+ 7.5 + 2.5 = 10

Donc un cycle de 10% correspond à un angle de +45°.

## Contrôle avec _pigpio_
Pour définir une période de 20 ms avec la bibliothèque *pigpio*, on règle la fréquence PWM sur la broche du servo avec `pi.set_PWM_frequency(servo, f)`, où `pi` est l'instance de la classe `Pi`, `servo` le numéro GPIO et `f` la fréquence en Hz.

> La fréquence et la période sont réciproques : **f (Hz) = 1000 / période (ms)**.

Aussi, il peut être utile de redéfinir la plage de valeurs possibles du cycle de travail. La bibliothèque `pigpio` utilise par défaut une plage de 256 valeurs pour `set_PWM_dutycycle()`. Pour utiliser une plage de 100 correspondant à des pourcentages, on appelle `pi.set_PWM_range(servo, 100)`.


## Exemple 1
Dans cet exemple on génère un mouvement angulaire de 90 degrés à gauche.

```python
import pigpio
from time import sleep

servo = 17 # Le numéro GPIO de la broche ou est connecté le servomoteur
FREQ = 50 # Fréquence en Hz de la période

pi = pigpio.pi()
pi.set_mode(servo,pigpio.OUTPUT)
pi.set_PWM_frequency(servo,FREQ)
pi.set_PWM_range(servo,100) # Valeurs possibles dutycycle de 0-100

plus90 = 12.5
moins90 = 2.5

pi.set_PWM_dutycycle(servo,plus90)
sleep(1)
pi.set_PWM_dutycycle(servo,moins90)
```

## Exemple 2
Dans cet exemple on demande à l'utilisateur d'entrer une valeur entre 2.5 et 12.5 et on applique la rotation correspondante. 
```python
import pigpio
from time import sleep

servo = 17
FREQ = 50 # Fréquence en Hz de la période

pi = pigpio.pi()
pi.set_mode(servo,pigpio.OUTPUT)
pi.set_PWM_frequency(servo,FREQ)
pi.set_PWM_range(servo,100) # Valeurs possibles dutycycle de 0-100

while True:
    dc = float(input("Entrez une valeur entre 2.5 et 12.5:"))
    pi.set_PWM_dutycycle(servo,dc)
```

## Exercices

1. Faites un programme similaire à l'exemple 2, mais l'utilisateur doit entrer un angle entre 0 et 180 degrés au lieu d'une valeur entre 2.5 et 12.5.
<!--
```python
import pigpio
from time import sleep

servo = 17
FREQ = 50 # Fréquence en Hz de la période

pi = pigpio.pi()
pi.set_mode(servo,pigpio.OUTPUT)
pi.set_PWM_frequency(servo,FREQ)
pi.set_PWM_range(servo,100) # Valeurs possibles dutycycle de 0-100

while True:
    dc = float(input("Entrez une valeur entre 0 et 180:"))
    dc = dc / 18 + 2.5
        pi.set_PWM_dutycycle(servo,dc)
```
-->

2. Au début du programme, votre servomoteur est à 0 degrés. Ensuite, à chaque clic d'un bouton Keystudio, l'angle augmente de 30 degrés. Lorsque l'angle atteint sa valeur maximale, on revient à 0.

