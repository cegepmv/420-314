Voici une page de notes de cours au format Markdown pour Hugo, prête à être intégrée. Elle regroupe tous les shortcodes d'alertes, de blocs d'informations et d'encadrés spéciaux (souvent utilisés avec les thèmes Hugo populaires en éducation comme *Relearn*, *Learn*, *DocDock* ou *Book*).

Tu as juste à copier-coller ce bloc !

```markdown
+++
title = "Boîtes spéciales et Alertes"
date = 2026-06-17T15:00:00-04:00
draft = false
weight = 99
+++

Cette page sert de guide visuel pour l'utilisation des blocs d'alertes et des encadrés spéciaux dans vos notes de cours. Utilisez-les pour structurer vos laboratoires et attirer l'attention des étudiants.

---

## 1. Les Alertes Standard (Thème Relearn / Learn)

Ces boîtes utilisent la syntaxe standard des shortcodes Hugo pour formater du contenu important.

### Note / Information
Idéal pour ajouter des précisions, du contexte historique ou des détails théoriques secondaires.

{{% notice info %}}
**Le saviez-vous ?** L'ESP32 intègre nativement le Wi-Fi et le Bluetooth, contrairement au Raspberry Pi Pico standard. C'est ce qui en fait un choix incontournable pour les projets IoT (Internet des Objets).
{{% /expand %}}

### Astuce / Tip
Pour donner des raccourcis, des bonnes pratiques de code ou des astuces de débogage.

{{% notice tip %}}
**Astuce de pro :** En MicroPython, si votre script `main.py` boucle à l'infini et bloque la communication en série, vous pouvez envoyer un **Ctrl + C** dans la console REPL pour forcer l'arrêt du programme.
{{% /expand %}}

### Attention / Warning
Pour signaler un comportement inattendu, une erreur fréquente chez les étudiants ou une règle importante.

{{% notice warning %}}
**Attention aux broches !** Les broches de l'ESP32 fonctionnent en **3.3V**. Ne connectez jamais directement un signal de 5V sur une broche d'entrée sous peine de griller le microcontrôleur de façon permanente.
{{% /expand %}}

### Danger / Important
Pour tout ce qui touche à la sécurité du matériel (courts-circuits, surcharges), à la destruction de composants ou aux erreurs critiques.

{{% notice danger %}}
**DANGER DE SURCHAUFFE !** Ne reliez jamais directement la broche VBUS (5V) à la masse (GND). Vous risquez de détruire le port USB de votre ordinateur ou d'endommager la carte ESP32.
{{% /expand %}}

---

## 2. Blocs de Contenu Déroulants (Expand / Accordéon)

Très utile pour masquer les solutions de laboratoires ou les explications très denses afin de ne pas encombrer la page.

### Solution d'exercice

{{% expand "Cliquez ici pour voir la solution du défi" %}}
```python
from machine import Pin
# Code de solution
led = Pin(2, Pin.OUT)
led.value(1)

```

*Note aux étudiants : Essayez de le faire par vous-même avant de regarder la réponse !*
{{% /expand %}}

---

## 3. Citations et Mises en valeur (Blockquotes)

Pour mettre l'accent sur une définition importante ou une formule clé sans utiliser une boîte colorée agressive.

> **Définition — PWM (Pulse Width Modulation) :**
> Technique qui permet de simuler un signal analogique (comme une variation de tension pour changer la vitesse d'un moteur) à partir d'un signal numérique en faisant varier le rapport cyclique (*duty cycle*).

---

## 4. Raccourcis Clavier ou Éléments d'interface

Si vous devez guider les étudiants dans un logiciel (comme Thonny IDE) :

* Pour exécuter le script actuel, appuyez sur F5.
* Allez dans **Outils** → **Options** → **Interprète** pour sélectionner `MicroPython (ESP32)`.

```

```