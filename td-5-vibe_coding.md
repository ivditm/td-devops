# Introduction

## 💡 C’est quoi le vibe coding ?

Le vibe coding est une nouvelle approche de développement logiciel assistance par intelligence artificielle où l’on génère du code à partir de requêtes en langage naturel, plutôt que d’écrire manuellement chaque ligne de code. Concrètement, on dit à un modèle d’IA ce que l’on veut (« crée-moi une API REST qui… »), et l’IA génère le code, que l’on peut ensuite tester et affiner.

---

# 📘 TD / TP – Vibe Coding & DevOps

**Licence 3 Informatique**

> 🎯 Positionnement :
> *Le vibe coding comme accélérateur, le DevOps comme garde-fou.*

---


## 🎯 Objectifs pédagogiques

À la fin de ce TD, vous serez capables de :

* Comprendre ce qu’est le **vibe coding**
* Utiliser une IA pour générer du code **de manière encadrée**
* Identifier les **manques DevOps** d’un code généré par IA
* Améliorer un projet via :

  * Dockerfile
  * CI/CD
* Comprendre le rôle du DevOps face à l’IA

---

## 🧪 Partie 1 – Génération de code (vibe coding)

### 1️⃣ Prompt fourni

Utilisez une IA pour générer une application backend simple :

> **Prompt :**
> *Génère une API Python simple avec Flask, avec :*
> *– un endpoint `/health`*
> *– un endpoint `/items` qui retourne une liste JSON*

📌 **Consigne**

* Copiez le code généré dans `app.py`
* Ne modifiez rien pour l’instant

---

## 🧪 Partie 2 – Analyse critique DevOps

### 2️⃣ Checklist DevOps

Analysez le code généré et complétez :

| Critère             | OK | KO |
| ------------------- | -- | -- |
| Tests automatisés   | ⬜  | ⬜  |
| Gestion des erreurs | ⬜  | ⬜  |
| Logs structurés     | ⬜  | ⬜  |
| Dockerisation       | ⬜  | ⬜  |
| CI/CD               | ⬜  | ⬜  |

📌 **Question**

> Pourquoi ce code ne peut-il pas être déployé tel quel en production ?

---

## 🧪 Partie 3 – Dockerfile généré par IA

### 3️⃣ Génération assistée

Prompt :

> *Génère un Dockerfile pour cette application Flask*

📌 Copiez le Dockerfile généré tel quel.

---

### 4️⃣ Analyse du Dockerfile

Répondez aux questions :

* L’image de base est-elle figée ?
* L’application tourne-t-elle en root ?
* Y a-t-il un `.dockerignore` ?
* Les dépendances sont-elles maîtrisées ?

---

### 5️⃣ Amélioration manuelle

Corrigez le Dockerfile pour :

* éviter `latest`
* utiliser un user non-root
* optimiser les couches

---

## 🧪 Partie 4 – CI/CD générée par IA

### 6️⃣ Pipeline CI

Prompt :

> *Génère un pipeline GitHub Actions pour tester et builder l’application*

📌 Analysez :

* Que fait réellement la CI ?
* Qu’est-ce qui manque ?

---

### 7️⃣ Amélioration CI

Ajoutez :

* installation des dépendances
* tests (même simples)
* build Docker

---

## 🧠 Conclusion TD

A vous de me le dire ? 
 (Lien du draft.io)[https://draft.io/b84z2jgjyxk27uws284rcqqng4cbddc4qr4gyxrpvryy]
---

