+++
title = 'Moteurs DC'
date = 2025-09-23T09:20:48-04:00
draft = true
weight = 61
+++

![dcmotor3v](/420-314/images/dc-motor-3v.png)

## Fonctionnement
Les moteurs DC ("courant direct") sont les plus simples des moteurs électriques. Ils ont deux pôles (positif et négatif), et le sens de leur rotation change lorsqu'on inverse la polarité du courant sur ces pôles. 


## Contrôleur 
Un moteur DC ne doit pas être connecté directement aux broches GPIO d'un Pi : celles-ci fournissent des signaux logiques et ne sont pas conçues pour alimenter un moteur. Le courant nécessaire dépend du moteur et de la charge; il peut augmenter fortement au démarrage ou si le moteur bloque. Utilisez un pilote adapté et une alimentation moteur capable de fournir le courant requis, avec une masse commune au Pi.

Plusieurs composantes électroniques peuvent être utilisées à cette fin, mais une des plus populaires est le contrôleur L293D. 

![l293d](/420-314/images/l293d.png)

Le courant électrique destiné au moteur traverse ce circuit intégré. Lorsqu'on le connecte sur un Pi, il permet d'utiliser les GPIO comme des interrupteurs et aussi d'inverser facilement le sens de rotation du moteur.

Utiliser un contrôleur a d'autres avantages:
- Le moteur peut être alimenté par une source distincte de celle du Pi. Le L293D accepte une alimentation moteur pouvant atteindre 36 V; il faut également respecter les limites de courant et les autres spécifications de sa fiche technique.
- On peut connecter deux moteurs DC au contrôleur L293D.
- Il inclut des composantes qui diminuent le risque de dommages électriques.

### Broches
![l293dpins](/420-314/images/l293d-pinout.png)


## Exemple
Comme on le voit dans le programme suivant, on contrôle le sens de la rotation à partir des deux broches `INPUT`: inverser les valeurs inversera la rotation. Si les deux valeurs sont identiques, le moteur ne tournera pas (on recommande quand même de mettre les deux valeurs à 0 pour arrêter le moteur).  

La broche `ENABLE` active ou désactive le canal de sortie. Le moteur est alimenté par la broche `VS` du L293D; `VSS` correspond à son alimentation logique.

```python
import pigpio
import time

# Broches GPIO
EN1 = 16
IN1 = 20
IN2 = 21

# Initialisation
pi = pigpio.pi()
pi.set_mode(EN1,pigpio.OUTPUT)
pi.set_mode(IN1,pigpio.OUTPUT)
pi.set_mode(IN2,pigpio.OUTPUT)

try:
    # Sens de rotation
    pi.write(IN1, 0)
    pi.write(IN2, 1)

    # Envoyer le courant
    pi.write(EN1, 1)
    time.sleep(5)

finally:
    pi.write(IN1,0)
    pi.write(IN2,0)
    pi.write(EN1,0)
    pi.stop()
```

Pour exécuter ce programme, vous devez faire les connexions suivantes:

![dcl293d](/420-314/images/dc-l293d.jpg?width=400px&lightbox=true)

## Exercices
1. Faites un programme qui fait tourner le moteur 1 seconde dans un sens et 1 seconde dans l'autre sens, et qui se termine en arrêtant le moteur.
{{% expand "Solution" %}}
```python
import pigpio
import time

# Broches GPIO
EN1 = 16
IN1 = 20
IN2 = 21

# Initialisation
pi = pigpio.pi()
pi.set_mode(EN1,pigpio.OUTPUT)
pi.set_mode(IN1,pigpio.OUTPUT)
pi.set_mode(IN2,pigpio.OUTPUT)

try:
    # Sens de rotation
    pi.write(IN1, 0)
    pi.write(IN2, 1)

    # Envoyer le courant
    pi.write(EN1, 1)
    time.sleep(1)

    pi.write(IN1, 1)
    pi.write(IN2, 0)
    time.sleep(1)
    
finally:
    pi.write(IN1,0)
    pi.write(IN2,0)
    pi.write(EN1,0)
    pi.stop()
```
{{% /expand %}}

2. En utilisant la fonction ***set_PWM_dutycycle()***, démarrez le moteur à sa vitesse maximale puis diminuez-la de 10% à chaque seconde jusqu'à l'arrêt. 
{{% expand "Solution" %}}
```python
import pigpio
import time

# Broches GPIO
EN1 = 16
IN1 = 20
IN2 = 21

# Initialisation
pi = pigpio.pi()
pi.set_mode(EN1,pigpio.OUTPUT)
pi.set_mode(IN1,pigpio.OUTPUT)
pi.set_mode(IN2,pigpio.OUTPUT)

try:
    dc = 255

    pi.write(IN1, 0)
    pi.write(IN2, 1)

    while dc > 0:
        pi.set_PWM_dutycycle(EN1,dc)
        dc -= 25
        time.sleep(1)

finally:
    pi.write(IN1,0)
    pi.write(IN2,0)
    pi.write(EN1,0)
    pi.stop()
```
{{% /expand %}}

3. Changez le programme du numéro précédent pour qu'il commence à zéro et augmente sa vitesse de rotation de 10% à toutes les secondes. Que remarquez-vous?
