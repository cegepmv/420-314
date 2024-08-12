+++
title = 'GPIO'
date = 2024-08-01T18:05:56-04:00
draft = false
weight = 31
+++

GPIO signifie "General Purpose Input/Output" et désigne la série de broches métalliques qui servent à envoyer ou recevoir des signaux électriques sur un microcontrôleur.

Le RaspberryPi en comprend 40:
+ 2 broches pour un courant de 3.3V
+ 2 broches pour un courant de 5V
+ 8 broches pour le courant négatif ("ground")
+ 28 broches génériques

![gpio-p1](/420-314/images/GPIO_P1.png)

> Parmi les 28 broches génériques, certaines donnent accès à des fonctionnalités plus spécifiques. Pour plus de détails: https://pinout.xyz/

Les broches sont numérotées, ce qui permet de les identifier dans les programmes qui utilisent le Pi pour contrôler des composantes électroniques. Mais attention: le numéro de broche ne correspond pas au numéro GPIO. Par exemple, la broche `29` est associée à `GPIO 5`. 

Dans un programme, en *python* ou dans n'importque quel autre langage, c'est le numéro du GPIO qui doit être utilisé (pas le numéro de la broche). Par exemple, dans le programme suivant, le nombre **16** réfère au numéro de GPIO, donc à la broche `36` du *RaspberryPi*:

```python
import pigpio
import time

pi = pigpio.pi()
pi.set_mode(16,pigpio.OUTPUT)

pi.write(16,1)
time.sleep(1)
pi.write(16,0)
```

## Références
+ GPIO: https://pinout.xyz/
+ Module *pigpio*: https://abyz.me.uk/rpi/pigpio/python.html
+ Modules Keystudio: https://docs.keyestudio.com/projects/KS0522/en/latest/KS0522.html

