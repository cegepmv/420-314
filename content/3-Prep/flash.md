+++
title = 'Flash'
date = 2026-06-12T11:31:37-04:00
draft = false
weight = 31
+++

### 1. Télécharger le firmware

Va sur le [site officiel](https://github.com/arduino/lab-micropython-installer/releases/tag/v1.4.0) de MicroPython et télécharge le fichier **`.bin`** pour l'Arduino Nano ESP32. Place-le dans ton dossier *Téléchargements*.


>[!IMPORTANT]
>Si l'étape 1 ne fonctionne pas, vous pouvez essayer les étapes suivantes sur Linux. J'ai testé sur Ubuntu et j'ajouterai éventuellement les étapes pour Windows. 
### 2. Créer l'environnement de flashage (Sandbox) sur Ubuntu

Ouvre un terminal et tape ces commandes pour installer l'outil officiel d'Espressif (`esptool`) sans casser ton système :

```bash
mkdir ~/esp_flash
cd ~/esp_flash
python3 -m venv espenv
source espenv/bin/activate
pip install esptool

```

### 3. Passer l'Arduino en "Mode Bootloader" (Mode Flash)

* Branche l'Arduino en USB.
* Relie la broche **`B1`** à la broche **`GND`** avec un petit fil ou un trombone.
* Appuie une fois sur le bouton **Reset** de la carte.
* L'Aduino doit s'allumer en **Vert/Violet fixe**. Tu peux enlever le fil.

### 4. Nettoyer et Installer

Dans ton terminal (toujours avec le préfixe `(espenv)` visible), tape :

```bash
# Libérer le port des services Linux (ModemManager)
sudo systemctl stop ModemManager

# Effacer la mémoire de la carte
esptool.py --port /dev/ttyACM0 erase_flash

```

Une fois que c'est écrit 100%, tape `deactivate` pour fermer la venv. Débranche et rebranche l'Arduino. Recommence l'étape 1.