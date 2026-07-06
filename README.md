# GOAT Index — Messi vs Ronaldo

A dark-themed, data-driven comparison platform for Lionel Messi vs Cristiano
Ronaldo, built with React + Vite, Tailwind CSS v4, React Router, and Recharts.

## Run locally

```bash
npm install
npm run dev
```

Then open the printed local URL (usually http://localhost:5173).

To build for production:

```bash
npm run build
npm run preview
```

## Project structure

```
src/
  data/            # Clean, per-domain JSON extracted from the source dataset
    profiles.json
    careerTotals.json
    seasons.json
    competitions.json
    international.json
    trophies.json
    awards.json
    knockouts.json
    finals.json
    records.json
    ...
  lib/
    goatConfig.js  # Tunable category WEIGHTS for the GOAT Index (sum to 100)
    goatIndex.js   # Calculation engine — reads data + config, outputs scores
  components/      # Navbar, Footer, StatBar, RadarComparison, SeasonChart,
                   # TrophyTimeline, ScoreRing, CategoryCard
  pages/
    Home.jsx
    PlayerProfile.jsx   # shared, parameterized by player name + accent color
    Messi.jsx           # thin wrapper around PlayerProfile
    Ronaldo.jsx         # thin wrapper around PlayerProfile
    Compare.jsx         # head-to-head category breakdown + radar chart
    GoatIndexPage.jsx   # full methodology + final scores
```

## Tuning the GOAT Index

Open `src/lib/goatConfig.js` and adjust `GOAT_WEIGHTS` (must sum to 100):

```js
export const GOAT_WEIGHTS = {
  goals: 25,
  assists: 10,
  trophies: 25,
  bigGames: 15,
  consistency: 15,
  awards: 10,
};
```

Every score, chart, and category card recalculates automatically — no other
code changes needed.

## Swapping in your own dataset

The app reads exclusively from the JSON files in `src/data/`. To plug in a
different or updated dataset:

1. Keep the same file names and shape (each file is an array of objects keyed
   by `"Player": "Lionel Messi" | "Cristiano Ronaldo"`), **or**
2. Update the field references in `src/lib/goatIndex.js` and the page
   components to match your new schema.

To add a third player later, the cleanest path is to generalize
`relativeSplit`/`averageScores` in `goatIndex.js` from a 2-way split to an
N-way normalized score, and turn `PLAYERS` into a config-driven list.

## Tech stack

- React 19 + Vite
- Tailwind CSS v4 (via `@tailwindcss/postcss`)
- React Router v7
- Recharts (radar, line, area charts)
