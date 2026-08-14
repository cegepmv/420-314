+++
title = 'SSH'
date = 2024-08-01T13:49:54-04:00
draft = false
weight = 32
+++

## Connexion au réseau
Dans ce cours nous utiliserons souvent les PC du lab pour nous connecter sur le RaspberryPi. Pour ce faire, les deux hôtes doivent être connectés sur le même réseau local. 

Sur chacun des bureaux du local S-013 il y a 4 prises RJ-45. 

La première (en bleu) est reliée au réseau du cégep.

La 2e et la 3e (en jaune) sont reliées au réseau du département d'informatique. Connectez-y votre PC et votre Pi afin qu'ils puissent communiquer ensemble.

## Connexion au Pi
Chaque _RaspberryPi_ est nommé selon le numéro sur le boîtier: par exemple, si le numéro du boîtier est S011-22, le _hostname_ sera `rasp22`. 

Chaque _Pi_ a aussi un utilisateur nommé `pi` qui a les droits d'administrateur (root) du système. 

Le suffixe du réseau local est `.local`.

Ainsi, pour vous connecter par SSH sur le _Pi_ numéro 22 via le réseau local, la commande sera la suivante:

```
ssh pi@rasp20.local
```

## Accès de Windows avec _putty_

À partir d'un PC Windows, ouvrez une session sur votre Pi avec _putty_ (remplacez "prof.local" par le nom de votre _Pi_): 

![puttycnx](/420-314/images/puttycnx.png)

Si le service est correctement configuré, vous devriez avoir accès à la ligne de commande sur le Pi.
