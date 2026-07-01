# helios-visualizer
An interactive 3D knowledge graph — a living map of mythological, cultural, and philosophical ideas and the resonances between them.  **Live Deployment:** [helios.ruetzanita.com](https://helios.ruetzanita.com)

Helios Visualizer lets users explore a spinning constellation of glowing spheres representing concepts from classical mythology to contemporary music and cinema, read essay-like descriptions of each node, and trace connections from ancient antiquity through modern pop culture.

---

## 🎨 Core Features

*   **Dynamic 3D Physics Graph:** Built with `Three.js` and `react-force-graph-3d`, utilizing force-directed simulations to layout nodes in 3D space with interactive camera navigation and zoom-to-target transitions.
*   **Aesthetic Styling:** Curated warm monochrome palette (blood crimson through amber to pale gold) matching a professional "golden hour" visual layout. Included details like ambient glowing highlight effects and drifting dust particles.
*   **Fuzzy Search Engine:** Custom debounced search query bar that fires against an Express search microservice, scoring and sorting nodes and connection paths by relevance on every keystroke.
*   **Constellation (Isolation) Mode:** Isolate focus onto any selected node to view only its direct first-degree neighbors, dynamically re-mapping the physical layout on the fly.
*   **Auto-Sync Pipeline:** Development watch daemon automatically catches changes to data configuration, extracts, validates graph constraints, and updates the search index within 800ms.

---

## 🏗 System Architecture

```
┌──────────────────────────────────────────────────┐
│                 YOUR BROWSER                     │
│                                                  │
│   ┌────────────┐    ┌────────────────────────┐   │
│   │  SearchBar │───▶│   Express API Server   │   │
│   │  (React)   │◀───│   localhost:3001        │   │
│   └────────────┘    └────────────────────────┘   │
│                              ▲                   │
│   ┌────────────┐             │ reads             │
│   │   App.jsx  │        server/data.json         │
│   │  (React)   │             │                   │
│   └────────────┘        (auto-updated             │
│          │               when you edit            │
│          ▼               src/data/*)              │
│   ┌─────────────────┐                            │
│   │ GraphRenderer   │  ── 3D spinning graph ──▶  │
│   │ (Three.js/      │                            │
│   │  react-force-   │                            │
│   │  graph-3d)      │                            │
│   └─────────────────┘                            │
│                                                  │
│          All data ultimately comes from          │
│               src/data/  (the brain)             │
└──────────────────────────────────────────────────┘
```

The system splits responsibilities to optimize client performance:
*   **Frontend (Vite/React 19):** Dynamically fetches nodes and links from the local endpoint on load, rendering physics simulations and complex layouts on the canvas while remaining extremely lightweight.
*   **Backend (Express 5):** Exposes search, fetching, and data parsing APIs. Watches directories for data adjustments, runing validation and sanitization steps automatically to build the runtime JSON cache.

---

## 📂 Directory Structure

*   `src/data/nodes.js` & `src/data/links.js` — Core graph database.
*   `src/App.jsx` — Core UI shell, tracking panel history, search choices, and isolation modes.
*   `src/GraphRenderer.jsx` — Canvas management and Three.js physics configurations.
*   `src/SearchBar.jsx` — Input handling, fuzzy query debouncing, and upward popup results.
*   `server/index.mjs` — Local search and dataset proxy server.
*   `server/extract.mjs` — Extraction, validation, and schema checking script.
*   `test/verify_nodes.py` — Database schema verification test script.

---

## 🚀 Running Locally

### 1. Prerequisites
Make sure you have Node.js (v18+) and Python 3 installed.

### 2. Dependency Installation
```bash
npm install
```

### 3. Generate Search Cache
Create the initial `server/data.json` search database:
```bash
npm run extract
```

### 4. Start Development Server
This starts both the Vite dev server and the Express API server concurrently:
```bash
npm run dev
```

The local application will be running at `http://localhost:5173`.
The production application lives at `https://helios.ruetzanita.com`.

---

## ⚖️ License

Helios Visualizer uses a split (hybrid) licensing model:

*   **Software & Source Code:** All code in this repository (e.g., rendering logic, servers, scripts) is licensed under the [MIT License](LICENSE).
*   **Curated Data & Content:** The database of mythological nodes, connection descriptions, essays, and metadata (specifically the contents under [src/data/](file:///home/anita_ruetz/mythos_visualizer/src/data/)) are licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](LICENSE-CONTENT).
