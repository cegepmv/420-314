+++
title = 'Formatif intra 2'
date = 2026-06-14T11:32:05-04:00
draft = false
weight = 10
+++

---

### Question 1

* **Données :** $f = 50\text{ Hz}$ et $ct = 2\text{ ms}$.
* **Question :** Quel est le cycle de travail en pourcentage ($ct_{\%}$) ?

{{% expand title="Solution" %}}
    $$période = \frac{1}{f} = \frac{1}{50} = 20ms$$
    $$ct_{\%} = \frac{ct}{période} = \frac{2/1000}{20/1000} = 0.1 = 10{\%}$$
{{% /expand %}}

### Question 2

* **Données :** La pièce à laquelle tu transmets du courant accepte au maximum du $2\text{ V}$ (constant), ton Arduino envoie du $3{,}3\text{ V}$, et la période est de $50\text{ ms}$.
* **Question :** Quel cycle de travail maximal dois-tu appliquer en ms ($ct$ en ms) ?

{{% expand title="Solution" %}}
    $$ct_{\%} = \frac{Tension désirée}{Tension de sortie} = \frac{2}{3.3} = 0.606061 = 60.6061{\%}$$
    $$ct = période * ct_{\%} = 30.30ms$$
{{% /expand %}}

### Question 3

* **Données :** Un microcontrôleur alimenté en $5\text{ V}$ pilote une charge qui supporte au maximum une tension moyenne de $3\text{ V}$. La période du signal PWM est fixée à $20\text{ ms}$.
* **Question :** Quel doit être le temps à l'état haut ($ct$ en ms) pour ne pas dépasser cette limite de tension moyenne ?

{{% expand title="Solution" %}}
$$ct_{\%} = \frac{Tension désirée}{Tension de sortie} = \frac{2}{3.3} = 0.606061 = 60.6061{\%}$$
$$ct = période * ct_{\%} = 30.30ms$$
{{% /expand %}}

### Question 4

* **Données :** Sur un oscilloscope, le temps à l'état haut ($ct$ en ms) d'un signal PWM est mesuré à $0{,}8\text{ ms}$ pour une période totale de $4\text{ ms}$.
* **Questions :**
1. Quel est le cycle de travail en pourcentage ($ct_{\%}$) ?
2. Quelle est la fréquence ($f$) de ce signal ?

{{% expand title="Solution 1" %}}
$$ct_{\%} = \frac{ct}{période} = \frac{0.8 / 1000}{4 / 1000} = 20{\%}$$
{{% /expand %}}
{{% expand title="Solution 2" %}}
$$f = \frac{1}{période} = \frac{1}{4 / 1000} = 250 Hz$$
{{% /expand %}}

---
| Constante MicroPython | Atténuation | Plage de tension mesurable |
| --- | --- | --- |
| `ADC.ATTN_0DB` | 0 dB | 0.0V à 1.1V |
| `ADC.ATTN_2_5DB` | 2.5 dB | 0.0V à 1.5V |
| `ADC.ATTN_6DB` | 6 dB | 0.0V à 2.2V |
| `ADC.ATTN_11DB` | 11 dB | 0.0V à 3.6V (idéal pour le 3.3V) |
### Question 5

* **Données :** Vous souhaitez mesurer un capteur dont la tension de sortie maximale est de $1{,}8\text{ V}$ à l'aide d'une broche ADC d'un ESP32 en MicroPython.
* **Question :** Quelle constante d'atténuation devez-vous configurer pour pouvoir lire cette tension sans saturer le convertisseur, tout en choisissant la plage la plus adaptée ?

{{% expand title="Solution" %}}

ADC.ATTN_6DB (0 à 2,2V) est le choix idéal : elle est la plus petite qui couvre entièrement votre plage de $1{,}8\text{ V}$.
{{% /expand %}}

### Question 6

* **Données :** Vous configurez une broche ADC de votre ESP32 avec la ligne de code `adc.atten(ADC.ATTN_2_5DB)`.
* **Questions :**
Que se passe-t-il si vous appliquez accidentellement une tension de $2{,}0\text{ V}$ sur cette broche ?

{{% expand title="Solution" %}}

L'ADC atteint sa limite haute et sature. Il renverra systématiquement la valeur numérique maximale (soit 4095 pour une résolution de 12 bits) pour toute tension supérieure au seuil d'atténuation, rendant la lecture complètement fausse ou figée au maximum.
{{% /expand %}}
### Question 7

* **Données :** Un capteur analogique délivre un signal faible dont la tension varie strictement entre $0\text{ V}$ et $0{,}9\text{ V}$.
* **Questions :**
1. Quel est le niveau d'atténuation le plus optimal à utiliser?

{{% expand title="Solution" %}}
ADC.ATTN_0DB (0 à 1.1V) est le choix idéal : elle est la plus petite qui couvre entièrement votre plage de $0.9\text{ V}$.
{{% /expand %}}

---

### Question 8

* **Données :** Soit deux nombres représentés en binaire : $n_1 = 0b11001010$ et $n_2 = 0b10101100$.
* **Question :** Quel est le résultat de l'opération `n1 & n2` (ET logique) sous forme de chaîne binaire (`bin()`) ou de valeur décimale ?

{{% expand title="Solution" %}}
0b10001000
136
{{% /expand %}}

### Question 9

* **Données :** En reprenant les mêmes nombres de la question précédente ($n_1 = 0b11001010$ et $n_2 = 0b10101100$).
* **Question :** Quel est le résultat de l'opération `n1 ^ n2` (OU exclusif / XOR) ?

{{% expand title="Solution" %}}
0b01100110
102
{{% /expand %}}

### Question 10

* **Données :** Vous écrivez la ligne de code MicroPython suivante : `resultat = 24 << 3`.
* **Question :** Quel est le résultat décimal de cette opération, et à quelle multiplication par une puissance de $2$ cela correspond-il mathématiquement ?

{{% expand title="Solution" %}}
$$24 \times 2^3 = 24 \times 8 = 192$$
{{% /expand %}}

### Question 11

* **Données :** Vous souhaitez diviser un entier par $32$ en utilisant un opérateur de décalage de bits (*bit shift*) à partir du nombre $480$.
* **Questions :**
1. Quel opérateur de décalage (`<<` ou `>>`) devez-vous utiliser ?
2. De combien de positions (*n*) devez-vous décaler les bits pour effectuer cette division exacte ?

{{% expand title="Solution" %}}
1. **L'opérateur à utiliser :**
Vous devez utiliser l'opérateur de décalage vers la droite : **`>>`** (right shift).
2. **Le nombre de positions ($n$) :**
Vous devez décaler les bits de **$5$ positions** ($n = 5$).
{{% /expand %}}



---

# Cahier des charges : Jeu de devinette de mot (Potentiomètre & Matrice LED)

## 📌 Résumé global du projet

Le but de ce projet est de deviner un **mot secret** lettre par lettre à l'aide d'un dispositif physique :

1. **Sélection de la lettre :** On utilise un potentiomètre pour parcourir l'alphabet (de 'A' à 'Z'). La lettre sélectionnée s'affiche en temps réel sur une **matrice LED 8x8**.
2. **Indices de guidage :** Une **LED blanche** fournit des indices visuels (clignotement ou variation d'intensité) pour indiquer si l'on se rapproche ou si l'on a trouvé la bonne lettre par rapport à celle attendue dans le mot secret.
3. **Validation :** Lorsqu'on pense avoir trouvé la bonne lettre, on appuie sur un **bouton** pour valider. Le jeu confirme si la réponse est correcte, fait progresser le mot, ou indique une erreur.

---

## ⚙️ Spécifications fonctionnelles détaillées

### 1. Sélection de la lettre via le potentiomètre

* La plage de valeurs du potentiomètre est divisée en 26 portions égales (1/26e de la plage chacune).
* Chaque portion correspond à une lettre de l'alphabet, allant de **'A'** pour la valeur minimale jusqu'à **'Z'** pour la valeur maximale.

### 2. Optimisation de l'affichage (Matrice LED 8x8)

* Pour garantir la fluidité du programme et éviter le scintillement, l'affichage de la lettre sur la matrice LED ne se rafraîchit pas à chaque lecture brute du capteur, mais **uniquement lorsque la lettre active change**.

![matrice](/420-314/images/matrice_e2.png)


### 3. Système d'indication visuelle (LED blanche)

L'état de la LED blanche varie en fonction de la lettre actuellement sélectionnée par rapport à la lettre recherchée du mot secret :

* **Si la lettre affichée est la bonne :** La LED blanche clignote à une fréquence d'environ **10 fois par seconde**, de manière non bloquante.
* **Si la lettre affichée est incorrecte :** La LED s'allume avec une intensité proportionnelle à la proximité de la bonne lettre dans l'alphabet (plus l'écart alphabétique est faible, plus l'intensité lumineuse est forte).

### 4. Validation et interaction (Bouton poussoir)

* L'utilisateur peut appuyer sur le bouton à tout moment pour soumettre la lettre affichée :
* **En cas de bonne réponse :** Le jeu valide la sélection, l'ajoute aux lettres trouvées, passe à la lettre suivante du mot secret, et affiche un message de succès.
* **En cas de mauvaise réponse :** Le jeu affiche un message d'erreur et l'utilisateur peut continuer à chercher.

### 5. Fin de partie et gestion des erreurs

* **Condition de victoire :** Lorsque l'intégralité du mot secret est trouvée, la partie se termine, le mot complet est affiché, et tous les éléments visuels sont activés au maximum.
* **Sécurité matérielle :** En cas d'interruption du programme par l'utilisateur (interruption clavier / arrêt brusque), un nettoyage du matériel doit être exécuté pour éteindre proprement les LED et réinitialiser les composants.


# Code

Compléter ce code, vous ne pouvez pas modifier ce qui est déjà écrit, vous avez uniquement le droit de retirer les `pass`. Attention, je retirerai des points si l'expérience utilisateur est mauvaise (ex: indication pas claire)

```py
from machine import Pin, I2C, ADC, PWM
from time import sleep


MAX_DUTY = 1023 

led = PWM(Pin(Pin.board.A7, Pin.OUT))

btn = Pin(Pin.board.A6)

adc = ADC(Pin(Pin.board.A3))
adc.atten(ADC.ATTN_11DB)  

i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
adresse_matrice = 0x70  

i2c.writeto(adresse_matrice, bytes([0x21])) 
i2c.writeto(adresse_matrice, bytes([0x81]))
i2c.writeto(adresse_matrice, bytes([0xE4])) 

lettreMapping = {
    'A': [0b111, 0b101, 0b111, 0b101, 0b101],
    'B': [0b011, 0b101, 0b011, 0b101, 0b011],
    'C': [0b111, 0b001, 0b001, 0b001, 0b111],
    'D': [0b011, 0b101, 0b101, 0b101, 0b011],
    'E': [0b111, 0b001, 0b111, 0b001, 0b111],
    'F': [0b111, 0b001, 0b111, 0b001, 0b001],
    'G': [0b111, 0b001, 0b101, 0b101, 0b111],
    'H': [0b101, 0b101, 0b111, 0b101, 0b101],
    'I': [0b111, 0b010, 0b010, 0b010, 0b111],
    'J': [0b100, 0b100, 0b100, 0b101, 0b111],
    'K': [0b101, 0b011, 0b001, 0b011, 0b101],
    'L': [0b001, 0b001, 0b001, 0b001, 0b111],
    'M': [0b101, 0b111, 0b101, 0b101, 0b101],
    'N': [0b101, 0b111, 0b111, 0b101, 0b101],
    'O': [0b111, 0b101, 0b101, 0b101, 0b111],
    'P': [0b111, 0b101, 0b111, 0b001, 0b001],
    'Q': [0b111, 0b101, 0b101, 0b111, 0b100],
    'R': [0b111, 0b101, 0b011, 0b101, 0b101],
    'S': [0b111, 0b001, 0b111, 0b100, 0b111],
    'T': [0b111, 0b010, 0b010, 0b010, 0b010],
    'U': [0b101, 0b101, 0b101, 0b101, 0b111],
    'V': [0b101, 0b101, 0b101, 0b101, 0b010],
    'W': [0b101, 0b101, 0b101, 0b111, 0b101],
    'X': [0b101, 0b101, 0b010, 0b101, 0b101],
    'Y': [0b101, 0b101, 0b010, 0b010, 0b010],
    'Z': [0b111, 0b100, 0b010, 0b001, 0b111]
}

def set_white_brightness(brightness):
    """
    Modifie la luminosité de la led blanche

    Args:
        brightness (float):Valeur entre 0 et 1 [0,1] qui représente l'intensité de la lumière blanche en pourcentage.

    """
    pass


def fermeTout(matrice):
    """
    Ferme toutes les leds de la matrice 8x8

    Args:
        i2c (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
    """
    pass

def allumeTout(matrice):
    """
    Allume toutes les leds de la matrice 8x8

    Args:
        i2c (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
    """
    pass

fermeTout(i2c)


def afficherLettre(i2c, lettre, lettreMapping):
    """
    Affiche une lettre sur la led 8x8

    Args:
        i2c (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
        lettre (char): la lettre à afficher
        lettreMapping (dict): Le dictionnaire qui permet d'obtenir la représentation en binaire nécessaire pour allumer sur la led de chaque lettre 
    """
    
    pass

MOT_SECRET = "CHAT" 
index_lettre_actuelle = 0
lettres_trouvees = ["_"] * len(MOT_SECRET)
MAX_VALUE_ADC = 4095

print("\033[2J\033[H", end="") 
print("=== JEU : TROUVE LE MOT ===")
print("Tournez le potentiometre pour faire defiler les lettres (A-Z).")
print("Restez stable sur une lettre pour valider.")
print("Mot :", " ".join(lettres_trouvees), "\n")

last_lettre = ''
flash_toggle = False

cur_btn_state = btn.value()
last_btn_state = cur_btn_state


```

{{% expand title="Solution" %}}

{{% notice style="warning"%}}
Ce n'est qu'une seule des bonnes réponses possibles.
{{% /notice %}}

```py
from machine import Pin, I2C, ADC, PWM
from time import sleep, ticks_ms

MAX_DUTY = 1023 

led = PWM(Pin(Pin.board.A7, Pin.OUT))

btn = Pin(Pin.board.A6)

adc = ADC(Pin(Pin.board.A3))
adc.atten(ADC.ATTN_11DB)  

i2c = I2C(0, scl=Pin(Pin.board.A5), sda=Pin(Pin.board.A4))
adresse_matrice = 0x70  

i2c.writeto(adresse_matrice, bytes([0x21])) 
i2c.writeto(adresse_matrice, bytes([0x81]))
i2c.writeto(adresse_matrice, bytes([0xE4])) 

lettreMapping = {
    'A': [0b111, 0b101, 0b111, 0b101, 0b101],
    'B': [0b011, 0b101, 0b011, 0b101, 0b011],
    'C': [0b111, 0b001, 0b001, 0b001, 0b111],
    'D': [0b011, 0b101, 0b101, 0b101, 0b011],
    'E': [0b111, 0b001, 0b111, 0b001, 0b111],
    'F': [0b111, 0b001, 0b111, 0b001, 0b001],
    'G': [0b111, 0b001, 0b101, 0b101, 0b111],
    'H': [0b101, 0b101, 0b111, 0b101, 0b101],
    'I': [0b111, 0b010, 0b010, 0b010, 0b111],
    'J': [0b100, 0b100, 0b100, 0b101, 0b111],
    'K': [0b101, 0b011, 0b001, 0b011, 0b101],
    'L': [0b001, 0b001, 0b001, 0b001, 0b111],
    'M': [0b101, 0b111, 0b101, 0b101, 0b101],
    'N': [0b101, 0b111, 0b111, 0b101, 0b101],
    'O': [0b111, 0b101, 0b101, 0b101, 0b111],
    'P': [0b111, 0b101, 0b111, 0b001, 0b001],
    'Q': [0b111, 0b101, 0b101, 0b111, 0b100],
    'R': [0b111, 0b101, 0b011, 0b101, 0b101],
    'S': [0b111, 0b001, 0b111, 0b100, 0b111],
    'T': [0b111, 0b010, 0b010, 0b010, 0b010],
    'U': [0b101, 0b101, 0b101, 0b101, 0b111],
    'V': [0b101, 0b101, 0b101, 0b101, 0b010],
    'W': [0b101, 0b101, 0b101, 0b111, 0b101],
    'X': [0b101, 0b101, 0b010, 0b101, 0b101],
    'Y': [0b101, 0b101, 0b010, 0b010, 0b010],
    'Z': [0b111, 0b100, 0b010, 0b001, 0b111]
}

def set_white_brightness(brightness):
    """
    Modifie la luminosité de la led blanche

    Args:
        brightness (float):Valeur entre 0 et 1 [0,1] qui représente l'intensité de la lumière blanche en pourcentage.

    """
    led.duty(int(min(MAX_DUTY, max(0, brightness * MAX_DUTY))))


def fermeTout(matrice):
    """
    Ferme toutes les leds de la matrice 8x8

    Args:
        matrice (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
    """
    for row in range(0, 16, 2):
        matrice.writeto(adresse_matrice, bytes([row, 0]))

def allumeTout(matrice):
    """
    Allume toutes les leds de la matrice 8x8

    Args:
        matrice (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
    """
    for row in range(0, 16, 2):
        matrice.writeto(adresse_matrice, bytes([row, 0b11111111]))

fermeTout(i2c)


def afficherLettre(matrice, lettre, lettreMapping):
    """
    Affiche une lettre sur la led 8x8

    Args:
        i2c (I2C): Instance de la classe I2C représentant la matrice de leds 8x8 
        lettre (char): la lettre à afficher
        lettreMapping (dict): Le dictionnaire qui permet d'obtenir la représentation en binaire nécessaire pour allumer sur la led de chaque lettre 
    """
    mapping = lettreMapping[lettre]
    index = 0
    for row in [10, 8, 6, 4, 2]:
        matrice.writeto(adresse_matrice, bytes([row, mapping[index] << 2]))
        index += 1

MOT_SECRET = "CHAT" 
index_lettre_actuelle = 0
lettres_trouvees = ["_"] * len(MOT_SECRET)
MAX_VALUE_ADC = 4095

print("\033[2J\033[H", end="") 
print("=== JEU : TROUVE LE MOT ===")
print("Tournez le potentiometre pour faire defiler les lettres (A-Z).")
print("Restez stable sur une lettre pour valider.")
print("Mot :", " ".join(lettres_trouvees), "\n")

last_lettre = ''
flash_toggle = False

cur_btn_state = btn.value()
last_btn_state = cur_btn_state

lastLettreEnChiffre = -1 # Impossible
fermeTout(i2c)
# Avec ce code je trouve que A == 65
# for i in range(256):
#     print(f"{i} : {chr(i)}")
lastFlash = ticks_ms()
try:
    while index_lettre_actuelle < len(MOT_SECRET):
        cur_btn_state = btn.value()
        curLettreEnChiffre = int(max(0, min(25, (adc.read() / MAX_VALUE_ADC) * 26 ))  + 65)
        if curLettreEnChiffre != lastLettreEnChiffre:
            afficherLettre(i2c, chr(curLettreEnChiffre), lettreMapping)
        distLettre = abs(curLettreEnChiffre - ord(MOT_SECRET[index_lettre_actuelle]))

        if distLettre == 0:
            if ticks_ms() - lastFlash > 50:
                led.duty(0 if flash_toggle else MAX_DUTY)
                flash_toggle = not flash_toggle
                lastFlash = ticks_ms()
        else:
            led.duty(int(max(0, min(MAX_DUTY, MAX_DUTY * (1- distLettre / 26)))))

        if cur_btn_state != last_btn_state and cur_btn_state == 0:
            if distLettre == 0:
                lettres_trouvees[index_lettre_actuelle] = chr(curLettreEnChiffre)
                index_lettre_actuelle += 1
                print("\033[2J\033[H", end="") 
                print("=== JEU : TROUVE LE MOT ===")
                print(f"Felicitation, la lettre {chr(curLettreEnChiffre)} etait la bonne")
                print("Mot :", " ".join(lettres_trouvees), "\n")
            else:
                print("\033[2J\033[H", end="") 
                print("=== JEU : TROUVE LE MOT ===")
                print(f"Dommage, la lettre {chr(curLettreEnChiffre)} n'est pas celle qu'on cherche")
                print("Mot :", " ".join(lettres_trouvees), "\n")

        lastLettreEnChiffre = curLettreEnChiffre
        last_btn_state = cur_btn_state
        sleep(0.05)
    print("\033[2J\033[H", end="") 
    print(f"Felicitation, vous avez trouver le mot : {MOT_SECRET}")
except KeyboardInterrupt:
    led.duty(0)
    led.deinit()

    fermeTout(i2c)
```
{{% /expand %}}