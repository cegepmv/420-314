+++
title = 'Plaquette'
date = 2025-08-23T16:14:33-04:00
draft = false
weight = 20
+++

Une plaquette de prototypage, aussi appelée "breadboard", sert à faciliter l'assemblage des composantes électroniques lorsqu'on construit des circuits. Elle permet de connecter les circuits sans avoir à faire de soudures.

Dans une plaquette de prototypage, les points sont reliés entre eux comme dans le schéma suivant:

![bread1](/420-314/images/bread1.png?width=400px)

Tous les points d'une rangée "+" (en rouge) sont reliés entre eux, comme tous les points d'une rangée "-" (en noir). Les points a, b, c, d et e d'une rangée sont reliés; f, g, h, i et j aussi, etc.

Les rangées + et - sont celles où on connecte la source de courant. Lorsqu'on veut utiliser ce courant avec une composante, on branche la composante dans une rangée et on relie cette rangée avec un connecteur.

Ainsi, pour recréer sur une plaquette le circuit simple LED-résistance, on pourrait procéder comme suit:

![bbsimple](/420-314/images/bbsimple.png?width=800px)

Dans ce schéma, le courant électrique suit le chemin suivant:
+ Un fil relie le courant positif de la rangée "+" au connecteur *a-14*;
+ Puisque *a-14* est relié à *b-14*, le courant est envoyé dans la résistance;
+ Le courant suit le chemin entre *b-10* et *e-10* et alimente la LED;
+ Le courant suit le chemin entre *e-9* et *a-9*;
+ Un connecteur relie *a-9* et la rangée "-", qui retourne le courant dans le pôle négatif de la pile et complète le circuit.




