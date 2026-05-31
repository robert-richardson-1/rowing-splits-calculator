# SplitCalc — Rowing Goal Split Calculator

A coxswain/coaching tool for calculating goal splits based on river flow rate, direction, and boat history. Initially built for Notre Dame Men's Rowing.

---

## What it does

On any given practice day, a coach enters the current river flow (in CFS) and whether the boat is going upstream or downstream. The app returns a target split (min:sec per 500m) adjusted for those conditions.

The app supports three boat classes: **8+**, **4+**, and **4-**.

There are two ways it calculates a goal split:

**1. Calibration model (recommended)**
The most accurate method. Enter two steady-state sessions for a boat — each with an average upstream and downstream split at a known flow rate. The app solves for the boat's true flat-water speed and the river's effect per unit of flow, then applies those parameters to today's conditions exactly.

**2. History-based fallback**
If calibration data isn't available yet, the app uses logged split history to estimate a goal. It normalizes past splits to a flat-water equivalent, then adjusts for today's flow and direction. Less precise than the calibration model but useful on day one.

---

## How the calibration model works

The model assumes boat split follows:

```
split = v₀ + k × direction × flow
```

Where:
- `v₀` is the boat's flat-water speed (seconds per 500m)
- `k` is the river sensitivity (seconds per CFS)
- `direction` is `-1` downstream, `+1` upstream

Two sessions (each with an upstream and downstream split at a known flow) give four equations and solve the two unknowns exactly. The app solves each session independently and averages the results, reporting a residual to flag inconsistency between sessions.

---

## Setup

### 1. Database (Supabase)

The app uses [Supabase](https://supabase.com) as a shared backend so all coaches read from and write to the same data.

---

### 2. Hosting (GitHub Pages)

This is a single self-contained HTML file with no build step or dependencies.

---

## Usage

1. **Select a boat class** — 8+, 4+, or 4-
2. **Enter today's flow** in CFS (check your local river gauge)
3. **Select direction** — upstream or downstream
4. The goal split appears automatically once calibration or history data exists

### Adding calibration data

Under the **Calibration** section, enter two sessions for the selected boat:
- Session A: flow rate, downstream avg split, upstream avg split
- Session B: same fields at a different flow rate

The app solves the model as soon as all 6 values are filled in and saves automatically.

### Logging splits

Use the **+ Add entry** tab in the history panel to log individual splits with date, flow, direction, and split time. These inform the fallback model and build a historical record.

---

## Tech

- Vanilla HTML/CSS/JS — no framework, no build step
- [Supabase](https://supabase.com) for the shared database (free tier)
- [DM Sans + DM Mono](https://fonts.google.com) via Google Fonts
- Hosted via GitHub Pages
