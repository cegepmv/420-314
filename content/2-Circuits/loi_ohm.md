+++
title = 'Loi de Ohm'
date = 2024-08-01T17:54:17-04:00
draft = false
weight = 22
+++

Dans cette section, nous verrons les règles qui mettent en relation la tension, l'intensité et la résistance, ce qui permet de calculer précisément le voltage ou la résistance requis dans un circuit.

Notre premier circuit est constitué d'une source de tension (par exemple, une pile) et d'une LED :

![circ1led](/420-314/images/circuit-1-led.png?width=400px)

Le courant électrique est semblable au flot de l'eau dans un tuyau: il a une source, il "coule" dans un sens spécifique et la vitesse du courant dépend de la pression exercée. Pour que le courant s'écoule, le tuyau doit être ouvert; si on met un bouchon au bout du tuyau, le courant s'arrête.

Dans un circuit fermé, le courant conventionnel circule de la borne positive vers la borne négative de la source. La "vitesse" du courant (son *intensité*) est proportionnelle à sa "pression" (sa *tension*).

Dans le circuit ci-haut, on inclut une LED. Dans cette LED le courant électrique doit y entrer par l'**anode**, qui correspond au courant positif, et sortir par la **cathode** qui correspond au courant négatif. 

> La "patte" la plus longue est le pôle **positif**.

{{% notice style="warning" title="Protéger la LED" %}}
Les éléments d'un circuit électrique ne peuvent pas supporter n'importe quelle intensité de courant. Une tension de 9 V appliquée directement à une LED, sans résistance de limitation, peut faire circuler un courant excessif et la détruire.
{{% /notice %}}

![circ1leddone](/420-314/images/circuit-1-led-done.png?width=400px)

#### Résistance
Afin d'atténuer l'intensité du courant et d'épargner notre LED, on ajoutera une **résistance** à notre circuit:

![circ1ledres](/420-314/images/circuit-1-led-resistor.png?width=400px)

En démarrant la simulation, vous constaterez que la LED s'allume.

Pour continuer l'analogie du flot d'eau dans un tuyau, imaginez une résistance comme une partie du tuyau qui serait plus étroite: ceci diminuerait la puissance du flot dans toute la longueur du tuyau. Ce qui se passe dans notre circuit est similaire: peu importe où on met la résistance, l'intensité du courant diminue sur tout le circuit.

La résistance qu'on doit ajouter dépend de la tension de la source, de la tension directe de la LED et du courant choisi. Une tension de source plus élevée nécessite généralement une résistance plus grande pour limiter le courant. Même avec une source de 1,5 V, il faut vérifier la tension directe de la LED et le courant avant de décider si une résistance est nécessaire.

Dans TinkerCAD, lorsqu'on clique sur la résistance, ses spécifications s'affichent: 

![resspecs](/420-314/images/resistor-specs.png)

Cette valeur est initialement de 1kΩ, soit 1000Ω. Le symbole **Ω** signifie "Ohm" et est l'unité qu'on utilise pour mesurer la résistance.

Si vous changez cette valeur pour 10Ω, vous verrez que la résistance est trop faible: la LED brûle. À partir de quelle valeur de résistance la LED s'allume-t-elle normalement?

#### Loi de Ohm
Il est possible de calculer cette valeur. La *loi de Ohm* est une formule qui met en relation les 3 valeurs suivantes:
+ Tension (en volts, *v*)
+ Résistance (en ohms, *Ω*)
+ Intensité (en ampères, *A*) 

La formule est la suivante:

> Tension = Intensité * Résistance

Ce qui signifie que pour une tension donnée, augmenter la résistance fait diminuer l'intensité du courant.

On suppose que la pile fournit 9 V, que la tension directe de la LED est de 2 V et que le courant visé est de 20 mA (0,02 A). Quelle résistance faut-il ajouter en série ? La formule (qui découle de la loi d'Ohm) est la suivante :

> Résistance = (Tension de la source - Tension directe de la LED) / Intensité

Notre résistance doit valoir (9 - 2) / 0,02, donc 350 Ω. On choisira une valeur normalisée appropriée, par exemple 360 Ω.

L'équation permet donc de calculer n'importe quelle des 3 valeurs à partir des deux autres. Pour déduire l'intensité du courant lorsqu'on connaît sa tension et la valeur de la résistance, la formule est:

> Intensité = Tension / Résistance

Une manière simple de se souvenir de cette loi est le triangle suivant:

![triangleohm](/420-314/images/ohms-law-triangle.png?width=400px)

--------------------
## Exercices
1. Une source de tension de 3 V alimente une LED dont la tension directe est de 2 V. Quelle résistance faut-il ajouter en série pour limiter le courant à 20 mA ?
{{% expand "Réponse" %}}
Résistance = (Tension de la source - Tension directe de la LED) / Intensité = (3V - 2V) / 0.02A = `50 Ohms`. On peut choisir une valeur normalisée de 51 Ω.
{{% /expand %}}

2. Une source de tension de 6 V alimente une LED dont la tension directe est de 2 V. Quelle résistance faut-il ajouter en série pour limiter le courant à 20 mA ?
{{% expand "Réponse" %}}
Résistance = (Tension de la source - Tension directe de la LED) / Intensité = (6V - 2V) / 0.02A = `200 Ohms`
{{% /expand %}}

3. Pour une source de tension de 6 V, quelle sera l'intensité si le circuit comprend une résistance de 500 ohms ?
{{% expand "Réponse" %}}
Intensité = Tension / Résistance = 6V / 500 Ohms = `12mA `
{{% /expand %}}

4. Pour une source de tension de 9 V, quelle sera l'intensité si le circuit comprend une résistance de 400 ohms ?
{{% expand "Réponse" %}}
Intensité = Tension / Résistance = 9V / 400 Ohms = `22.5mA` 
{{% /expand %}}

5. Quelle est la tension dans un circuit qui contient une résistance de 200 Ohms où l'intensité est mesurée à 30mA ?
{{% expand "Réponse" %}}
Tension = Intensité * Résistance = 0.03A * 200 = `6V`
{{% /expand %}}

6. Quelle est la tension dans un circuit qui contient une résistance de 500 Ohms où l'intensité est mesurée à 30mA ?
{{% expand "Réponse" %}}
Tension = Intensité * Résistance = 0.03A * 500 = `15V`
{{% /expand %}}
