+++
title = 'Contrôle avec PWM'
date = 2024-10-01T16:28:35-04:00
draft = false
weight = 62
+++


On contrôle les mouvements d'un servo-moteur en utilisant la technique PWM.

Dans cette section on utilisera le module *Micro Servo 9G*. On connecte les fils positifs et négatifs, et le fil jaune doit être connecté dans une des broches GPIO du Raspberry Pi.

## Contrôle des servos-moteurs
L'utilisation de PWM, lorsqu'on l'utilise pour contrôler des servos-moteurs, est soumise à quelques contraintes. En effet, la période de l'impulsion doit avoir une valeur bien spécifique (définie par le fabricant du moteur), et des cycles de travail précis correspondent aux movement angulaires qu'on veut faire faire par le moteur.

<!-- Si on se fie aux [spécification techniques](http://www.ee.ic.ac.uk/pcheung/teaching/DE1_EE/stores/sg90_datasheet.pdf), -->
Le servo-moteur *Micro Servo 9G* utilise un signal PWM dont la période est de 20ms, et des cycles de travail allant de 0.5ms à 2.5ms (donc 2.5% à 12.5%). Les angles correspondants à chacun de ces cycles de travail sont les suivants:

![pwm2](/420-314/images/pwm2.png)

Les valeurs intermédiaires, par exemple -15°, +40°, etc. sont obtenues en faisant varier le cycle de travail entre 2.5% et 12.5%. Pour calculer le cycle de travail pour un angle donné, la logique est la suivante:

+ La plage de valeurs possibles en degrés est de 180 (de -90° à +90°)
+ La plage de valeurs possibles de cycle est de 10% (de 2.5% à 12.5%)
+ On rapporte l'angle recherché comme un pourcentage de 180
+ On rapporte ce pourcentage sur 10
+ On additionne 2.5 car la valeur minimale possible de cycle est 2.5%
  
Par exemple, pour un angle de +45°:
![calc_dc](/420-314/images/calc_dc.png)

+ Un angle de +45° correspond à 135° à partir de la valeur minimale du cycle de travail (-90° + 45°). 
+ 135 / 180 = 0.75
+ 0.75 x 10 = 7.5
+ 7.5 + 2.5 = 10

Donc un cycle de 10% correspond à un angle de +45°.

## Contrôle avec _pigpio_
Pour définir une période de 20ms avec la librairie *pigpio* on doit utiliser la méthode ``set_PWM_frequency(pi,f)`` où ``pi`` désigne l'instance de la classe "Pi" qu'on utilise dans le programme et ``f`` désigne la fréquence en Hz correpsondant à la période. 

> Sachant que 1Hz correspond à une période de 1ms, la formule pour convertir en Hz une période mesurée en ms est **Hz = 1000 / ms**.

Aussi, il peut être utile de redéfinir la plage de valeurs possibles de cycles. En effet, la librairie ``pigpio`` utilise par défaut une plage de 256 valeurs lorsqu'on appelle la méthode ``set_PWM_dutycycle()``. Si on souhaite utiliser une plage de 100, ce qui serait utile si on représente les cycles comme des pourcentages, on doit appeler la méthode ``set_PWM_range(pi,max)`` où ``max`` correspond à la valeur maximale de la plage qu'on veut définir.


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

dc = 2.5

while True:
    pi.set_PWM_dutycycle(servo,dc)

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
<!--
## Exercice 1
Faites un programmes similaire à l'exmeple 2, mais l'utilisateur doit entrer un angle entre 0 et 180 degrés au lieu d'une valeur entre 2.5 et 12.5.

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

## Exercice 2
Utilisez un potentiomètre pour changer l'angle de rotation du servo moteur.

```python
import pigpio
from time import sleep
import busio
import adafruit_ads1x15.ads1115 as ADS
from adafruit_ads1x15.analog_in import AnalogIn

SCL = 3
SDA = 2
GAIN = 1
servo = 17
FREQ = 50 # Fréquence en Hz de la période

i2cBus = busio.I2C(SCL, SDA)
ads = ADS.ADS1115(i2cBus, GAIN)
a0 = AnalogIn(ads, 0)

pi = pigpio.pi()
pi.set_mode(servo,pigpio.OUTPUT)
pi.set_PWM_frequency(servo,FREQ)
pi.set_PWM_range(servo,100) # Valeurs possibles dutycycle de 0-100

while True:
    dc = abs(a0.value) / 3277 + 2.5
    pi.set_PWM_dutycycle(servo,dc)
```
-->
