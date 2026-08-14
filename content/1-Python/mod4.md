+++
title = 'f-strings'
date = 2026-06-17T15:51:15-04:00
draft = false
weight = 14
+++

---


## 1. Introduction

Introduites en Python 3.6, les **f-strings** (ou *Formatted String Literals*) sont la méthode moderne, la plus rapide et la plus lisible pour intégrer des variables ou des expressions à l'intérieur d'une chaîne de caractères.

Pour transformer une chaîne normale en f-string, il suffit de placer la lettre `f` ou `F` juste **avant** les guillemets ouvrants.

```python
nom = "Alice"
# Chaîne classique
print("Bonjour " + nom) 

# f-string (plus Propre)
print(f"Bonjour {nom}")

```

---

## 2. Syntaxe de base et Expressions

Tout ce qui se trouve à l'intérieur des accolades `{}` est évalué comme du code Python. On peut y mettre des variables, mais aussi des calculs ou des appels de fonctions.

```python
a = 5
b = 10

# Calculs directs
print(f"La somme de {a} et {b} est {a + b}")
# Résultat: La somme de 5 et 10 est 15

# Appel de méthodes de chaînes
nom = "bob"
print(f"Bonjour {nom.capitalize()}")
# Résultat: Bonjour Bob

```

---

## 3. Le Formatage des Nombres

Les f-strings excellent dans la mise en forme esthétique des nombres (float, grandes valeurs, pourcentages). On utilise le symbole `:` suivi du code de formatage à l'intérieur de l'accolade.

### A. Limiter les décimales (`.Nf`)

Pour afficher un nombre flottant avec un nombre précis de chiffres après la virgule.

```python
pi = 3.14159265
print(f"Pi arrondi : {pi:.2f}")
# Résultat: Pi arrondi : 3.14

```

### B. Séparateur de milliers (`_` ou `,`)

Pour rendre les grands nombres lisibles.

```python
population = 8000000
print(f"Population : {population:_}")
# Résultat: Population : 8_000_000

```

### C. Convertir en Pourcentage (`.N%`)

Multiplie automatiquement par 100 et ajoute le symbole `%`.

```python
taux = 0.155
print(f"Le taux est de {taux:.1%}")
# Résultat: Le taux est de 15.5%

```

---

## 4. Alignement et Espacement

On peut forcer une variable à occuper un nombre fixe de caractères, ce qui est idéal pour créer des tableaux ou des reçus alignés dans la console.

* `<` : Aligner à **gauche**
* `>` : Aligner à **droite**
* `^` : **Centrer**

```python
item1 = "Pomme"
item2 = "Ordinateur portable"
prix1 = 1.50
prix2 = 1200.00

# On réserve 20 caractères pour le nom (aligné à gauche) et 8 pour le prix (aligné à droite)
print(f"{item1:<20}{prix1:>8.2f}$")
print(f"{item2:<20}{prix2:>8.2f}$")

# Résultat visuel parfaitement aligné :
# Pomme                  1.50$
# Ordinateur portable 1200.00$

```

---

## 5. Astuce de débogage rapide (`{variable=}`)

Depuis Python 3.8, ajouter un signe `=` après le nom de la variable dans l'accolade affiche automatiquement son nom **et** sa valeur. C'est un outil de débogage ultra-rapide.

```python
x = 42
y = "Test"

print(f"{x=}, {y=}")
# Résultat: x=42, y='Test'

```

---

## 6. Pièges courants et règles

* **Les guillemets internes :** Si votre f-string utilise des guillemets doubles, utilisez des guillemets simples à l'intérieur des accolades (comme pour les clés de dictionnaires).
```python
etudiant = {"nom": "Luc"}
# Correct :
print(f"L'étudiant est {etudiant['nom']}") 

```


* **Afficher de vraies accolades :** Si vous voulez afficher les caractères `{` ou `}` dans une f-string sans qu'ils soient interprétés, il faut les doubler (`{{` et `}}`).
```python
print(f"En f-string, on utilise {{}} pour les variables.")
# Résultat: En f-string, on utilise {} pour les variables.

```



---

### Résumé visuel des formats

| Syntaxe | Effet | Exemple (`val = 10.5`) | Résultat |
| --- | --- | --- | --- |
| `{val:.2f}` | Floats à 2 décimales | `f"{val:.2f}"` | `10.50` |
| `{val:>6}` | Aligner à droite (largeur 6) | `f"{val:>6}"` | `  10.5` |
| `{val:<6}` | Aligner à gauche (largeur 6) | `f"{val:<6}"` | `10.5  ` |
| `{val=}` | Débogage (nom + valeur) | `f"{val=}"` | `val=10.5` |