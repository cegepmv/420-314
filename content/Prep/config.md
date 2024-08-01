+++
title = 'Configuration du Pi'
date = 2024-08-01T13:48:14-04:00
draft = true
weight = 11
+++

Le Raspberry Pi est fourni avec le système d'exploitation _RaspberryPi OS_ pré-installé.

Il y a tout de même quelques étapes de configuration à réaliser.

## Brancher les périphériques
Le Raspberry Pi est un ordinateur à part entière: on l'utilise souvent dans des projets d'objets connectés mais il est tout-à-fait possible d'y raccorder les périphériques habituels (clavier, souris et écran) qu'on retrouve sur les PC.

Le Pi de votre kit est dans un boîtier et est déjà connecté à un écran tactile. Vous pouvez ôter la plaque de derrière pour accéder aux broches GPIO:

IMAGE

Ces broches permettent de connecter les différentes composantes électroniques qui seront utilisées avec le Pi dans ce cours.


## Changer le _hostname_
La première chose à faire est de renommner votre _Pi_. Ceci lui donnera un nom unique qui permettra de l'identifier sur le réseau.

Par défaut, ce nom est “raspberrypi”. Choisissez un nom ayant de bonnes chances d’être unique sur le réseau, puis écrivez-le dans le fichier `/etc/hostname`:

Il faut aussi changer le nom associé à l'adresse `127.0.1.1` dans le fichier `/etc/hosts`:


## Inverser l'écran
Le boîtier utilisé fait que l'affichage apparaît à l'envers sur l'écran. Pour le remettre à l'endroit il faut apporter des modifications dans le fichier `/boot/config.txt`:
+ Ajouter la ligne `lcd_rotate=2`
+ Commentez la ligne `dtoverlay=vc4-fkms-v3d`

Redémarrez ensuite le Pi avec la commande `sudo restart now`.

## Connexion au réseau

## Activer pigpio

## Programme python avec thonny

