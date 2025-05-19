# 📚 MTG Decklist Cache

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

[English](https://github.com/barrins-project/mtg_decklist_cache/blob/main/README_EN.md) | [Français](https://github.com/barrins-project/mtg_decklist_cache/blob/main/README.md)


Un cache local des listes de decks **Magic: The Gathering** extraites depuis des sources publiques comme **MTGO** ou **MTGTop8**, au format JSON.
Ce dépôt centralise les données brutes avant leur traitement, enrichissement ou intégration dans une base de données.

---

## 📁 Organisation des fichiers

Chaque fichier JSON représente un tournoi. Les données sont organisées hiérarchiquement selon la **source**, l’**année**, le **mois** et le **jour** :

```

.
├── mtgo/
│   └── YYYY/
│       └── MM/
│           └── DD/
│               ├── event1.json
│               └── ...
├── mtgtop8/
│   └── YYYY/
│       └── MM/
│           └── DD/
│               ├── 1111\_event.json
│               └── ...

````

---

## 📦 Schéma des données

```ts
export type MTGScrape = {
  tournament: Tournament;
  decks: Deck[];
  rounds: Round[];
  standings: Standing[];
};

export type Tournament = {
  date: string; // ISO date
  name: string;
  url: string;
  format: string;
  players: number;
};

export type Deck = {
  date: string; // ISO date
  player: string;
  result?: number;
  anchor_uri: string;
  mainboard: CardEntry[];
  sideboard?: CardEntry[];
};

export type CardEntry = {
  count: number;
  name: string;
};

export type Round = {
  round_name: number;
  matches: Match[];
};

export type Match = {
  player_1: string;
  player_2: string;
  result: string;
};

export type Standing = {
  rank: number;
  player: string;
  points: number;
  wins: number;
  losses: number;
  draws: number;
  omwp: number;
  gwp: number;
  ogwp: number;
};
````

---

## 🧰 Utilisation

Ce dépôt est utilisé par :

* [`mtg_scraper`](https://github.com/barrins-project/mtg_scraper) — collecte et enregistrement des fichiers.
* [`mtg_viewer`](#) *(accès restreint)* — chargement et ingestion des fichiers JSON dans une base PostgreSQL.
* Des notebooks de data science — pour l’analyse du métagame, la classification d’archétypes, ou l’entraînement de modèles de ML.

---

## 🗃️ Bonnes pratiques

* Tous les fichiers doivent être encodés en **UTF-8**.
* Chaque fichier doit contenir un champ `decks`, même s’il est vide (`[]`).
* Le nom des fichiers doit être unique par tournoi.

---

## 🤝 Contribuer

Ce dépôt ne doit pas être modifié manuellement.
Les ajouts doivent passer par les outils automatisés (`mtg_scraper`) ou une PR coordonnée avec les mainteneurs du backend `mtg_viewer`.

---

## 📜 Licence

MIT — libre pour usage, adaptation ou extension.

---

**Projet parent :** [barrins-project](https://github.com/barrins-project)
💬 Pour toute question : GitHub Issues ou Discord privé du projet.
