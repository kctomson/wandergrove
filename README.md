# 🌿 Wandergrove

*A quiet park. Curious critters. One trusty Glowseed.*

Wandergrove is a cozy critter-catching mini-game that ships as a **single,
dependency-free HTML file** — all the art, audio, physics, UI, and save data live
inside `index.html`. No build step, no server, no network access required.

The layout is responsive and safe-area aware, so it plays nicely in a narrow
desktop window or on a phone, with mouse or touch.

## 🎮 How to play

1. **Critters wander the park on their own** — tap one to walk up close.
2. **Press anywhere in the lower half**, then **flick to toss** — the Glowseed
   flies exactly where you flick it, like a real throw. There is no auto-aim:
   a gentle lob falls short, a hard flick flies fast.
3. **Watch the white ring** shrink over the critter. Hitting while it is small
   earns an **EXCELLENT** or **GREAT** bonus to your catch odds.
4. **POWER matters.** The badge on each critter hints how stubborn it is —
   higher POWER, more resistance.

If the critter breaks free, the Glowseed drops back — grab it and try again.
Caught critters are logged in the **Journal** (the book icon at the bottom-left),
which tracks how many of the 10 species you have met, your best POWER per
species, and your total tosses. Progress persists between sessions.

## 🐾 The critters

Ten species share the park. Up to six wander at once (never more than two of a
kind), spawn-weighted by rarity, and each individual rolls its own POWER value,
a subtle hue shift, and a size variation.

| Species  | Tag      | Rarity    | Diff | POWER range | Movement            |
| -------- | -------- | --------- | ---- | ----------- | ------------------- |
| Pecklet  | Chirp    | Common    | 1    | 46–76       | hops                |
| Woolby   | Fluff    | Common    | 1    | 58–92       | hops                |
| Puddlin  | Dewdrop  | Common    | 2    | 64–100      | slides              |
| Sproutle | Sprout   | Common    | 2    | 70–108      | walks               |
| Buzzlet  | Zap      | Uncommon  | 3    | 84–124      | hovers, flying      |
| Flickit  | Ember    | Uncommon  | 3    | 96–140      | darts               |
| Chilliw  | Frost    | Rare      | 4    | 108–154     | slides              |
| Wispurr  | Veil     | Rare      | 4    | 126–176     | drifts, flying      |
| Pebblo   | Stone    | Very rare | 5    | 150–206     | stands still        |
| Lumina   | Radiant  | Very rare | 5    | 180–240     | drifts, flying      |

("Diff" is the internal difficulty 1–5 used in the catch-odds formula — it is
not shown in the game; the POWER number is the only difficulty signal.)

POWER badges are color-coded by tier: **< 100** olive · **100–179** green ·
**180–259** blue · **260+** purple.

## 🎯 Catch odds

```
chance = 0.90 − 0.075 × difficulty (1–5)
       + ring bonus  (EXCELLENT +0.18 · GREAT +0.10 · NICE +0.04)
       − POWER penalty (up to −0.12 for POWER above 180)
       clamped to 42% … 97%
```

The focus ring cycles every 4.3 s (scale 1.9 → 0.65). Land the hit while the
ring scale is **below 0.85** for EXCELLENT, **below 1.25** for GREAT — anything
else counts as NICE. So: easy critters are near-guaranteed with decent timing,
and even a perfect EXCELLENT toss at a very rare Lumina only reaches ~70%.

## ✨ Features

- **One file, zero dependencies** — vanilla HTML/CSS/JS; runs offline from `file://`.
- **A living park** — per-species wandering AI (hoppers, walkers, sliders,
  darters, hovering drifters, and one stubborn statue), depth scaling, idle
  animations, ambient butterflies, drifting leaves, and birds crossing the sky.
  Ground critters keep to the meadow; flying species (Buzzlet, Wispurr, Lumina)
  roam up into the open sky with their hover animations.
- **Flick-to-throw physics** — no aim assist: the Glowseed flies exactly along
  your flick, like a real thrown object. It shrinks as it flies away from the
  camera with a ground shadow tracking beneath it, so the toss reads as a real
  3D arc. Pointer-velocity tracking, gravity, wall bounces, and swept
  segment-vs-circle hit detection so fast throws can't tunnel through the target.
- **A strolling companion** — the player stands centered on the road (glued to
  the parallax layer) and strolls over to where each critter was caught.
- **Skill-based timing** — the shrinking focus ring rewards a well-timed release.
- **Hand-drawn SVG art** — every critter and the Glowseed are inline SVG with
  per-instance hue and size variation.
- **Parallax world** — the park is built from four pointer-driven depth layers
  (sun & clouds, far hills, tree line, near meadow) with atmospheric depth:
  distant layers are softly blurred and hazed, a vignette adds focus, and the
  encounter screen has its own layered backdrop with the same parallax.
- **Procedural audio** — WebAudio synthesizes every sound: the throw whoosh,
  hit ticks, wobble ticks, catch chimes, the break-free buzz, and a little
  per-species "cry" derived deterministically from its name.
- **Particle FX** — a canvas layer bursts stars, petals, and sparks on hits
  and catches.
- **Journal & stats** — species gallery, best POWER, counts, and toss totals,
  persisted in `localStorage` under the key `wandergrove-v1`.
- **Responsive & mobile-ready** — pointer events with a touch-event fallback
  for older browsers, safe-area insets respected.
