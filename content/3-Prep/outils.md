+++
title = 'Outils et raccourcis'
date = 2026-11-17T10:31:00-05:00
draft = false
weight = 33
+++

Cette page rassemble les commandes utiles dans la console MicroPython de l'ESP32 et les réglages de VS Code utilisés avec Pymakr.

## Console MicroPython

| Raccourci | Action |
|---|---|
| `Ctrl+C` | Interrompt le programme en cours. |
| `Ctrl+D` | Effectue un redémarrage logiciel de l'ESP32. |
| `Ctrl+Shift+R` | Exécute le fichier ouvert avec Pymakr, si ce raccourci est configuré. |

## Raccourcis de VS Code

| Raccourci | Action |
|---|---|
| `Ctrl+K`, puis `Ctrl+S` | Ouvre la liste des raccourcis clavier. |
| `Ctrl+Shift+P` | Ouvre la palette de commandes. |
| `Ctrl+,` | Ouvre les paramètres de VS Code. |

Pour associer `Ctrl+Shift+R` à la commande `pymakr.runEditor`, ajoutez cette entrée au fichier `keybindings.json` de VS Code :

```json
[
    {
        "key": "ctrl+shift+r",
        "command": "pymakr.runEditor"
    }
]
```

## Réglages Pymakr

Pour désactiver la confirmation lors de l'exécution d'un script, ajoutez ce réglage aux paramètres JSON de VS Code :

```json
{
    "pymakr.projects.runScriptPrompt": false
}
```

## Profil du terminal Linux

Si vous souhaitez sélectionner Bash comme profil du terminal intégré, ajoutez ces réglages aux paramètres JSON de VS Code :

```json
{
    "terminal.integrated.profiles.linux": {
        "bash": {
            "path": "/usr/bin/bash",
            "icon": "terminal-bash"
        }
    },
    "terminal.integrated.defaultProfile.linux": "bash"
}
```

## Réinitialisation du mode de démarrage

Sur l'Arduino Nano ESP32, relier `B1` à `GND` puis appuyer sur `Reset` permet de passer en mode bootloader. Pour démarrer normalement, débranchez le fil et redémarrez la carte.

{{% notice style="warning" title="Attention" %}}
Le mode bootloader sert à installer ou récupérer le firmware; ce n'est pas une réinitialisation ordinaire. N'effacez pas la mémoire flash si vous souhaitez simplement redémarrer votre programme.
{{% /notice %}}
