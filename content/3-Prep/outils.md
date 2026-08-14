+++
title = 'Outils'
date = 2026-06-12T16:47:54-04:00
draft = false
weight = 33
+++

Dans le terminal du ESP32:

ctrl + c : permet d'arrêter les processus en cours.

ctrl + d : permet d'effectuer un soft reset.

ctrl + shift + R : Permet d'exécuter le fichier en cours

Pour voir tous les raccourci de VsCode faites : ctrl + K & ctrl + S

ctrl + shift + P
{
        "key": "ctrl+shift+r",
        "command": "pymakr.runEditor",
}


ctrl + ,

"pymakr.projects.runScriptPrompt": false,

"terminal.integrated.profiles.linux": {
"bash": {
        "path": "/usr/bin/bash",
        "icon": "terminal-bash"
}
},
        "terminal.integrated.env.linux": {
        "LANG": "fr_FR.UTF-8",
        "LC_ALL": "fr_FR.UTF-8",
},


B1 + GND pour reset, vous devez réinstaller MicroPython ensuite