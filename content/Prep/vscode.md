+++
title = 'VS Code'
date = 2024-08-01T13:50:14-04:00
draft = false
weight = 14
+++

Le programme Visual Studio Code peut utiliser le protocole SSH pour se connecter sur un hôte distant et modifier les fichiers qui s'y trouvent.

Dans ce qui suit nous allons configurer VSCode (sur un PC Windows) pour qu’il se connecte directement sur le RaspberryPi. C’est cette configuration qui sera utilisée en classe.

#### Branchement physique
Assurez-vous d'abord que le PC et le Pi sont tous deux connectés sur le réseau local du département.

IMAGE

#### Connexion SSH
Votre Pi est identifié par son _hostname_ suivi du domaine “.local”, par exemple `prof.local`. Pour vous connecter au serveur pour la première fois, cliquez sur l’élément suivant dans le coin inférieur gauche de VSCode:

![vscodeconnect](/420-314/images/vscodeconnect.png)

Ensuite, choisissez `Connect to Host...` puis `Add New SSH Host...` dans les options disponibles:

![newconnection](/420-314/images/newconnection.png)

Entrez ensuite la commande de connexion sur votre serveur. Par exemple pour l'utilisateur _pi_ sur le  serveur _prof.local_, la commande est `pi@prof.local`.

Enfin, sélectionnez le fichier de votre choix pour sauvegarder les informations de connexion. Vous pourrez ensuite vous connecter en choisissant votre Pi dans le menu des hôtes disponibles.

