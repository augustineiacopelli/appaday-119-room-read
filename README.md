# Room Read - Interior Design Assistant

App 119 of AppADay, a daily discipline project to design, build, and ship one complete web app every day.

Live portfolio: https://augustineiacopelli.github.io/appaday/

## What it does

Start a project, add one or more rooms to it, and photograph each one from a few angles: full room, each wall, floor, ceiling, and any furnishings worth a closer look. Add context per room (room name, budget, fixed elements to keep, style preference), then run Analyze All Rooms to get a Claude-generated design read for every room in the project: a critique, a set of observations, a tappable punch list with color swatches for any suggested paint or accent colors, and two or three style directions each paired with reference photos pulled live from Openverse. Past projects are saved locally and browsable in a history list, with a tab bar to flip between each room's results.

## How it works

Single self-contained `index.html`. No build step, no framework, no backend.

- A small hand-rolled state machine drives six screens: Home/History, Project Overview, Context Form, Photo Capture, Analyzing, and Results, plus a Settings modal.
- A project holds a list of rooms. Each room tracks its own context fields, photo set, analysis status, and result, so a project can be a single room or a whole apartment.
- Photos are compressed client-side on canvas to a working image near 1024px and a thumbnail near 150px, both base64 JPEG, before anything leaves the browser. Each room supports up to six photos.
- Analyze All Rooms calls the Claude API (model: claude-sonnet-5) once per room in sequence, sending that room's compressed photos plus its context fields, and is instructed to return JSON only. A parsing layer strips code fences and falls back to a raw text view with a per-room retry button if parsing fails.
- Openverse's public API (no key required) supplies two or three reference thumbnails per style direction, fetched client-side.
- Saved analyses persist in localStorage as a project: id, date, project name, a representative thumbnail, and each room's parsed result minus reference image URLs, wrapped in try/catch throughout.

## Using it

Open the app, tap the gear icon, and paste in your own Anthropic API key. The key is stored only in your browser's localStorage and is sent solely to Anthropic's API. Settings also holds an optional default style preference that pre-fills on every new room.

## Stack

HTML, CSS, and vanilla JavaScript in one file. Google Fonts (Fraunces, Inter) via CDN. Claude API for vision analysis. Openverse API for reference imagery.

---

Part of [AppADay](https://augustineiacopelli.github.io/appaday/) by Augustine Iacopelli.
