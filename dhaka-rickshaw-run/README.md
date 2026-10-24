# Dhaka Rickshaw Run

A browser driving game set on real Dhaka roads. You ride an **electric motorbike** through traffic, junctions and roundabouts, in a 2D top-down view or first person.

Single HTML file, vanilla JavaScript, Canvas 2D and Web Audio. No game engine and no image or audio assets: every vehicle, building and sound is generated in code.

## Play

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Routes

| Route | Stops |
|---|---|
| Mirpur Road | Gabtoli → Technical → Shyamoli → Asad Gate → Science Lab → New Market → Azimpur |
| Mirpur 10 → Shahbagh | Mirpur 10 Circle → Kazipara → Agargaon → Bijoy Sarani → Farmgate → Karwan Bazar → Shahbagh (MRT Line 6 viaduct on the median, oncoming lanes beyond) |
| Airport Road | Uttara → Airport → Khilkhet → Kuril → Banani → Mohakhali → Jahangir Gate |
| Gulshan → Rampura | Gulshan 2 Circle → Gulshan 1 Circle → Badda → Rampura Bridge |
| Motijheel → Sadarghat | Shapla Chattar → Gulistan → Nawabpur → Bahadur Shah Park → Sadarghat |
| Endless | Infinite Mirpur Road, for the leaderboard |

Distances are scaled to about 40% of the real roads.

## Controls

| Action | Keyboard | Phone |
|---|---|---|
| Change lane | ← → / A D | ◀ ▶ pads, or swipe |
| Turbo (hold) | ↑ / W | TURBO |
| Brake (hold) | ↓ / S | BRAKE |
| Roadster Hop (fly) | Space / F | FLY 🚀 |
| Horn | H | 📯 |
| Switch camera (2D / 1st person) | V | 1P / 2D button |
| Immortal mode (cheat) | G | Tap the distance counter 5× |
| Pause / mute | P / M | — |

## Rules

- 5 lives per run. A crash or a flat battery costs one; you respawn in place with a short shield.
- Hitting a pedestrian costs 100 m of distance.
- Immortal mode: no crashes or battery drain, but the run doesn't count for your best or the leaderboard.
- Battery drains over time (faster on Turbo, slower when braking). Battery packs add 28%.

## Multiplayer and leaderboard

Rooms, live ghost racers and the global leaderboard use the claude.ai Artifact runtime (`room`, `db`, `user` capabilities). Opened as a plain web page, those features switch off and the game runs solo, saving your best on the device.

## Code map

| Part | What it does |
|---|---|
| `CFG`, `ROUTES`, `BUS_LINES` | Tunable numbers and route data |
| `Sound` | Synthesized motor, horns (with street echo), effects |
| `draw*` functions | Vehicles and people in a light oblique 3D view; `drawBike` is the layered 3D player bike |
| `renderFP` and `fp*` | First-person renderer: perspective road, 3D vehicle boxes, peripheral markers, mirrors |
| Scenery + route renderers | Buildings, zebras, junctions, roundabouts, signs, bridges, metro |
| `warpBlit` | Bends the road: ground and overhead layers are drawn straight, then shifted row by row |
| `update(dt)` / `render()` | Game loop on `requestAnimationFrame` with a capped delta time |
| `MP`, `LB` | Rooms and races over presence; leaderboard in the artifact database |
