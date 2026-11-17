+++
title = 'PWM'
date = 2024-09-18T22:22:51-04:00
draft = false
weight = 51
+++

La modulation à largeur d'impulsion (MLI, ou en anglais **PWM** pour *Pulse Width Modulation*), est une technique très utilisée dans le domaine de l'électronique. Elle permet de contrôler la puissance délivrée à une composante électrique en modulant la _durée_ des impulsions électriques. Dans ce cours, on l'utilisera de deux façons:
- Simuler un signal analogique
- Contrôler des servo-moteurs (Module 8)

Dans cette section nous verrons comment fonctionne cette technique et comment la librairie ``pigpio`` permet d'utiliser PWM pour simuler des signaux analogiques.

## Fonctionnement
Comme nous l'avons vu précédemment le Raspberry Pi ne peut envoyer que des signaux numériques. Avec les broches GPIO, il y a deux états possibles: soit elles envoient un courant, soit elles n'en envoient pas. Il n'est pas possible de faire varier le voltage sur celles-ci de façon continue.

Il en est de même pour la réception de signaux: les broches GPIO ne peuvent lire que les valeurs 0 ou 1.
Lorsqu'on dispose de senseurs qui envoient des signaux analogiques, comme un potentiomètre ou un détecteur de luminosité, il faut utiliser un convertisseur pour traduire le signal analogique en valeurs numériques. Mais cette traduction n'est possible que pour les signaux qui vont ***vers*** le Raspberry Pi.

PWM permet de commander un actuateur avec un signal numérique dont on fait varier le cycle de travail. Le signal alterne rapidement entre un niveau bas et un niveau haut; il ne devient pas pour autant une tension analogique continue. Sur un Raspberry Pi, les sorties GPIO sont à 3,3 V (et non à 5 V).

Dans un modèle idéal, la valeur moyenne du signal est proportionnelle au cycle de travail : pour un niveau haut de 3,3 V et un niveau bas de 0 V, un cycle de travail de 50 % donne une moyenne théorique de 1,65 V; 30 % donne 0,99 V. La tension réellement mesurée aux bornes d'une charge dépend du circuit et de la charge; une tension continue moyenne n'est obtenue que si le signal est filtré. Certains actuateurs, comme une LED ou un moteur, réagissent à la puissance moyenne sans qu'on ait besoin de transformer le PWM en tension continue.

![pwm1](/420-314/images/pwm-1.png)

On appelle *cycle de travail* (en anglais, "duty cycle") cette proportion entre la durée de l'impulsion et la durée de la période. Dans le modèle idéal, un cycle de travail de 20 % avec un niveau haut de 3,3 V correspond à une valeur moyenne de 0,66 V. La période compte aussi pour le comportement de la charge : il faut choisir une fréquence appropriée à l'actuateur.


## Exemple 
Dans ce programme on allume graduellement le module LED Keystudio.

On utilise la fonction `set_PWM_dutycycle()` du module *pigpio*. Elle prend 2 paramètres:
+ Le numéro GPIO de la broche
+ La valeur du cycle de travail.

Ici le **dutycycle** n'est pas un pourcentage mais plutôt une valeur comprise entre 0 et 255. Une valeur de 255 correspond à un cycle de travail de 100 % (niveau haut de 3,3 V); 128 correspond à environ 50 % et à une moyenne théorique de 1,65 V. La tension sur la charge n'est pas nécessairement cette valeur moyenne.

{{% notice style="green" title="Référence" %}}
Il est possible de changer cette plage de valeurs pour une broche GPIO avec la méthode `set_PWM_range()`. Si on souhaite utiliser des pourcentages au lieu d'une valeur entre 0-255, il s'agit de fixer le maximum à 100 comme suit:
```python
pi.set_PWM_range(26,100)
```
Voir https://abyz.me.uk/rpi/pigpio/python.html#set_PWM_range
{{% /notice %}}

### Programme
```python
import pigpio
import time

LED = 26
MAX = 255 # La valeur maximale que set_PWM_dutycycle() accepte
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)
cycle = 0 # Variable qui correspond à l'intensité (simulée) du voltage

try:
    while cycle < MAX:
        cycle += 1
        pi.set_PWM_dutycycle(LED,cycle)
        time.sleep(0.01)
    pi.write(LED,0)
    
except KeyboardInterrupt:
    pi.write(LED,0)
```

## Exercices
1. Modifiez le programme de l'exemple pour que la LED commence à 100% et diminue graduellement d'intensité ensuite.
2. Faites un programme qui fait augmenter puis diminuer graduellement la luminosité et continue cycliquement sans arrêter.
3. Faites un programme qui demande d'entrer un nombre de 0-100 au clavier et allume ensuite la LED au pourcentage de luminosité correspondant.

{{% expand "Solution 1." %}}
```python
...
cycle = 255
try:
    while cycle > 0:
        cycle -= 1
        pi.set_PWM_dutycycle(LED,cycle)
        time.sleep(0.01)
    pi.write(LED,0)
    
except KeyboardInterrupt:
    pi.write(LED,0)
```
{{% /expand %}}
{{% expand "Solution 2." %}}
```python
import pigpio
import time

LED = 26
MAX = 255 
pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)
cycle = 0 

try:
    while True:
        while cycle < MAX:
            cycle += 1
            pi.set_PWM_dutycycle(LED,cycle)
            time.sleep(0.01)
        while cycle > 0:
            cycle -= 1
            pi.set_PWM_dutycycle(LED,cycle)
            time.sleep(0.01)
    
except KeyboardInterrupt:
    pi.write(LED,0)
```
{{% /expand %}}
{{% expand "Solution 3." %}}
```python
import pigpio

LED = 26

pi = pigpio.pi()
pi.set_mode(LED,pigpio.OUTPUT)

pi.set_PWM_range(LED,100)
dc = input("SVP entrez une valeur de 0-100: ")
pi.set_PWM_dutycycle(LED,dc)
```
{{% /expand %}}

-------------------------------

## Références utiles
- [U=RI | Qu'est-ce que la PWM?](https://www.youtube.com/watch?v=CSReyYwbGRY)
- [What is PWM?](https://www.youtube.com/watch?v=B_Ysdv1xRbA)
