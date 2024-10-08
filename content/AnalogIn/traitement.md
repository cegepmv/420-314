+++
title = 'Traitement des signaux analogiques'
date = 2024-08-20T11:56:27-04:00
draft = false
weight = 42
+++

Dans cet exemple nous allons utiliser le module de potentiomètre du kit *Keystudio* ("analog rotation sensor") pour générer un signal analogique, et nous verrons comment varier le ***gain*** pour modifier la qualité des données reçues.

![ars](/420-314/images/ars.png?width=300px)

Après avoir connecté le convertisseur au Pi, il reste 6 broches inutilisées; parmi celles-ci 4 peuvent être utilisées pour recevoir des signaux analogiques: il s'agit de A0, A1, A2 et A3. Nous devons donc brancher une de ces 4 broches à celle qui correspond au signal envoyé par le senseur (la broche "S").

Le potentiomètre doit donc être connecté comme suit:

+ V : broche **5v** du RaspberryPi
+ G : broche **Ground** du RaspberryPi
+ S : broche **A0** du convertisseur ADC

Lancez le programme *testADC.py* et observez ce qui se passe lorsque vous tournez le bouton.

## Interprétation des signaux analogiques
Lorsqu'on tourne le potentiomètre, le voltage envoyé par celui-ci vers le convertisseur augmente ou diminue (selon le sens de rotation). Le convertisseur ADC reçoit ce signal électrique et le traduit en valeur numérique, puis l'envoit vers le RaspberryPi. 

Ce signal envoyé par la puce ADC vers le Pi est une valeur stockée sur 16 bits. En théorie elle peut donc contenir 65536 valeurs distinctes, de -32768 à +32767. Mais attention: ce nombre n'est pas une mesure directe du voltage. 

Le module ADS1115 est conçu pour recevoir des signaux entre -4.096V et +4.096V. Mais le potentiomètre, s'il est connecté sur 5V, envoit un signal entre 0 et 5V: sa valeur maximum dépasse donc celle que peut recevoir le module (+4.096V). Cela ne causera pas de dommages, mais les valeurs lues au-delà de cette limite seront erronées. Si vous lancez le programme `testADC.py` vu précédemment et tournez le potentiomètre à son maximum, vous verrez que les valeurs ne se rendent pas jusqu'à 32767 / 5V.

Connectez maintenant le potentiomètre sur le courant 3.3V. Cette fois-ci lorsque vous le tournez au maximum, vous verrez que la valeur reçue est d'environ 26340 / 3.3V: les valeurs sont bonnes car elles sont à l'intérieur des limites du convertisseur ADS1115.

#### Gain

Selon le senseur et le voltage du courant, les caractéristiques du signal envoyé peuvent être différentes. Comme on vient de le voir, il arrive que les limites des valeurs envoyées par le convertisseur ADS1115 ne correspondent pas exactement aux limites du courant envoyé par le senseur: avec un potentiomètre branché sur 3.3V, la valeur maximum du potentiomètre (3.3V) est inférieure au maximum du module ADS1115 (4.096V); avec un courant de 5V, le maximum du potentiomètre (5V) est supérieur à 4.096V. Mais il est possible d'ajuster le maximum du module ADS1115. 

Le courant électrique peut être amplifié par le module ADS1115, et il est possible de contrôler cette amplification dans nos programmes. La variable `GAIN`, passée (optionnellement) au constructeur `ADS1115()`, correspond au facteur d'amplification qu'on souhaite avoir. Par exemple, pour multiplier par 2 le signal, on appellera la fonction comme suit:
```python
import busio
import board
import time

from adafruit_ads1x15.ads1115 import ADS1115
from adafruit_ads1x15.ads1115 import P0
from adafruit_ads1x15.analog_in import AnalogIn
 
# Initialisation de l'interface i2c
i2c = busio.I2C(board.SCL, board.SDA)
 
# GAIN = 2
ads = ADS1115(i2c,2)
 
# Créer une instance d'entrée analogique
# et l'associer à la broche 0 du module ADC
data = AnalogIn(ads, P0)
 
# Lire la valeur numérique et le voltage
while True:
    print(data.value, data.voltage)
```

Attention: le gain correspond à une amplification, mais le voltage, lui, ne change pas, ni le fait que c'est toujours sur 16 bits que le signal est représenté. Donc, un gain de valeur 2 a pour effet de diviser par 2 le voltage qui correspond à la valeur maximum de 32767.

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

> Lorsque le gain augmente, la **précision** des valeurs lues augmente, mais l'écart entre les valeurs minimum et maximum diminue.

<!--
{{% notice info "Quelques exercices de compréhension" %}}
Attention: il n'y a pas de réponses précises à ces questions. Comme Le signal qu'on traite est analogique, il peut contenir du bruit, ce qui a pour effet de faire varier plus ou moins fortement les données lues. Rechercez donc des réponses approximatives.
1. Avec un gain de 1, quelle est la valeur maximale lue?
2. Avec un gain de 2, quelle est la valeur maximale lue?
3. Avec un gain de 4, 8 ou 16, à quel moment dans la rotation du bouton atteint-on la valeur maximale lue?

Maintenant branchez votre potentiomètre sur le courant 3.3V.
1. Avec un gain de 1, quelle est la valeur maximum lue?
2. Avec un gain de 2, quelle est la valeur maximum lue?
3. En expérimentant avec différents gains et en lisant les valeurs obtenues, quelle valeur de gain vous semble donner les meilleurs résultats (c'est-à-dire, les résultats les plus utilisables) lorsque le potentiomètre est branché sur 3.3V?
4. Afin de diminuer les variations entre chaque lecture, sauriez-vous faire un programme qui calcule la moyenne des 3 derniers résultats lus et affiche cette moyenne?
   
{{% /notice %}}

<!--
{{% expand "Réponses" %}}
5V:
1. 30000
2. 32767
3. Avant la fin

3.3V:
1. 20
2. 55
{{% /expand %}}
-->
