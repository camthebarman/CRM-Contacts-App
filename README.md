# Camma

A personal CRM that runs entirely in your browser. No accounts, no server, no
tracking. Everything you type stays on your phone.

## What's in here

| File | What it is |
| --- | --- |
| `index.html` | The whole app: layout, styling and code, all commented |
| `manifest.webmanifest` | Lets you add Camma to your home screen |
| `icon-192.png`, `icon-512.png` | The home-screen icon |

There is no build step. Open `index.html` and it runs.

## Screens

- **Home** — a Today panel, a search bar, and your contacts in ranked order.
- **Contact** — log an interaction in one tap, add notes, see everything you know.
- **Add / Edit** — every field is optional except the name.
- **Settings** — export, import, load sample data.

## How ranking works

Set at the top of `index.html`, so you can change it:

```js
var TYPE_WEIGHTS = { "In person": 3, "Call": 2, "Text": 1, "Other": 1 };
var HALF_LIFE_DAYS = 14;
var TOP_SLOTS = 15;
```

1. Every interaction scores `weight x 0.5^(daysAgo / 14)` — worth half as much
   after two weeks, a quarter after four. A contact's score is the sum.
2. Sort by score, highest first.
3. Apply floor ranks, lowest floor number first. A floor is a promise of "never
   lower than this spot". People can still rank higher. Two people sharing
   floor 4 land at 3 and 4 at worst.
4. Positions 1–15 keep that order. From 16 down, sort only by who you spoke to
   most recently. People with no interactions go last, alphabetically.

Because of step 4, **a floor rank above 15 has no effect** — the tail is
re-sorted by recency.

## Backups

Camma stores data in this browser only. Clearing your browser data deletes it.
Settings → **Export** downloads a JSON file with everything in it; **Import**
puts it back. Home shows how long it's been since your last export.

Export on the old phone, import on the new one — that's how you move devices.

## Trying it out

Settings → **Load sample data** creates 20 made-up contacts with varied history
and a few floor ranks, so you can watch the ranking behave. It replaces whatever
is currently stored, so export first if you have real data.
