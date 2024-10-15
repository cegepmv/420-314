+++
title = 'Fonctionnement'
date = 2024-10-01T16:27:41-04:00
draft = false
weight = 61
+++

Contrairement à un moteur électrique ordinaire qui tourne tant qu'on lui envoit un courant électrique, Les servos-moteurs sont utilisés pour effectuer des mouvements angulaires. La plupart peuvent faire des rotations de 0 à 180 degrés. On s'en sert par exemple pour contrôler l’ouverture de fenêtres, déplacer des bras robotisés, orienter des antennes ou des caméras de surveillance, etc.

## Potentiomètre

Pour comprendre le fonctionnement d'un servo-moteur, il faut tout d'abord comprendre le fonctionnement d'un potentiomètre.

Dans la fiche techniques d'un potentiomètre, vous trouverez toujours une valeur de résistance: dans celle du modèle [PTT-B1K](https://www.protechtrader.com/manuals/PTT-B1k-Rotary-Potentiometer-Datasheet.pdf) par exemple, elle est de 0 à 1000 ohms.

Un potentiomètre est une composante qui permet de faire varier la résistance d'un circuit électronique; c'est en fait une résistance qu'on contrôle manuellement.

Un potentiomètre a 3 broches: 2 sont utilisées pour le courant positif et négatif, et celle du milieu fournit le signal électrique au circuit.

Dans le circuit suivant on voir comment connecter le potentiomètre, et on constate que le voltage est à 1,5V lorsque le potentiomètre est à la moitié:

![pot1](/420-314/images/pot1.png)

## Servo-moteur
On peut décrire simplement un servo-moteur comme un moteur ordinaire dans lequel on a intégré un potentiomètre.

La tige que le moteur fait tourner est rattachée à un potentiomètre. Ainsi, la rotation du moteur entraîne un mouvement du potentiomètre, ce qui augmente la résistance; la résistance qui augmente a pour effet de diminuer le courant envoyé au moteur, et cette diminution du courant fait que la rotation s'arrête. 

![servo1](/420-314/images/servo1.png)

Les nombreux engrenages, qu'on voit bien sur l'illustration, permettent de transformer la vitesse de rotation du moteur en _couple_ (la force de rotation de la tige du servo-moteur). La tige de rotation tourne moins vite que le moteur, mais sa "force" est plus grande.

## Références
- [Fonctionnement d'un potentiomètre](https://youtu.be/TepQsclEERA)
- [Fonctionnement d'un servo-moteur](https://youtu.be/fb8ZSe8fVIQ)
