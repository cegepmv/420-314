+++
title = 'Traitement des signaux analogiques'
date = 2024-08-20T11:56:27-04:00
draft = false
weight = 72
+++

Dans cet exemple nous allons utiliser le module de potentiomètre du kit *Keystudio* ("analog rotation sensor") pour générer un signal analogique, et nous verrons comment varier le ***gain*** pour modifier la qualité des données reçues.

![ars](/420-314/images/ars.png?width=200px)

Après avoir connecté le convertisseur au Pi, il reste 6 broches inutilisées; parmi celles-ci 4 peuvent être utilisées pour recevoir des signaux analogiques: il s'agit de A0, A1, A2 et A3. Nous devons donc brancher une de ces 4 broches à celle qui correspond au signal envoyé par le senseur (la broche "S").

Le potentiomètre doit donc être connecté comme suit:

+ V : broche **3.3V** du RaspberryPi
+ G : broche **Ground** du RaspberryPi
+ S : broche **A0** du convertisseur ADC

Lancez le programme *testADC.py* de la [section précédente](/420-314/7-analogin/ads1115/#tester-la-communication) et observez ce qui se passe lorsque vous tournez le bouton.

## Interprétation des signaux analogiques
Lorsqu'on tourne le potentiomètre, le voltage envoyé par celui-ci vers le convertisseur augmente ou diminue (selon le sens de rotation). Le convertisseur ADC reçoit ce signal électrique et le traduit en valeur numérique, puis l'envoit vers le RaspberryPi. 

Ce signal envoyé par la puce ADC vers le Pi est une valeur stockée sur **16 bits** signé (i.e. positif et négatif). En théorie elle peut donc contenir 65536 valeurs distinctes, soit de `-32768` à `+32767`. Mais attention: _ce nombre n'est pas une mesure directe du voltage_. 

Dans le programme, il y a deux méthodes pour accéder aux données envoyées par le module: 
- `value()` retourne les données brutes, une valeur entre -32768 et 32767.
- `voltage()` convertit ces données en voltage. _Les valeurs retournées sont le résultat d'un calcul_, elles ne correspondent pas à un voltage réel reçu par le _Pi_, et sont donc moins fiables.  

Le module ADS1115 est conçu pour recevoir des signaux entre -4.096V et +4.096V. Mais le potentiomètre, s'il est connecté sur 5V, envoit un signal entre 0 et 5V: sa valeur maximum dépasse donc celle que peut recevoir le module (+4.096V). Cela ne causera pas de dommages, mais les valeurs lues au-delà de cette limite seront erronées. Si vous branchez le potentiomètre sur 5V et que vous lancez le programme `testADC.py`, puis tournez le potentiomètre à son maximum, vous verrez que les valeurs commencent à devenir incohérentes à partir de 30000 environ.

Connectez maintenant le potentiomètre sur le courant 3.3V. Cette fois-ci lorsque vous le tournez au maximum, vous verrez que la valeur reçue est d'environ 26340: les valeurs sont bonnes car elles sont à l'intérieur des limites du convertisseur ADS1115.

> Il faut donc s'assurer que les valeurs envoyées au convertisseur ne dépassent pas le voltage maximum qu'il peut représenter sur les 16 bits. Donc les senseurs analogiques dont le signal sera traité par le convertisseur doivent toujours être alimentées par un courant de 3.3V.

#### Gain

Selon le senseur et le voltage du courant, les caractéristiques du signal envoyé peuvent être différentes. Comme on vient de le voir, il arrive que les limites des valeurs envoyées par le convertisseur ADS1115 ne correspondent pas exactement aux limites du courant envoyé par le senseur: avec un potentiomètre branché sur 3.3V, la valeur maximum du potentiomètre (3.3V) est inférieure au maximum du module ADS1115 (4.096V); avec un courant de 5V, le maximum du potentiomètre (5V) est supérieur à 4.096V. Mais il est possible d'ajuster le maximum du module ADS1115. 

Le courant électrique peut être **amplifié** par le module ADS1115, et il est possible de contrôler cet effet dans nos programmes. La variable `GAIN`, passée (optionnellement) au constructeur `ADS1115()`, correspond au facteur d'amplification qu'on souhaite avoir. Par exemple, pour multiplier par 2 le signal, on appellera la fonction comme suit:
```python
data = AnalogIn(ads, 2) 
```

Attention: le voltage _entrant_ qui vient du senseur ne change pas, ni le fait que c'est toujours sur 16 bits que le signal est représenté. Le gain amplifie le signal _sortant_, entre le module ADS1115 et le RaspberryPi.

Par exemple, pour un signal de 1V avec un gain de 1, la valeur lue sera d'environ 7999 car:
```
  1V        7999
------  =~  -----
4.096V      32767
```
Pour un signal de 1V avec un gain de 2, la valeur lue sera d'environ 16000 car le Pi reçoit un signal de 2V:
```
  2V        16000
------  =~  -----
4.096V      32767
```
Le gain a donc aussi, inversement, l'effet de **diviser la limite maximale du voltage entrant**: avec un gain de 1, le convertisseur peut recevoir des valeurs allant jusqu'à 4.096V; avec un gain de 2, la valeur entrante maximum est de 2.048; etc. Encore une fois, ce n'est pas une limite physique, mais seulement une limite sur les valeurs qui peuvent être représentées sur les 16 bits.

Le tableau suivant donne les valeurs possibles de la variable GAIN et l'effet qu'elles ont sur le signal envoyé:

![tblgain](/420-314/images/tblgain.png?width=400px)

> Il n'est pas possible de donner des valeurs de gain autre que celles du tableau: par exemple, des gains de 3 ou 32 ne sont pas possibles et causeront une erreur dans le programme.

Ceci signifie que:
+ Un gain de 1 permet de lire les voltages allant de -4.096V à 4.096V et donc, que la valeur 16bit de -32768 correspond à -4.096V et 32767 correspond à 4.096V.
+ Un gain de 2 permet de lire les voltages allant de -2.048V à 2.048V et donc, que la valeur 16bit de -32768 correspond à -2.048V et 32767 correspond à 2.048V.
+ etc.

En contrepartie, lorsque le gain augmente, il devient possible de détecter des plus petites variations de voltage car on a toujours 16bits pour représenter un ensemble de valeurs moins large. Par exemple, encore selon la table ci-haut:
+ Un gain de 1 permet de lire des variations de voltage de 0.1250mV.
+ Un gain de 2 permet de lire des variations de voltage de 0.0625mV.
+ etc.

#### À quoi ça sert?
Dans ce cours, on recommande que les senseurs dont le signal doit être converti par le module ADS1115 soient toujours connectés sur 3.3V. Si le voltage d'entrée est toujours le même, inférieur à 4.096V, à quoi peut donc servir la variable `GAIN`?

La réponse est que parfois, les senseurs ont des paramètres opérationnels très différents de ceux dans lesquels ils seront utilisés.

Imaginez une application pour une épicerie qui mesure le poids d'un objet qu'on dépose sur un plateau. La composante électronique qui détecte le poids a peut-être une limite de 100kg: elle convertit des poids de 0-100kg en une valeur de 0-3.3V. Cette valeur de 0-3.3V sera elle-même convertie en une valeur de 0-32767. Mais dans notre application, on veut seulement peser des fruits ou des légumes. On ne pense jamais dépasser 10kg même dans les cas extrêmes. Donc dans les valeurs de 0-32767, seulement 10% seront utilisés (soit 0-3277 ou 0,33V).

Si on définit un gain de 8, le voltage maximum (qui correspond à la valeur 32767) sera de 0,512V. On a donc une plus grande précision pour des variations de poids plus petites: les poids de 0-10kg correspondront à des valeurs de 0-21119. 

> Lorsque le gain augmente, la **précision** des valeurs lues augmente, mais l'écart entre les valeurs minimum et maximum diminue.


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

# Constantes GPIO
LED = 21

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
data = AnalogIn(ads, 0)
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
<!--
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
MAX_VAL = 32767

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
data = AnalogIn(ads, 0)
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
-->
3. Connectez le bouton Keystudio à votre Pi et alimentez-le avec un courant de 3.3V. Modifiez votre programme pour que le bouton ait l'effet d'allumer ou d'éteindre la LED. Le potentiomètre contrôle toujours la la LED lorsqu'elle est allumée.
<!--
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
-->
4. Dans cet exercice, c'est un senseur de luminosité qui contrôle si la LED s'allume ou s'éteint. Connectez le senseur de luminosité au port A1 du module ADC1115 et alimentez-le avec le courant de 3.3V. Modifiez votre programme pour que le senseur de luminosité éteigne la LED lorsque la luminosité est inférieure à 50% du maximum possible (attention, 50% ne correspond pas à 32767 / 2...). Le potentiomètre contrôle toujours la la LED lorsqu'elle est allumée.
<!--
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
MAX_VAL = 32767

# Initialisations
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS1115(i2c)
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
-->
5. Quelle valeur de GAIN serait optimale pour la luminosité ambiante de la classe?
