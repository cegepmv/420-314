+++
title = 'GPIO'
date = 2024-08-01T18:05:56-04:00
draft = false
weight = 41
+++

GPIO signifie "General Purpose Input/Output" et désigne la série de broches métalliques qui servent à envoyer ou recevoir des signaux électriques sur un microcontrôleur.

Le RaspberryPi en comprend 40:
+ 2 broches d'alimentation 3,3 V
+ 2 broches d'alimentation 5 V
+ 8 broches de masse ("ground")
+ 28 broches GPIO, dont les niveaux logiques sont de 3,3 V

Les limites de courant des broches d'alimentation et des GPIO dépendent du modèle de Raspberry Pi et du circuit. Ne considérez pas 16 mA comme un courant garanti par GPIO; consultez les spécifications de la carte et utilisez une résistance ou un circuit pilote adapté à la charge.

![gpio-p1](/420-314/images/gpio-p1.png)

> Parmi les 28 broches génériques, certaines donnent accès à des fonctionnalités plus spécifiques. Pour plus de détails: https://pinout.xyz/

Les broches sont numérotées, ce qui permet de les identifier dans les programmes qui utilisent le Pi pour contrôler des composantes électroniques. Mais attention: le numéro de broche ne correspond pas au numéro GPIO. Par exemple, la broche `29` est associée à `GPIO 5`. 

Dans un programme, en *Python* ou dans n'importe quel autre langage, c'est le numéro du GPIO qui doit être utilisé (pas le numéro de la broche). Par exemple, dans le programme suivant, le nombre **16** réfère au numéro de GPIO, donc à la broche `36` du *Raspberry Pi* :

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(16,pigpio.OUTPUT)

pi.write(16,1)
time.sleep(1)
pi.write(16,0)
```
Dans ce programme, on applique un niveau logique haut (environ 3,3 V) au GPIO 16 durant 1 seconde. Le courant fourni dépend de la charge connectée et doit respecter les limites de la carte.

{{% notice style="green" title="Référence" %}}
+ GPIO: https://pinout.xyz/
+ Module *pigpio*: https://abyz.me.uk/rpi/pigpio/python.html
{{% /notice %}}


