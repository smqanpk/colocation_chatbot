# Towerco Colocation Analyzer

A single-page web app for sales and planning teams to run colocation & drift analysis against a towerco inventory — no backend, no install, runs entirely in the browser.

## 🚀 Deploy to GitHub Pages (2 minutes)

1. Create a new **public** GitHub repository
2. Upload `index.html` and `sites_data.json` to the root
3. Go to **Settings → Pages → Source → Deploy from branch → main / root**
4. Your app is live at `https://<your-username>.github.io/<repo-name>/`

## 🌐 Alternative: Deploy to Netlify (drag & drop)

1. Go to [netlify.com](https://netlify.com) → Sign up free
2. Drag the entire folder onto the **"Deploy manually"** area
3. Done — live URL in seconds

## 📁 Files

| File | Purpose |
|---|---|
| `index.html` | The entire application (map, charts, table, AI assistant) |
| `sites_data.json` | Pre-processed colocation results (1,120 sites) |

## ✨ Features

- **Interactive map** with colour-coded markers (Exact Match / Drift Match / New Build) and clustering
- **Filters** by classification, region, demography, priority, SSRF status, distance, tower height, and city search
- **Stats panel** with live-updating donut, bar, and distribution charts
- **Sortable table** with all 1,120 sites, capped at 500 rows for performance
- **Site detail popup** with full tower and colocation information
- **CSV export** of the current filtered view
- **Upload your own data** — paste any Excel/CSV with Lat/Lng/City/Region/Demography columns and the app re-runs the classification instantly
- **Configurable drift radii** — adjust Dense Urban / Urban / Suburban / Rural thresholds and re-run
- **AI Assistant** — powered by Claude (requires Anthropic API key), answers questions about the current filtered dataset: priority cities, revenue estimates, regional comparisons

## 🔑 AI Assistant

The AI assistant uses the **OpenAI API** (`/v1/chat/completions`) and is compatible with any OpenAI-API-compatible provider (OpenAI, Azure OpenAI, Groq, Together AI, Ollama with an OpenAI-compatible wrapper, etc.).

To use it:
1. Get an API key from [platform.openai.com](https://platform.openai.com)
2. Enter it in the **AI Assistant** panel — it is saved to browser `localStorage` only, never sent anywhere except the chosen API endpoint
3. Pick your model from the dropdown: `gpt-4o`, `gpt-4o-mini`, `gpt-4-turbo`, or `gpt-3.5-turbo`

**Using a different OpenAI-compatible provider?**  
The fetch call targets `https://api.openai.com/v1/chat/completions`. To point it at a different base URL (e.g. Azure, Groq, a local Ollama instance), edit the one `fetch(...)` line in `askAI()` inside `index.html`.

## 🔧 Methodology

| Tier | Threshold | Action |
|---|---|---|
| Exact Match | Distance < 100m | Colocate directly |
| Drift Match | Within land-use drift radius | Propose site relocation |
| New Build | Beyond drift radius | New tower required |

**Drift radii:** Dense Urban 100m · Urban 300m · Suburban 700m · Rural 1,500m  
**Data source:** Zong NRO 2024 rollout plan vs Deodar (SMDB) towerco inventory
