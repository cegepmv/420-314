+++
title = 'Traitement des signaux analogiques'
date = 2024-08-20T11:56:27-04:00
draft = false
weight = 72
+++

Dans cet exemple nous allons utiliser le module de potentiomètre du kit *Keystudio* ("analog rotation sensor") pour générer un signal analogique, et nous verrons comment varier le ***gain*** pour modifier la qualité des données reçues.

![ars](/420-314/images/analog-rotation-sensor.png?width=200px)

Après avoir connecté le convertisseur au Pi, il reste 6 broches inutilisées; parmi celles-ci 4 peuvent être utilisées pour recevoir des signaux analogiques: il s'agit de A0, A1, A2 et A3. Nous devons donc brancher une de ces 4 broches à celle qui correspond au signal envoyé par le senseur (la broche "S").

Le potentiomètre doit donc être connecté comme suit:

+ V : broche **3.3V** du RaspberryPi
+ G : broche **Ground** du RaspberryPi
+ S : broche **A0** du convertisseur ADC

Lancez le programme *testADC.py* de la [section précédente](/420-314/7-analogin/ads1115/#tester-la-communication) et observez ce qui se passe lorsque vous tournez le bouton.

## Interprétation des signaux analogiques
Lorsqu'on tourne le potentiomètre, la tension envoyée par celui-ci vers le convertisseur augmente ou diminue (selon le sens de rotation). Le convertisseur ADC reçoit ce signal électrique et le traduit en valeur numérique, puis l'envoie vers le Raspberry Pi.

Ce signal envoyé par la puce ADC vers le Pi est une valeur stockée sur **16 bits** signé (i.e. positif et négatif). En théorie elle peut donc contenir 65536 valeurs distinctes, soit de `-32768` à `+32767`. Mais attention: _ce nombre n'est pas une mesure directe du voltage_. 

Dans le programme, il y a deux propriétés pour accéder aux données envoyées par le module :
- `value` retourne la conversion brute, une valeur signée entre -32768 et 32767.
- `voltage` estime la tension mesurée à l'entrée de l'ADS1115 à partir de cette valeur et du gain configuré. Le Raspberry Pi reçoit les données numériques par I2C; il ne reçoit pas cette tension sur une broche GPIO.

Avec un gain de 1, la plage de conversion différentielle de l'ADS1115 est de ±4,096 V. Cette plage ne signifie pas que l'on peut appliquer ces tensions directement à ses entrées : les broches doivent aussi respecter leurs limites électriques par rapport à VDD. Comme le module de cet exemple est alimenté en 3,3 V et que le potentiomètre fournit une tension entre 0 V et VDD, ne le branchez pas sur 5 V. Une tension hors des limites des broches peut endommager le convertisseur; au-delà de la plage de conversion, les résultats ne sont pas fiables. Pour lire un capteur dont la sortie dépasse VDD, utilisez un circuit d'adaptation approprié.

Connectez maintenant le potentiomètre sur la broche 3,3 V. La tension de sortie reste alors dans les limites électriques du convertisseur.

> Il faut respecter à la fois la plage de conversion choisie et les limites électriques des entrées. N'alimentez pas tous les capteurs automatiquement en 3,3 V : consultez leur documentation et adaptez leur signal si nécessaire.

#### Gain

La plage de conversion de l'ADS1115 peut être réglée avec son amplificateur à gain programmable (*PGA*). Ce réglage ne modifie pas la tension du capteur et n'envoie pas un signal amplifié vers le Raspberry Pi : il adapte la plage de conversion interne du convertisseur. Par exemple, on peut régler un gain de 2 ainsi :
```python
ads = ADS1115(i2c, gain=2)
```

Attention : la tension _entrante_ du capteur ne change pas et la conversion est toujours représentée sur 16 bits. Avec un gain de 1, la plage différentielle est de ±4,096 V; avec un gain de 2, elle est de ±2,048 V. Ces plages de conversion ne remplacent pas les limites électriques des broches décrites plus haut.

Par exemple, pour un signal différentiel de 1 V avec un gain de 1, la valeur lue sera d'environ 8000, car :
```
  1V        7999
------  =~  -----
4.096V      32767
```
Pour le même signal de 1 V avec un gain de 2, la valeur lue sera d'environ 16000, car la plage de conversion est deux fois plus petite :
```
  1V        16000
------  =~  -----
2.048V      32767
```
Augmenter le gain réduit donc la plage de conversion et permet de représenter de plus petites variations avec davantage de comptes numériques. Cela ne rend pas les broches capables de recevoir une tension supérieure à leurs limites électriques.

Le tableau suivant donne les valeurs possibles de la variable GAIN et l'effet qu'elles ont sur le signal envoyé:

![tblgain](/420-314/images/table-gain.png?width=400px)

> Il n'est pas possible de donner des valeurs de gain autre que celles du tableau: par exemple, des gains de 3 ou 32 ne sont pas possibles et causeront une erreur dans le programme.

Ceci signifie que:
+ Un gain de 1 associe les valeurs numériques de -32768 à 32767 à la plage différentielle de -4,096 V à environ +4,096 V.
+ Un gain de 2 associe ces mêmes valeurs à la plage différentielle de -2,048 V à environ +2,048 V.
+ etc.

En contrepartie, lorsque le gain augmente, la résolution de conversion s'améliore : les mêmes 16 bits couvrent une plage plus petite. Par exemple, selon la table ci-haut :
+ Un gain de 1 permet de lire des variations de voltage de 0.1250mV.
+ Un gain de 2 permet de lire des variations de voltage de 0.0625mV.
+ etc.

#### À quoi ça sert?
Dans l'exemple du potentiomètre, on utilise une alimentation de 3,3 V pour que son signal reste dans les limites électriques du convertisseur. Le gain peut être utile lorsqu'un capteur produit un signal plus petit que la plage de conversion choisie, à condition que ce signal respecte toujours les limites électriques des broches.

La réponse est que parfois, les senseurs ont des paramètres opérationnels très différents de ceux dans lesquels ils seront utilisés.

Imaginez une application pour une épicerie qui mesure le poids d'un objet déposé sur un plateau. Le capteur peut convertir une plage de 0 à 100 kg en une tension de 0 à 3,3 V. Si l'on veut peser au plus 10 kg, le signal utile ne dépassera que 0,33 V. Avec un gain de 1, cette tension correspond à environ 2640 comptes sur la plage de conversion de ±4,096 V.

Avec un gain de 8, la plage de conversion devient ±0,512 V; le même signal de 0,33 V correspond alors à environ 21119 comptes. Le convertisseur utilise ainsi davantage de ses comptes pour représenter la plage de poids utile, sans que cela augmente la précision intrinsèque du capteur.

> Lorsque le gain augmente, la **résolution** de conversion augmente, mais la plage mesurable diminue. La précision réelle dépend aussi du bruit et des autres caractéristiques du capteur et du convertisseur.


## Exercices
1. Utilisez le potentiomètre pour faire allumer le module LED Keystudio: lorsque la valeur lue est supérieure à 16000, la LED s'allume; en-dessous de 16000, elle s'éteint.
{{% expand "Solution" %}}
```python
import busio
import board
import time
import pigpio

from adafruit_ads1x15.ads1115 import ADS1115
from adafruit_ads1x15.analog_in import AnalogIn
from adafruit_ads1x15 import ads1x15

# Constantes GPIO
LED = 21

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
data = AnalogIn(ads, ads1x15.Pin.A0)
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)

# Boucle principale
try:
    while True:
        if data.value > 16000:
            pi.write(LED,1)
        else:
            pi.write(LED,0)

except KeyboardInterrupt:
    print("Programme interrompu.")
```
{{% /expand %}}
2. Modifiez le programme de l'exercice précédent pour que le potentiomètre contrôle la luminosité de la LED.
{{% expand "Solution" %}}
```python
import busio
import board
import time
import pigpio

from adafruit_ads1x15.ads1115 import ADS1115
from adafruit_ads1x15.analog_in import AnalogIn
from adafruit_ads1x15 import ads1x15

# Constantes 
LED = 21
MAX_VAL = 32767

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
data = AnalogIn(ads, ads1x15.Pin.A0)
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)
pi.set_PWM_range(LED,100)

# Boucle principale
try:
    while True:
        val_pct = data.value / MAX_VAL * 100
        pi.set_PWM_dutycycle(LED,val_pct)
        print(data.value,val_pct)

except KeyboardInterrupt:
    print("Programme interrompu.")
```
{{% /expand %}}
3. Connectez le bouton Keystudio à votre Pi et alimentez-le avec un courant de 3.3V. Modifiez votre programme pour que le bouton ait l'effet d'allumer ou d'éteindre la LED. Le potentiomètre contrôle toujours la la LED lorsqu'elle est allumée.
{{% expand "Solution" %}}
```python
import busio
import board
import time
import pigpio

from adafruit_ads1x15.ads1115 import ADS1115
from adafruit_ads1x15.analog_in import AnalogIn

# Constantes 
LED = 21
BTN = 20
MAX_VAL = 32767

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
data = AnalogIn(ads, 0)
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)
pi.set_mode(BTN,pigpio.INPUT)
pi.set_PWM_range(LED,100)

allume = False
dernier_clic = 1

# Boucle principale
try:
    while True:
        clic = pi.read(BTN)
        time.sleep(0.1)
        if clic == 0 and clic != dernier_clic:
            allume = not allume
        dernier_clic = clic

        if allume:
            pi.set_PWM_dutycycle(LED,data.voltage / 3.3 * 100)
        else:
            pi.write(LED,0)
            

except KeyboardInterrupt:
    print("Programme interrompu.")
```
{{% /expand %}}
4. Dans cet exercice, c'est un senseur de luminosité qui contrôle si la LED s'allume ou s'éteint. Connectez le senseur de luminosité au port A0 du module ADC1115 et alimentez-le avec le courant de 3.3V. Modifiez votre programme pour que le senseur de luminosité éteigne la LED lorsque la luminosité est inférieure à 50% du maximum possible (attention, 50% ne correspond pas à 32767 / 2...). Le potentiomètre contrôle toujours la la LED lorsqu'elle est allumée.
{{% expand "Solution" %}}
```python
import busio
import board
import time
import pigpio

from adafruit_ads1x15.ads1115 import ADS1115
from adafruit_ads1x15.analog_in import AnalogIn

# Constantes 
LED = 21
# Avec un gain de 1, le maximum possible est 26399 
# car 3,3V vaut 80,6% de 4.096V, et 80,6% de 32767 = 26399
MAX_VAL = 26399 
GAIN = 1

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c, GAIN)
data_pot = AnalogIn(ads, 0)
data_lum = AnalogIn(ads, 1)
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)
pi.set_PWM_range(LED,100)

# Boucle principale
try:
    while True:
        if data_lum.value < MAX_VAL / 2:
            val_pct = data_pot.value / MAX_VAL * 100
            pi.set_PWM_dutycycle(LED,val_pct)
        else: 
            pi.set_PWM_dutycycle(LED,0)

except KeyboardInterrupt:
    print("Programme interrompu.")
```
{{% /expand %}}
5. Quelle valeur de GAIN serait optimale pour la luminosité ambiante de la classe? Trouvez cette valeur et passez-la au constructeur `ADS1115()` dans le programme du numéro précédent.
