# 📚 MTG Decklist Cache

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

[Français](https://github.com/barrins-project/mtg_decklist_cache/blob/main/README.md) | [English](https://github.com/barrins-project/mtg_decklist_cache/blob/main/README_EN.md)

A local cache of **Magic: The Gathering** decklists extracted from public sources such as **MTGO** or **MTGTop8**, stored in JSON format.
This repository centralizes raw data before any processing, enrichment, or integration into a database.

---

## 📁 File Organization

Each JSON file represents a tournament. Files are organized hierarchically by **source**, **year**, **month**, and **day**:

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

## 📦 Data Schema

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

## 🧰 Usage

This repository is used by:

* [`mtg_scraper`](https://github.com/barrins-project/mtg_scraper) — to collect and store the files.
* [`mtg_viewer`](#) *(restricted access)* — to load and ingest JSON files into a PostgreSQL database.
* Data science notebooks — for metagame analysis, archetype classification, or machine learning training.

---

## 🗃️ Best Practices

* All files must be encoded in **UTF-8**.
* Each file must contain a `decks` field, even if it's empty (`[]`).
* File names must be unique per tournament.

---

## 🤝 Contributing

This repository is not meant to be edited manually.
All updates should go through the import tools (`mtg_scraper`) or via a PR coordinated with the maintainers of the `mtg_viewer` backend.

---

## 📜 License

MIT — free to use, adapt, or extend.

---

**Parent project:** [barrins-project](https://github.com/barrins-project)
💬 For any questions: GitHub Issues or the project's private Discord.

```
