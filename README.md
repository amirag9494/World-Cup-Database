# World Cup Database & ETL Pipeline

A relational database and automated Bash ETL pipeline built with **PostgreSQL** and **Bash scripting** to store, normalize, and analyze match results from the final three rounds of the FIFA World Cup (2014 & 2018).

---

## Project Overview

This project automates the extraction, transformation, and loading (ETL) of raw match data from a CSV dataset into a normalized PostgreSQL schema, followed by analytical SQL reporting scripts.

### Key Components
- **`worldcup.sql`**: Full database dump containing table schemas, constraints, and populated records.
- **`insert_data.sh`**: Automated Bash ETL script that parses `games.csv`, deduplicates team entities, resolves foreign keys dynamically, and populates the database.
- **`queries.sh`**: Analytical reporting script executing SQL aggregations, joins, and filtering queries against the database.
- **`games.csv`**: Source dataset containing match metadata (year, round, teams, and scorelines).

---

## Database Schema

The database is normalized to eliminate data redundancy using two related tables:

### `teams`
| Column | Type | Constraints |
| :--- | :--- | :--- |
| `team_id` | `SERIAL` | `PRIMARY KEY` |
| `name` | `VARCHAR(50)` | `UNIQUE`, `NOT NULL` |

### `games`
| Column | Type | Constraints |
| :--- | :--- | :--- |
| `game_id` | `SERIAL` | `PRIMARY KEY` |
| `year` | `INT` | `NOT NULL` |
| `round` | `VARCHAR(50)` | `NOT NULL` |
| `winner_id` | `INT` | `FOREIGN KEY` → `teams(team_id)`, `NOT NULL` |
| `opponent_id` | `INT` | `FOREIGN KEY` → `teams(team_id)`, `NOT NULL` |
| `winner_goals` | `INT` | `NOT NULL` |
| `opponent_goals` | `INT` | `NOT NULL` |

---

## Setup & Usage

### 1. Restore the Database
Rebuild the `worldcup` database and populate all tables using the provided SQL dump:
```bash
psql -U postgres < worldcup.sql
