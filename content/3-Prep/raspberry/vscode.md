+++
title = 'VS Code'
date = 2024-08-01T13:50:14-04:00
draft = false
weight = 34
+++

Le programme _Visual Studio Code_ peut utiliser le protocole SSH pour se connecter sur un hôte distant et modifier les fichiers qui s'y trouvent.

Dans ce qui suit nous verrons comment configurer VSCode pour qu’il se connecte directement sur le _RaspberryPi_. C’est cette configuration qui sera utilisée en classe.

#### Connexion SSH

Assurez-vous d'abord que le PC et le Pi sont tous deux connectés sur le réseau local du département.

Votre Pi est identifié par son _hostname_ suivi du domaine “.local”, par exemple `rasp22.local`. Pour vous connecter au serveur pour la première fois, cliquez sur l’élément suivant dans le coin inférieur gauche de VSCode:

![vscodeconnect](/420-314/images/vscodeconnect.png)

Ensuite, choisissez `Connect to Host...` puis `Add New SSH Host...` dans les options disponibles:

![newconnection](/420-314/images/newconnection.png)

Entrez ensuite la commande de connexion sur votre serveur. Par exemple pour l'utilisateur _pi_ sur le serveur _rasp22.local_, la commande est `pi@rasp22.local`.

Enfin, sélectionnez le fichier de votre choix pour sauvegarder les informations de connexion. Vous pourrez ensuite vous connecter en choisissant votre Pi dans le menu des hôtes disponibles.

