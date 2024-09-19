+++
title = 'PWM'
date = 2024-09-18T22:22:51-04:00
draft = false
weight = 51
+++

La modulation à largeur d'impulsion (MLI, ou en anglais **PWM** pour *Pulse Width Modulation*), est une technique très utilisée dans le domaine de l'électronique. Elle permet de contrôler la puissance délivrée à une composante électrique en modulant la durée des impulsions électriques. On s'en sert pour simuler un signal analogique, ou encore pour contrôler des servo-moteurs.
Dans cette section nous verrons comment fonctionne cette technique et comment la librairie ``pigpio`` permet d'utiliser la MLI pour simuler des signaux analogiques.

## Fonctionnement
Comme nous l'avons vu précédemment le Raspberry Pi ne peut envoyer que des signaux numériques. Avec les broches GPIO, il y a deux états possibles: soit elles envoient un courant, soit elles n'en envoient pas. Il n'est pas possible de faire varier le voltage sur celles-ci de façon continue.

Il en est de même pour la réception de signaux: les broches GPIO ne peuvent lire que les valeurs 0 ou 1.
Lorsqu'on dispose de senseurs qui envoient des signaux analogiques, comme un potentiomètre ou un détecteur de luminosité, il faut utiliser un convertisseur pour traduire le signal analogique en valeurs numériques. Mais cette traduction n'est possible que pour les signaux qui vont ***vers*** le Raspberry Pi.

La MLI permet de simuler des signaux analogiques envoyés ***à partir*** d'un Raspberry Pi vers un actuateur.
Le principe est assez simple. Si on a un courant de 5V (par exemple) et qu'on alterne ce courant rapidement avec un signal de 0V, la tension dans le circuit sera de 2.5V, car la moyenne entre les deux valeurs 0V et 5V est de 2.5V.

Dans cet exemple, on suppose que la durée de l'impulsion de 5V est égale à la durée du signal de 0V: c'est pourquoi la moyenne est de 2.5V. Mais cette moyenne dépend des durées relatives entre le signal et l'absence de signal. Pour une **période** de 100ms, une impulsion de 50ms correspond à 50% de la période; l'effet sur le voltage sera proportionnel: pour un courant de 5V, le résultat sera un tension de 50% de 5V, donc 2.5V. Pour la même période, une impulsion de 30ms correspond à 30% de la période, donc donnera un courant de 1.5V (30% de 5V); etc.

![pwm1](/420-314/images/pwm1.png)

On appelle *cycle de travail* (en anglais, "duty cycle") cette proportion entre la durée de l'impulsion et la durée de la période. Donc pour une tension de 5V, un cycle de travail de 20% donnera un signal de 1V (20% de la tension de référence de 5V), et ainsi de suite. Un élément important: la durée de la période n'a pas beaucoup d'importance ici, ce qui compte c'est la valeur du *duty cycle*.


## Exemple 
Dans ce programme on allume graduellement le module LED Keystudio.

On utilise la fonction `set_PWM_dutycycle()` du module *pigpio*. Elle prend 2 paramètres:
+ Le numéro GPIO de la broche
+ La valeur du cycle de travail.

Ici le **dutycycle** n'est pas un pourcentage mais plutôt une valeur comprise entre 0 et 255. Sur le Raspberry Pi, ils correspondent respectivement à 0V et 3.3V car le courant électrique qu'on peut envoyer via une broche GPIO a une tension de 3.3V.

### Connexions
+ LED: S sur broche 33 (GPIO13)
+ LED: V sur courant 3.3V
+ LED: G sur "ground"

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
-------------------------------

## Références utiles
- [U=RI | Qu'est-ce que la PWM?](https://www.youtube.com/watch?v=CSReyYwbGRY)
- [What is PWM?](https://www.youtube.com/watch?v=B_Ysdv1xRbA)
