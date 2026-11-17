+++
title = 'Fonctionnement'
date = 2024-10-01T16:27:41-04:00
draft = false
weight = 61
+++

Contrairement à un moteur électrique ordinaire qui tourne continuellement lorsqu'il est alimenté, un servomoteur positionnel est conçu pour atteindre une position angulaire commandée. La course varie selon le modèle; plusieurs servomoteurs courants couvrent environ 0 à 180 degrés. On s'en sert par exemple pour contrôler l’ouverture de fenêtres, déplacer des bras robotisés, orienter des antennes ou des caméras de surveillance.

## Potentiomètre

Pour comprendre le fonctionnement d'un servo-moteur, il faut tout d'abord comprendre le fonctionnement d'un potentiomètre.

Dans la fiche technique d'un potentiomètre, vous trouverez toujours une valeur de résistance : dans celle du modèle [PTT-B1K](https://www.protechtrader.com/manuals/PTT-B1k-Rotary-Potentiometer-Datasheet.pdf) par exemple, elle est de 0 à 1000 ohms.

Un potentiomètre est une composante qui permet de faire varier la résistance d'un circuit électronique; c'est en fait une résistance qu'on contrôle manuellement.

Un potentiomètre a 3 broches: 2 sont utilisées pour le courant positif et négatif, et celle du milieu fournit le signal électrique au circuit.

Dans le circuit suivant, on voit comment connecter le potentiomètre, et on constate que la tension est à 1,5 V lorsque le potentiomètre est à la moitié :

![pot1](/420-314/images/potentiometer-1.png)

## Servo-moteur
Un servomoteur positionnel comprend généralement un moteur, un train d'engrenages, un potentiomètre de retour de position et un circuit de commande. Le potentiomètre mesure la position de l'axe; le circuit compare cette position à la consigne reçue et alimente le moteur pour réduire l'écart. Le potentiomètre ne sert donc pas simplement à diminuer le courant du moteur.

![servo1](/420-314/images/servo-1.png)

Les nombreux engrenages, qu'on voit bien sur l'illustration, réduisent la vitesse de rotation du moteur et augmentent le _couple_ disponible à l'axe de sortie.

## Références
- [Fonctionnement d'un potentiomètre](https://youtu.be/TepQsclEERA)
- [Fonctionnement d'un servo-moteur](https://youtu.be/fb8ZSe8fVIQ)
