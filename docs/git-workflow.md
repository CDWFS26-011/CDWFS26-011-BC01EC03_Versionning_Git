# Workflow Git

## Contrainte réseau — Port SSH 22 bloqué

Le port 22 (SSH) est bloqué sur le réseau de l’établissement. Git via SSH est donc indisponible.

**Solution : utilisation de HTTPS à la place de SSH.**

```bash
git remote set-url origin https://github.com/<username>/<repo>.git
git remote -v
git push origin main
```

Mise en cache des identifiants :

```bash
git config --global credential.helper store
```

> Avec l’authentification à deux facteurs, utilisation d’un **Personal Access Token (PAT)**.

---

## Modèle de branches — Stratégie hybride

```
main (production)
 └── develop (intégration)
      ├── feature/*
      ├── release/*
      └── hotfix/*
```

### Rôle des branches

| Branche     | Rôle                             |
| ----------- | -------------------------------- |
| `main`      | Version stable en production     |
| `develop`   | Intégration des développements   |
| `feature/*` | Développement de fonctionnalités |
| `release/*` | Préparation d’une version        |
| `hotfix/*`  | Corrections urgentes             |

---

## Cycle de développement

### Feature

```bash
git checkout develop
git checkout -b feature/architecture-doc

git commit -m "Add architecture overview"
git commit -m "Fix broken link in architecture doc"

git push origin feature/architecture-doc
```

→ Pull Request vers `develop`

---

### Release

```bash
git checkout develop
git checkout -b release/v1.0.0

git commit -m "Prepare release v1.0.0"
git push origin release/v1.0.0
```

→ Merge vers `main` et `develop`
→ Création d’un tag

---

### Hotfix

```bash
git checkout main
git checkout -b hotfix/fix-login-bug

git commit -m "Fix critical login bug"
git push origin hotfix/fix-login-bug
```

→ Merge vers `main` et `develop`

---

### Suppression des branches

```bash
git branch -d feature/architecture-doc
git push origin --delete feature/architecture-doc
```

---

## Protection des branches

### `main`

* PR obligatoire
* ≥ 1 approbation
* Pas de push direct

### `develop`

* PR recommandée
* Pas de push direct en équipe

---

## Convention de commits

* Messages en anglais
* ≤ 10 mots
* Verbe d’action
* 1 commit = 1 modification

```bash
git commit -m "Add architecture overview"
git commit -m "Fix broken link in backlog"
```

---

## Gestion des tags

### Création

```bash
git tag -a v1.0.0 -m "Release v1.0.0"
```

### Push

```bash
git push origin v1.0.0
git push origin --tags
```

### Suppression

```bash
git tag -d v1.0.0
git push origin --delete v1.0.0
```

### Bonnes pratiques

* Format **SemVer** : `vMAJOR.MINOR.PATCH`
* Tag uniquement sur `main`
* Correspond à une version stable

---

## Réponses aux questions d’évaluation

### T1 — Protocole standard

Le protocole standard utilisé par Git est **SSH (Secure Shell)**.
Il permet une communication sécurisée avec le dépôt distant via le port 22.

---

### T2 — Protocole de secours

Le protocole de secours est **HTTPS**, utilisant le port 443.
Il est utilisé lorsque SSH est bloqué, comme dans ce projet.

---

### T3 — Synchronisation multi-dépôts

Oui, Git permet de synchroniser un dépôt local avec plusieurs dépôts distants :

```bash
git remote add origin https://github.com/username/repo.git
git remote add mirror https://codeberg.org/username/repo.git
```

Ou :

```bash
git remote set-url --add --push origin <url1>
git remote set-url --add --push origin <url2>
```

---

### T4 — Effet de `git revert`

La commande `git revert` crée un nouveau commit qui annule les modifications d’un commit précédent.

* Le commit original est conservé
* L’historique n’est pas réécrit
* L’opération est traçable et sécurisée en équipe

---

### M1 — `merge` vs `rebase`

* `merge` conserve l’historique et ajoute un commit de fusion
* `rebase` réécrit l’historique pour le rendre linéaire

→ Le projet utilise **merge** pour garantir la sécurité en environnement collaboratif.

---

### M2 — Méthode alternative

La méthode alternative est **Git Flow**, qui introduit les branches :

* `develop`
* `release/*`
* `hotfix/*`

Elle permet une gestion plus structurée des versions, contrairement à GitHub Flow qui est plus simple.

---

### M3 — Objectif d’une Pull Request

Une Pull Request permet :

* la revue de code
* la validation via CI/CD
* la traçabilité des modifications
* le contrôle d’intégration

→ Elle garantit une intégration contrôlée et collaborative du code.

---

## Résumé

* Feature → `feature/*`
* Intégration → `develop`
* Release → `release/*`
* Production → `main`
* Correctif urgent → `hotfix/*`
