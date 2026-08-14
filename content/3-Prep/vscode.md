+++
title = 'Vscode'
date = 2026-06-12T12:10:23-04:00
draft = false
weight = 32
+++


Pour travailler confortablement avec **MicroPython** sur un ESP32 depuis **VSCode**, l'extension incontournable est **Pymakr**. Elle gère la connexion, l'exécution du code à la volée et la synchronisation de tes fichiers.

Voici comment configurer VSCode sur Windows et Ubuntu (Linux)

---

## 🛠️ Étape 1 : Les prérequis sur le système

### Sur Windows

1. **Node.js :** L'extension Pymakr en a absolument besoin pour fonctionner. Télécharge et installe la version LTS sur le [site officiel de Node.js](https://nodejs.org/).
2. **Pilotes (Driver) :** Comme tu as dit l'avoir déjà installé, assure-toi simplement que ton ESP32 apparaît bien dans le **Gestionnaire de périphériques** sous l'onglet *Ports (COM et LPT)* (généralement nommé `CH340` ou `CP210x`).

### Sur Ubuntu (Linux)

1. **Node.js :** Installe-le via ton terminal :
```bash
sudo apt update
sudo apt install nodejs npm

```


2. **Droits d'accès au port série (Crucial sur Linux) :** Par défaut, Ubuntu bloque l'accès aux ports USB pour les utilisateurs standards. Tu dois t'ajouter au groupe `dialout` (ou `tty`), sinon VSCode ne pourra pas parler à l'ESP32.
Ouvre un terminal et tape :
```bash
sudo usermod -aG dialout $USER

```


⚠️ **IMPORTANT :** Tu *dois* redémarrer ton PC (ou te déconnecter/reconnecter de ta session) pour que ce changement de groupe soit pris en compte.

---

## 💻 Étape 2 : Configuration de VSCode (Identique sur Windows et Ubuntu)

1. Ouvre VSCode.
2. Va dans l'onglet **Extensions** (l'icône avec 4 carrés à gauche, ou `Ctrl+Shift+X`).
3. Recherche **Pymakr** (développée par Pycom) et clique sur **Installer**.
4. Une fois installée, une petite icône en forme d'éclair ou de carte électronique va apparaître dans ta barre latérale gauche.

---

## 🚀 Étape 3 : Créer ton premier projet MicroPython

Pour éviter les nœuds, suis cette structure simple :

1. Crée un dossier vide sur ton ordinateur nommé `MonProjetESP32`.
2. Ouvre ce dossier dans VSCode (`Fichier > Ouvrir le dossier`).
3. Clique sur l'icône **Pymakr** dans la barre latérale.
4. Dans la section **Projects**, clique sur **Create project** (ou initialise-le). Pymakr va générer un fichier de configuration (souvent nommé `pymakr.conf`).
5. Crée deux fichiers essentiels à la racine de ton dossier :
* `boot.py` : (Peut rester vide pour l'instant) C'est le fichier qui s'exécute en tout premier au démarrage de l'ESP32.
* `main.py` : C'est ici que tu mets ton code principal.



Ajoute ce code de test dans ton `main.py` pour faire clignoter la LED interne (la broche 0 sur nos ESP32) :

```python
import machine
import time

# La broche 2 contrôle souvent la LED bleue intégrée
led = machine.Pin(0, machine.Pin.OUT)

print("Démarrage du clignotement...")

while True:
    led.value(not led.value()) # Inverse l'état de la LED
    time.sleep(0.5)            # Attend 500ms

```

---

## ⚡ Étape 4 : Connexion et Flash

1. Branche ton ESP32 en USB.
2. Dans l'onglet Pymakr, sous **Devices**, ton ESP32 devrait être détecté automatiquement (il affichera le port COM sur Windows, ou `/dev/ttyUSB0` sur Ubuntu).
3. Clique sur l'icône de **Prise de courant** (Connect) à côté du nom de ta carte.
4. Une console (REPL) va s'ouvrir en bas. Tu es maintenant en direct dans l'ESP32 ! Tu peux taper `print("Hello")` pour tester.
5. Pour envoyer tout ton projet sur la carte, clique sur le bouton **Sync** (généralement une flèche vers le bas ou "Sync project" dans l'interface Pymakr).

L'ESP32 va redémarrer et ta LED devrait commencer à clignoter !


## Étape 5 

Dans votre projet à côter de votre ESP32 vous pouvez cliquer sur les "..." et configurer l'appareil. Ceci vous permet de changer son nom.

