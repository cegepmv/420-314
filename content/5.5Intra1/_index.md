+++
type = "chapter"
title = "Formatif intra 1"
date = 2026-06-12T16:54:59-04:00
draft = false
weight = 5.5
pre = ""
+++
---

### Examen - Projet 1 : Système de contrôle direct de LED par boutons


#### 1. Contexte et Objectif

Le but de ce projet est de concevoir un système interactif de base permettant de piloter des sources lumineuses de manière indépendante à l'aide de boutons poussoirs.

#### 2. Matériel à disposition

* 1 Microcontrôleur
* 2 Boutons poussoirs (A et B)
* 1 LED blanche (avec résistance de protection adaptée)
* 1 LED de signalisation
* Fils de câblage et plaque d'essai (breadboard)

#### 3. Travail demandé

Écrire un programme informatique permettant de réaliser le comportement suivant :

* Lorsque l'utilisateur appuie sur le **bouton A**, la **LED blanche** s'allume.
* Lorsque l'utilisateur appuie sur le **bouton B**, la **LED RGB** s'allume en couleur **verte**.
* Lorsque les boutons sont relâchés, les LED correspondantes doivent s'éteindre.

---

### Examen - Projet 2 : Interface de contrôle séquentiel et bascule

#### 1. Contexte et Objectif

On souhaite perfectionner le système précédent en introduisant la gestion d'états logiques (bascule) et l'utilisation de structures de données de type tableau pour la gestion des couleurs d'une LED RGB.

#### 2. Matériel à disposition

* Le même matériel que le Projet 1 (2 boutons, 1 LED blanche, 1 LED RGB).

#### 3. Travail demandé

Développer le programme en respectant les exigences fonctionnelles ci-dessous :

1. **Gestion du Bouton A (Interrupteur On/Off) :** Chaque pression sur le bouton A doit inverser l'état de la LED blanche (si elle était éteinte, elle s'allume ; si elle était allumée, elle s'éteint).
2. **Gestion du Bouton B (Défilement de couleurs) :** Déclarer un tableau contenant au moins 10 valeurs de couleurs RGB distinctes. Chaque pression sur le bouton B doit faire passer la LED RGB à la couleur suivante dans le tableau (avec un retour automatique au début du tableau après la dernière couleur).

---

### Examen - Projet 3 : Le jeu de réflexes interactif

**Matière :** Développement de Projets Interactifs

#### 1. Contexte et Objectif

Réaliser un mini-jeu de rapidité mesurant le temps de réaction entre deux joueurs ou évaluant la réactivité d'un utilisateur face à un stimulus visuel aléatoire.

#### 2. Matériel à disposition

* 2 Boutons poussoirs (Joueur A et Joueur B)
* 1 LED blanche
* 1 LED RGB

#### 3. Travail demandé


* Le système génère un temps d'attente aléatoire, au terme duquel la **LED blanche** s'allume brusquement.
* Le premier utilisateur à appuyer sur son bouton (A ou B) remporte la manche : la LED RGB s'allume alors de la couleur associée au gagnant (ex: Vert pour A, Bleu pour B) pendant que la LED blanche s'éteint.
* Prévoir une phase de réinitialisation automatique ou par un appui prolongé pour lancer une nouvelle manche.
* (indice : utilisez `time.ticks_ms()` qui est un compteur du temps en millisecondes depuis le démarage)
---

### Examen - Projet 4 : Simulateur de carrefour et contrôle d'accès


#### 1. Contexte et Objectif

Concevoir un système de contrôle de circulation combinant un feu tricolore et une gestion d'état sécurisée à l'aide de plusieurs entrées (boutons).

#### 2. Matériel à disposition

* 1 Boutons poussoirs (ex: Piéton)
* 1 LED blanche (Indicateur piéton / alerte)
* 1 LED RGB (Feu tricolore : Rouge, Orange, Vert)

#### 3. Travail demandé

La lumière passe du rouge au vert, puis au jaune en alternance. Si on clique sur le bouton, elle reste au rouge plus longtemps et le feu piéton blanc s'allume. Les lumières verte et rouge sont allumées normalement pendant 5 secondes, la jaune pendant 1 seconde, et la blanche pendant 5 secondes.
