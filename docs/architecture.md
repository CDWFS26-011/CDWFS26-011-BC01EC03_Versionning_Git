# Architecture du projet MediSync

## Vue générale

MediSync est une application structurée selon une architecture modulaire visant à séparer clairement les responsabilités.

## Structure globale

Le projet est organisé en plusieurs couches :

- **Présentation (UI)** : interface utilisateur
- **Logique métier** : traitement des données et règles métier
- **Accès aux données** : communication avec les sources de données

## Organisation du dépôt
```
/docs
├── architecture.md
├── backlog.md
├── git-workflow.md
/src
├── modules/
├── services/
├── utils/
```


## Principe de conception

- Séparation des responsabilités
- Modularité des composants
- Code réutilisable et maintenable

## Objectif

Garantir une architecture simple, évolutive et facile à maintenir dans un contexte collaboratif.