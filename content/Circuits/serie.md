+++
title = 'Circuits en série'
date = 2024-08-01T17:54:17-04:00
draft = false
weight = 22
+++

Dans cette section, nous verrons les règles qui mettent en relation la tension, l'intensité et la résistance, ce qui permet de calculer précisément le voltage ou la résistance requis dans un circuit.

Notre premier circuit est constitué d'une source de courant (par exemple, une pile) et d'une LED:

![circ1led](/420-314/images/circ1led.png?width=400px)

Le courant électrique est semblable au flot de l'eau dans un tuyau: il a une source, il "coule" dans un sens spécifique et la vitesse du courant dépend de la pression exercée. Pour que le courant s'écoule, le tuyau doit être ouvert; si on met un bouchon au bout du tuyau, le courant s'arrête.

Dans un circuit électrique, la source correspond au côté positif et le courant s'écoule vers le côté négatif. Si le circuit n'est pas connecté sur un pôle négatif, le courant ne circule pas. La "vitesse" du courant (son *intensité*) est proportionnelle à sa "pression" (sa *tension*).

Dans le circuit ci-haut, on inclut une LED. Dans cette LED le courant électrique doit y entrer par l'**anode**, qui correspond au courant positif, et sortir par la **cathode** qui correspond au courant négatif.

Les éléments d'un circuit électrique ne peuvent pas supporter n'importe quelle intensité de courant. En effet, une LED comme celle de l'exemple brûlera si on lui envoit un courant dont la tension est de 9V. 



![circ1leddone](/420-314/images/circ1leddone.png?width=400px)

#### Résistance
Afin d'atténuer l'intensité du courant et d'épargner notre LED, on ajoutera une **résistance** à notre circuit:

![circ1ledres](/420-314/images/circ1ledres.png?width=400px)

En démarrant la simulation, vous constaterez que la LED s'allume.

Pour continuer l'analogie du flot d'eau dans un tuyau, imaginez une résistance comme une partie du tuyau qui serait plus étroite: ceci diminuerait la puissance du flot dans toute la longueur du tuyau. Ce qui se passe dans notre circuit est similaire: peu importe où on met la résistance, l'intensité du courant diminue sur tout le circuit.

La résistance qu'on doit ajouter dépend de l'intensité initiale. En effet, une source de courant de 20V aura besoin d'une résistance plus "forte" pour empêcher la LED de brûler. À l'inverse, une source de 1.5V n'aura peut-être même pas besoin qu'on ajoute une résistance.

Dans TinkerCAD, lorsqu'on clique sur la résistance, ses spécifications s'affichent: 

![resspecs](/420-314/images/resspecs.png)

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

On sait que notre pile a 9v et que notre LED supporte un courant de 20mA (0,02A). Pour une avoir une intensité de 0,02A à partir de 9v, de quelle résistance a-t-on besoin? La formule (qui découle de la loi de Ohm) est la suivante:

> Résistance = Tension / Intensité

Notre résistance doit valoir 9 / 0,02, donc 450Ω.

L'équation permet donc de calculer n'importe quelle des 3 valeurs à partir des deux autres. Pour déduire l'intensité du courant lorsqu'on connaît sa tension et la valeur de la résistance, la formule est:

> Intensité = Tension / Résistance

Une manière simple de se souvenir de cette loi est le triangle suivant:

![triangleohm](/420-314/images/triangleohm.png?width=400px)


<!-- On peut laisser faire les schémas -->

#### Exercices:
+ 4 exercices théoriques sur la loi de Ohm
+ 2 exercices sur TinkerCAD et le vrai breadboard avec composantes simples (LED, bouton, potentiomètre)
