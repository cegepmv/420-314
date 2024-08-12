+++
title = 'Service VNC'
date = 2024-08-01T13:49:33-04:00
draft = false
weight = 13
+++

VNC est un protocole qui permet d'accéder à distance à l'environnement graphique d'un hôte. Le **serveur** VNC doit s'éxécuter sur l'hôte où on veut se connecter; le **client** VNC doit s'éxécuter sur l'hôte à partir d'où on se connecte. 

Il faut donc lancer le service VNC sur votre _Pi_ et utiliser le client VNC sur le poste de travail Windows du lab.

Dans ce cours on utilisera les programmes _RealVNC_ comme client et serveur. 

#### Serveur VNC
Pour installer le serveur VNC sur votre _RaspberryPi_, lancez la commande suivante:

```
sudo apt install realvnc-vnc-server
```

Une fois le service installé, il faut activer l'interface:
+ Lancez la commande `sudo raspi-config` 
+ Choisissez `Interface options` -> `VNC` -> `Yes`


#### Client VNC
Le programme `VNC Viewer` installé sur les postes de travail Windows permet de se connecter au serveur VNC sur votre Pi:

![vncview](/420-314/images/vncview.png)

Au démarrage du programme, utilisez le nom d’hôte de votre Pi suivi de ".local" pour y établir une connexion. Par exemple, si le Pi a `prof` comme _hostname_, vous devez vous connecter sur `prof.local`:

![vncconf](/420-314/images/vncconf.png)

Vous devrez confirmer que vous souhaitez bien vous connecter en cliquant sur `Continuer`. Ensuite, il suffit d’entrer votre identifiant et mot de passe du Pi.