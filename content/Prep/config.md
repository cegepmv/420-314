+++
title = 'Configuration du Pi'
date = 2024-08-01T13:48:14-04:00
draft = false
weight = 11
+++

Le Raspberry Pi est fourni avec le système d'exploitation _RaspberryPi OS_ pré-installé.

Il y a tout de même quelques étapes de configuration à réaliser.

## Brancher les périphériques
Le Raspberry Pi est un ordinateur à part entière: on l'utilise souvent dans des projets d'objets connectés mais il est tout-à-fait possible d'y raccorder les périphériques habituels (clavier, souris et écran) qu'on retrouve sur les PC.

Le Pi de votre kit est dans un boîtier et est déjà connecté à un écran tactile. Vous pouvez ôter la plaque de derrière pour accéder aux broches GPIO:

![broche](/420-314/images/prepBroche.png)

Ces broches permettent de connecter les différentes composantes électroniques qui seront utilisées avec le Pi dans ce cours.

Pour l'instant, connectez le clavier et la souris sur votre Pi et branchez-le pour le démarrer.

![assemblage](/420-314/images/prepAssemblage.png)


## Changer le _hostname_
La première chose à faire est de renommner votre _Pi_. Ceci lui donnera un nom unique qui permettra de l'identifier sur le réseau.

Par défaut, ce nom est “raspberrypi”. Choisissez un nom ayant de bonnes chances d’être unique sur le réseau, puis écrivez-le dans le fichier `/etc/hostname`:

Il faut aussi changer le nom dans le fichier `/etc/hosts`, qui est associé à l'adresse locale `127.0.1.1` :

![etchosts](/420-314/images/etchosts.png)

> Attention, le nom ne changera pas tant que vous n'aurez pas redémarrez le Pi. Pour ce faire vous pouvez lancer la commande `sudo reboot now`.

## Inverser l'écran

Le boîtier utilisé fait que l'affichage apparaît à l'envers sur l'écran. Pour le remettre à l'endroit il faut changer la configuration dans les préférences:

![screenconf1](/420-314/images/screenconf1.png?width=600px)

Ensuite, changez l'orientation pour "Inverted":

![screenconf](/420-314/images/screenconf2.png?width=600px)
## Connexion au réseau
Dans ce cours nous utiliserons souvent les PC du lab pour nous connecter sur le RaspberryPi. Pour ce faire, les deux hôtes doivent être connectés sur le même réseau local. 

Sur chacun des bureaux du local S-013 il y a 4 prises RJ-45. 

La première (en bleu) est reliée au réseau du cégep.

La 2e et la 3e sont reliées au réseau du département d'informatique. Connectez-y votre PC et votre Pi afin qu'ils puissent communiquer ensemble.

IMAGE

## Environnement de développement
Avant d'installer des logiciels, mettez votre système d'exploitation à jour:
```
sudo apt update
sudo apt upgrade
```

#### _pigpio_
Le langage python sera utilisé dans ce cours.

La librairie python qui permet de communiquer avec le GPIO du _RaspberryPi_ est "pigpio". Pour l'installer, lancez la commande suivante sur votre Pi:

```
sudo apt install pigpio
```

Cette librairie a besoin que le service _**pigpiod**_ s'exécute sur le _RaspberryPi_. Pour démarrer le service:
```
sudo systemctl start pigpiod
```

Pour que le service démarre automatiquement avec le *RaspberryPi*, la commande est celle-ci:
```
sudo systemctl enable pigpiod
```

Lorsque vous êtes connecté directement sur le _RaspberryPi_, il y a deux façons de développer en python.

#### nano
La première consiste à utiliser l'éditeur de texte **nano** sur la ligne de commande. Par exemple, créez un fichier nommé _hello.py_ avec la commande suivante:

```
nano hello.py
```
Ajoutez ensuite l'instruction suivante dans votre programme:

![nano](/420-314/images/nano.png)

Sauvegardez ensuite votre fichier (`CTRL-S` + `CTRL-X`), puis exécutez-le comme suit:

```
pi@demo:~ $ python hello.py
hello
```

#### Thonny
Le système d'exploitation _RaspberryPi OS_ inclut le programme _Thonny_, un environnement de développement simple pour python.

![thonny](/420-314/images/thonny.png)

Celui-ci dispose des fonctionnalités essentielles d’un IDE de base: débogueur, points d’arrêts et une console au bas de l’interface. Pour exécuter un programme, cliquez sur _Run_:

![thonnyexec](/420-314/images/thonnyexec.png)

## Programmes complémentaires
#### Clavier tactile
Si vous souhaitez utiliser un clavier tactile à l’écran, vous devez installer _matchbox-keyboard_, comme suit:
```
sudo apt install matchbox-keyboard
```
Ensuite pour activer le clavier tapez la commande `matchbox-keyboard` ou exécutez-le à partir de l'item "Accessoires" dans le menu de démarrage