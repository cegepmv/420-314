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

## Créer un nouvel utilisateur
Afin que chaque utilisateur du _RaspberryPi_ ait son propre compte et puisse travailler sur ses propres fichiers, vous devez vous créer un utilisateur et vous assurer qu'il peut utiliser la commande `sudo`. Les commandes `adduser` et `usermod` sont nécessaires; par exemple si vous voulez créer un utilisateur nommé "bob", faites ceci:
```
sudo adduser bob
[Répondez aux questions]
sudo usermod -aG sudo bob
```

Vous pouvez maintenant ouvrir une session avec l'utilisateur que vous venez de créer. 
 
## Accès de Windows avec _putty_
Puisque vous avez changé son nom d’hôte, votre _RaspberryPi_ devrait être joignable en utilisant ce nom suivi du suffixe `.local`. Donc si par exemple mon Pi a prof comme nom d’hôte, je dois utiliser `prof.local` pour m’y connecter.

À partir d'un PC Windows, ouvrez une session sur votre Pi avec _putty_: 

![puttycnx](/420-314/images/puttycnx.png)

Si le service est correctement configuré, vous devriez avoir accès à la ligne de commande sur le Pi.

{{% notice warning "ATTENTION" %}}
Il est possible que le suffixe `.local` ne fonctionne pas. Si c'est le cas, vous devez utiliser l'adresse IP du _RaspberryPi_ pour vous connecter.
{{% /notice %}}