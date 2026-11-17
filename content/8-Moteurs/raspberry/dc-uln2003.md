+++
title = 'Moteurs DC'
date = 2025-01-27T16:59:50-05:00
draft = true
weight = 61
+++

![dcmotor3v](/420-314/images/dc-motor-3v.png)

## Fonctionnement
Les moteurs DC ("courant direct") sont les plus simples des moteurs électriques. Ils ont deux pôles (positif et négatif), et le sens de leur rotation change lorsqu'on inverse la polarité du courant sur ces pôles. 


## Contrôleur 
Un moteur DC ne doit pas être connecté directement aux broches GPIO d'un Pi : celles-ci fournissent des signaux logiques et ne sont pas conçues pour alimenter un moteur. Le courant nécessaire dépend du moteur et de la charge; il peut augmenter fortement au démarrage ou si le moteur bloque. Utilisez un pilote adapté et une alimentation moteur capable de fournir le courant requis, avec une masse commune au Pi.

Plusieurs composantes électroniques peuvent être utilisées à cette fin, mais une des plus populaires est le contrôleur ULN2003. 

![uln2003](/420-314/images/uln2003.png)

Le courant électrique destiné au moteur traverse ce circuit intégré. Lorsqu'on y connecte aussi une broche GPIO du Pi, celle-ci est utilisée pour servir d'interrupteur au circuit.

Utiliser un contrôleur a d'autres avantages:
- Le moteur peut être alimenté par une source distincte de celle du Pi. L'ULN2003A supporte une tension de sortie pouvant atteindre 50 V et un courant pouvant atteindre 500 mA par canal, sous réserve des limites thermiques du circuit et des conditions de sa fiche technique.
- Ses sept sorties peuvent commander des charges distinctes dans les limites du circuit.
- Ses diodes de protection intégrées peuvent protéger les sorties contre les surtensions de charges inductives lorsque la broche `Common` est raccordée correctement.

### Broches
![uln2003pins](/420-314/images/uln2003-pinout.png)

(Source: https://hackaday.com/2024/05/25/everything-you-ever-wanted-to-know-about-the-uln2003/)

Les broches **IN** servent à activer les interrupteurs à partir du GPIO.

Les broches **OUT** doivent être connectées sur le circuit électrique à contrôler.

Pour une charge inductive, la broche **Common** doit être raccordée au positif de l'alimentation de la charge afin d'utiliser les diodes de protection intégrées. Vérifiez le schéma de câblage du module utilisé.

La broche **GND** doit être reliée à la masse de l'alimentation et à celle du Pi.

## Exemple
Connectez le moteur et le contrôleur comme suit:

![dc_uln2003](/420-314/images/dc-uln2003.jpg)

Lancez ensuite le programme suivant (assurez-vous que le numéro de GPIO correspond bien à vos connexions):
```python
import pigpio
import time

MTR = 21
pi = pigpio.pi()
pi.set_mode(MTR,pigpio.OUTPUT)

pi.write(MTR,1)
time.sleep(2)
pi.write(MTR,0)
```
Si vos connexions sont bonnes, le moteur devrait tourner 2 secondes puis s'arrêter.

L'ULN2003 commande chaque sortie en marche/arrêt et peut aussi recevoir un signal PWM pour faire varier la puissance moyenne. Il ne permet pas d'inverser le sens d'un moteur DC : il ne constitue pas un pont en H. Pour commander les deux sens, utilisez plutôt le L293D présenté dans la [section précédente](../dc-l293d/).

```python
import pigpio
import time

# Broche GPIO reliée à une entrée de l'ULN2003
MTR = 21

# Initialisation
pi = pigpio.pi()
pi.set_mode(MTR,pigpio.OUTPUT)

try:
    pi.write(MTR, 1)
    time.sleep(5)
finally:
    pi.write(MTR,0)
    pi.stop()
```

## Exercices
1. Faites tourner le moteur pendant 1 seconde, arrêtez-le pendant 1 seconde, et répétez cette séquence trois fois. Le moteur tourne dans un seul sens.
{{% expand "Solution" %}}
```python
import pigpio
import time

MTR = 21
pi = pigpio.pi()
pi.set_mode(MTR,pigpio.OUTPUT)

try:
    for _ in range(3):
        pi.write(MTR,1)
        time.sleep(1)
        pi.write(MTR,0)
        time.sleep(1)
finally:
    pi.write(MTR,0)
    pi.stop()
```
{{% /expand %}}


2. En utilisant la fonction ***set_PWM_dutycycle()***, démarrez le moteur à sa vitesse maximale puis diminuez-la de 10% à chaque seconde jusqu'à l'arrêt. 
{{% expand "Solution" %}}
```python
import pigpio
import time

# Broches GPIO
MTR = 21

# Initialisation
pi = pigpio.pi()
pi.set_mode(MTR,pigpio.OUTPUT)

try:
    dc = 255
    while dc > 0:
        pi.set_PWM_dutycycle(MTR,dc)
        dc -= 25
        time.sleep(1)

finally:
    pi.write(MTR,0)
    pi.stop()
```
{{% /expand %}}

3. Changez le programme du numéro précédent pour qu'il augmente sa vitesse de rotation de 10% à toutes les secondes. Que se passe-t-il?

4. Cet exercice requiert un pont en H comme le L293D, et non l'ULN2003. Faites un programme basé sur le [serveur UDP](/420-410/sockets/tcp-udp/#serveur) qui reçoit des messages UDP au port 2112 pour contrôler le moteur. Les messages doivent contenir 2 informations séparées par `:`:
- `fwd` ou `rwd`: Tourner en sens horaire ou antihoraire.
- Un nombre qui représente le nombre de secondes avant que le moteur s'arrête.
- Par exemple, `rwd:4` fera tourner le moteur 4 secondes en sens antihoraire. 
{{% expand "Solution" %}}
import socket
import pigpio
import time

# Constantes
EN1 = 16
IN1 = 20
IN2 = 21
PORT = 2112
BUFFER_SIZE = 1024

# Fonctions
def off():
    pi.write(EN1,0)
    pi.write(IN1,0)
    pi.write(IN2,0)

def on():
    pi.write(EN1,1)
    pi.write(IN1,1)
    pi.write(IN2,0) 

def fwd():
    pi.write(IN1,1)
    pi.write(IN2,0)    

def rwd():
    pi.write(IN1,0)
    pi.write(IN2,1) 

# Init
pi = pigpio.pi()
pi.set_mode(EN1,pigpio.OUTPUT)
pi.set_mode(IN1,pigpio.OUTPUT)
pi.set_mode(IN2,pigpio.OUTPUT)
socket_local = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
adr_local = ('', PORT)
socket_local.bind(adr_local)

print(f"On attend les messages UDP entrants au port {PORT}...")

try:
    while True:
        buffer, adr_dist = socket_local.recvfrom(BUFFER_SIZE)
        message = buffer.decode()

        print(f"Message reçu: {message}", end='')

        sens, sec = message.split(":")
        
        # Traitement des données reçues
        on()
        if sens == "fwd": fwd()
        elif sens == "rwd": rwd()
        time.sleep(int(sec))
        off()

except ValueError:
    print("Erreur dans le message")
    off()

finally:
    off()
    socket_local.close()
{{% /expand %}}


<!-- POUR PLUS TARD, 
Faire une version ou les message est en bits sur un octet
bit 1: on/off
bit 2: fwd/rwd
bits 3-8: nombre de secondes (0-64)

avec fonction asynchrone pour arrêter le moteur si le bit 1 est à 0
-->