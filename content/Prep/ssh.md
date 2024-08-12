+++
title = 'Service SSH'
date = 2024-08-01T13:49:54-04:00
draft = false
weight = 12
+++

## Installer SSH
Le service SSH est normalement installé par défaut sur _RaspberryPi OS_.

Pour rendre possible l’accès au Pi par SSH (avec _putty_ par exemple) il faut quand même s’assurer qu’il est lancé à chaque démarrage du Pi. Les commandes sont les suivantes:

```
sudo systemctl start ssh
sudo systemctl enable ssh
```

## Changer le MDP de l'utilisateur
> Cette étape n'est pas obligatoire, mais elle est recommandée pour des raisons de sécurité.

Afin d'éviter que tout le monde ait le même mot de passe par défaut, changez le mot de passe de l'utilisateur "pi" avec la commande suivante:

```
passwd 
```
 
## Accès de Windows avec _putty_
Puisque vous avez changé son nom d’hôte, votre _RaspberryPi_ devrait être joignable en utilisant ce nom suivi du suffixe `.local`. Donc si par exemple mon Pi a prof comme nom d’hôte, je dois utiliser `prof.local` pour m’y connecter.

À partir d'un PC Windows, ouvrez une session sur votre Pi avec _putty_: 

![puttycnx](/420-314/images/puttycnx.png)

Si le service est correctement configuré, vous devriez avoir accès à la ligne de commande sur le Pi.