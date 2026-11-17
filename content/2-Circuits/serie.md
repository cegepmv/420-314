+++
title = 'Circuits en série'
date = 2024-08-01T17:47:50-04:00
draft = false
weight = 21
+++

Nous utiliserons aussi [TinkerCAD](https://www.tinkercad.com/), un simulateur de circuits en ligne très utile pour tester des circuits sans risquer de les endommager.

## TinkerCAD
> Vous devez créer un compte personnel sur TinkerCAD pour l'utiliser.

Lorsque vous êtes connecté sur _TinkerCAD_, faites `Create`-> `Circuits` pour créer un circuit électronique.

À droite de l'espace de travail, vous verrez les différentes composantes disponibles (résistance, LED, bouton, etc.). Sélectionnez une pile 9V et une LED:

![batt-led](/420-314/images/batt-led.png?width=400px)

Ensuite, connectez les pôles de la pile à la LED. Le pôle positif doit être connecté à l'_anode_, et le pôle négatif à la _cathode_:

![batt-led-cn](/420-314/images/batt-led-cn.png?width=400px)

Pour tester votre circuit, cliquez sur le bouton `Start Simulation` en haut à gauche du plan de travail.

Vous verrez le message suivant:

![batt-led-br](/420-314/images/batt-led-br.png?width=400px)

Le courant qui traverse la LED est trop élevé : il faut ajouter une résistance en série pour le limiter.

Lorsque c'est fait, relancez la simulation; vous verrez que la LED ne "brûle" pas:

![batt-led-ok](/420-314/images/batt-led-ok.png?width=400px)

## Multimètre
Un multimètre est un instrument électrique utilisé pour mesurer les différentes propriétés des courants électriques et électroniques dans un circuit. Ils sont couramment utilisés par les électriciens, les électroniciens, les techniciens et les ingénieurs pour effectuer des tests et des mesures de tous genres dans le domaine de l'électricité et de l'électronique.

Un multimètre sert principalement à mesurer:
+ La tension (en volts)
+ L'intensité (en ampères)

### Mesurer la tension
Lorsqu'on utilise un multimètre pour mesurer la tension électrique, ce qu'on mesure est une *différence de tension* entre un pôle positif (+) et négatif (-).

Par exemple si on connecte un multimètre directement sur une pile, comme suit:
![multivpile](/420-314/images/multimeter-voltage-battery.png?width=400px)

On confirme que la tension entre les pôles + et - de la pile est de 9V, car le pôle positif est à 9V et le négatif à 0V.

Dans un circuit comprenant une LED et une résistance, on peut constater que la tension est aussi de (presque) 9V pour l'ensemble du circuit:

![multivcirctot](/420-314/images/multimeter-voltage-total-circuit.png?width=400px)

Notez ici que pour mesurer la tension de tout le circuit, il faut mesurer la différence en _volts_ entre le début et la fin du circuit. C'est pourquoi le multimètre est connecté sur les pôles de la pile, qui correspondent au début et à la fin du circuit électrique.

Il est possible de mesurer la tension à d'autres endroits dans le circuit. Dans l'exemple suivant, on mesure la différence de tension entre les deux pôles de la résistance:

![multivcircres](/420-314/images/multimeter-voltage-resistor.png?width=400px)

On peut faire la même chose pour la LED:

![multivcircled](/420-314/images/multimeter-voltage-led.png?width=400px)

On note que la différence de tension impartie par la résistance est de 7.04V et celle de la LED est de 1.95V.

On peut conclure les faits suivants:
+ La tension peut varier entre différents points dans un circuit
+ La somme des tensions des différents points dans un circuit correspond à la tension globale du circuit.


### Mesurer l'intensité
Lorsqu'on mesure l'intensité d'un courant électrique, il faut que celui-ci *traverse* le multimètre : ce n'est pas une différence qu'on mesure. Il faut ouvrir le circuit et insérer l'ampèremètre en série avec les composants.

Ne branchez jamais un multimètre en mode ampèremètre directement aux bornes d'une pile ou d'une source de tension : cela crée un court-circuit et peut endommager le multimètre ou la source.

Dans le circuit ci-dessous, le multimètre est branché en série avec la résistance :

![multiacircres](/420-314/images/multimeter-current-resistor-circuit.png?width=400px)

On constate que l'intensité du courant est de 15.4mA. Si on branche le multimètre à différents endroits du même circuit, on obtient les mêmes mesures:

![multiacircall](/420-314/images/multimeter-current-circuit-points.png?width=400px)

Dans un circuit en série en régime permanent, l'intensité du courant est la même en tout point de la branche.

### Utilisation d'un multimètre "réel"
Dans la section précédente on utilise des exemples de TinkerCAD, où le multimètre est très simplifié. Un multimètre réel a de nombreuses fonctionnalités, il est donc utile de voir ici comment on s'en sert.

Le cadran au centre permet de sélectionner la fonction qu'on veut utiliser:

![vraimulti](/420-314/images/real-multimeter.jpg)

Le symbole **⎓** désigne le courant direct (DC).

Le symbole **∿** désigne le courant alternatif (AC).

Le symbole **≃** désigne les deux types de courant (AC/DC).

Pour mesurer la tension:
+ Câble positif (rouge) dans **INPUT** 
+ Câble négatif (noir) dans **COM**
+ Cadran sur **V⎓ (DC)**

Pour mesurer la résistance:
+ Câble positif (rouge) dans **INPUT** 
+ Câble négatif (noir) dans **COM**
+ Cadran sur **Ω**
  
Pour mesurer l'intensité:
+ Câble positif (rouge) dans **mA** 
+ Câble négatif (noir) dans **COM**
+ Cadran sur **mA≃ (AC/DC)**


## Exercices
1. Quelle est l'intensité (en ampères) du courant fourni par une pile 9V ? 
{{% expand "Réponse" %}}
6 A
![multiex1](/420-314/images/multimeter-exercise-1.png?width=400px)
{{% /expand %}}

2. Montez le circuit suivant dans TinkerCAD (ne changez pas la valeur de la résistance):
![multiex2](/420-314/images/multimeter-exercise-2.png?width=400px)
Quelles sont les mesures d'intensité et de tension dans l'ensemble du circuit?
{{% expand "Réponse" %}}
1.16mA, 2.99V
![multiex2solV](/420-314/images/multimeter-exercise-2-solution-voltage.png?width=400px)
![multiex2solA](/420-314/images/multimeter-exercise-2-solution-current.png?width=400px)
{{% /expand %}}

3. Remplacez la LED par un moteur DC ("à courant continu"). Quelles sont les valeurs de tension et d'intensité?
{{% expand "Réponse" %}}
2.80mA, 2.97V
![multiex3solV](/420-314/images/multimeter-exercise-3-solution-voltage.png?width=400px)
![multiex3solA](/420-314/images/multimeter-exercise-3-solution-current.png?width=400px)
{{% /expand %}}

4. Faites un circuit avec une LED et un moteur DC en série. Quelles sont les valeurs de tension et d'intensité?
{{% expand "Réponse" %}}
1.09mA, 2.99V
![multiex4solV](/420-314/images/multimeter-exercise-4-solution-voltage.png?width=400px)
![multiex4solA](/420-314/images/multimeter-exercise-4-solution-current.png?width=400px)
{{% /expand %}}
