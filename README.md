<div align="center">

  <img src="assets/logo.png" alt="Anime Akashic Records Logo" width="120" height="120">

  # Anime Akashic Records (AAR)

  **Semantic Plot Search, Recommendation Engine & Native MCP Server for Anime, Manga, Light Novels, Visual Novels, Manhwa, and Manhua.**

  [![Status](https://img.shields.io/badge/Status-Open%20Beta-B83A50?style=flat-square)](https://aarecords.tech)
  [![Live Demo](https://img.shields.io/badge/Live%20Demo-aarecords.tech-3D6B48?style=flat-square)](https://aarecords.tech)
  [![API Docs](https://img.shields.io/badge/OpenAPI%20Docs-api.aarecords.tech%2Fdocs-387b97?style=flat-square)](https://api.aarecords.tech/docs)
  [![MCP Server](https://img.shields.io/badge/MCP%20Server-mcp.aarecords.tech%2Fsse-6B4C9A?style=flat-square)](https://mcp.aarecords.tech/sse)
  [![Indexed Works](https://img.shields.io/badge/Indexed%20Works-146%2C000%2B-2E5A88?style=flat-square)](https://aarecords.tech)

</div>

---

## Overview

Traditional anime and manga APIs (**Jikan/MyAnimeList**, **AniList**, **TMDB**, **Kitsu**, **VNDB**) are built for structured metadata, user watchlists, airing schedules, and high-resolution artwork. However, they fall short when users search by **forgotten scenes, specific plot tropes, character dynamics, or narrative vibes**.

**Anime Akashic Records (AAR)** is a semantic discovery and recommendation layer designed to work directly alongside your existing database or application stack:

- **146,000+ Canonical Works** across 6 formats: `ANIME`, `MANGA`, `LIGHT_NOVEL`, `VISUAL_NOVEL`, `MANHWA`, and `MANHUA`.
- **444,000+ Linked Entities**: Characters, voice actors (seiyuu), directors, authors, composers, and studios.
- **Plot & Scene Retrieval**: Understands natural language descriptions, obscure plot memories, and specific narrative premises in any language.
- **Multi-Seed Recommendations**: Generate thematic and stylistic recommendations from one or multiple works, with support for negative seeds (`negative_seeds`) and automatic franchise/sequel suppression.
- **Universal Cross-Platform ID Bridge**: Query and filter directly using the external IDs your app already stores (`mal`, `al`, `tmdb`, `imdb`, `vndb`, `kitsu`, `anidb`, `tvdb`) without migrating your database.
- **Ready for AI Agents & Discord Bots**: Built-in **Model Context Protocol (MCP)** server and sub-second REST endpoints ideal for autonomous AI agents, LLM tool-calling, and community Discord bots.

---

## Open Beta & Production API Keys

**Anime Akashic Records is currently in Open Beta.**

### 1. Instant Open Beta Access (No Signup Required)
You can start testing, prototyping, and integrating immediately using the shared Open Beta key:
- **Base URL:** `https://api.aarecords.tech`
- **Open Beta Key:** `aar_free_access`
- **Authentication:** Pass via HTTP header `x-api-key: aar_free_access` (or `Authorization: Bearer aar_free_access`)
- **Beta Rate Limits:** `3 requests/sec` (`30 requests/min` on core endpoints; `/health` is quota-free)

### 2. Dedicated Production API Keys
If you are building or operating a live application, website, Discord bot, or AI agent and need higher rate limits or dedicated access:
- Email **[contact@aarecords.tech](mailto:contact@aarecords.tech)** with a brief overview of your project or app.
- Production keys are issued based on project readiness and active integration needs.
- **Early Adopter Advantage:** Projects and developers granted a Production API Key during the Open Beta period will receive special benefits and priority terms after the beta concludes.

---

## Core Capabilities

### 1. Natural Language Plot & Trope Search (`GET` / `POST /search`)
Find works by describing scenes, character traits, or atmosphere rather than exact titles:
- *"A scientist accidentally invents a time machine using a microwave"*
- *"A swordsman who cannot die and the girl who hires him for revenge"*
- *"Korean romance manhwa where the red-haired protagonist is allergic to eggplants"*
- Combine semantic queries with strict metadata filters: media `type`, `status`, `genre`, `tag`, `theme`, `studio`, release `year` ranges, episode/chapter counts, `score`, staff credits, and `required_ids`.

### 2. Cross-Platform ID Bridge (`required_ids`)
Never worry about mismatched catalog entries. Pass `required_ids=mal`, `required_ids=al`, or `required_ids=tmdb` on `/search` or `/recommend` to guarantee that every returned item includes the exact external platform ID your frontend expects.

```json
"external_ids": {
  "aar_id": "aar_7257",
  "mal_id": 9253,
  "anilist_id": 9253,
  "kitsu_id": 5646,
  "anidb_id": 7729,
  "tmdb_id": 42509,
  "tvdb_id": 244061,
  "imdb_id": "tt1910272",
  "wikidata_id": "Q235428"
}
```

### 3. Single & Multi-Seed Recommendations (`/recommend`)
- **Instant Lookup (`GET /recommend/{work_id}`)**: Pass any native or external ID directly (`mal:9253`, `al:9253`, `tmdb:42509`, `imdb:tt0877057`, `vndb:v17`, or `aar_7257`).
- **Multi-Work Blending (`POST /recommend`)**: Combine multiple favorites (e.g., *Steins;Gate* + *Monster*) to find works at their thematic intersection, while steering away from disliked titles via `negative_seeds`.
- **Franchise Filtering**: `exclude_franchise=true` (enabled by default) automatically filters out direct sequels, prequels, OVAs, and spin-offs so users discover genuinely new series.

### 4. Grounded Q&A (`/ask`)
- **`/ask`**: Ask complex questions about lore, plot points, or character relationships and receive answers grounded strictly in verified catalog excerpts (`chunks`).

---

## Built for AI Agents, LLMs & Discord Bots

### Native Model Context Protocol (MCP) Server
Connect **Claude Desktop**, **Cursor**, **Cline**, **Windsurf**, or any custom MCP-compatible AI agent directly to the live Anime Akashic Records engine over SSE — zero local installation required.

- **MCP SSE Endpoint:** `https://mcp.aarecords.tech/sse`

Add this to your MCP client configuration (`claude_desktop_config.json` or `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "anime-akashic-records": {
      "url": "https://mcp.aarecords.tech/sse"
    }
  }
}
```

**Exposed MCP Tools:**
- `aar_search` — Semantic & hybrid search across 146,000+ works by plot description, scene memory, and filters.
- `aar_recommend` / `aar_find_similar` — Single or multi-seed recommendations with franchise exclusion.
- `aar_ask` — Grounded question answering with source citations.
- `aar_stats` & `aar_health` — Live corpus statistics and service status.

### Ideal for Discord Bots & Community Apps
With response times averaging ~150–250ms, AAR drops cleanly into Discord slash commands (`/find-anime`, `/recommend`, `/ask-lore`) and Telegram/WhatsApp bots without hosting heavy local models or scraping third-party sites.

---

## Quickstart & Code Examples

### 1. cURL

```bash
# Semantic plot search filtered to Anime + requiring AniList & TMDB IDs
curl -s "https://api.aarecords.tech/search?q=scientist+accidentally+invents+time+machine&type=anime&required_ids=al,tmdb&limit=3" \
  -H "x-api-key: aar_free_access"

# Direct recommendation using a MyAnimeList ID (or al:9253 / tmdb:42509)
curl -s "https://api.aarecords.tech/recommend/mal:9253?limit=5&required_ids=mal" \
  -H "x-api-key: aar_free_access"
```

### 2. Python (`requests`)

```python
import requests

BASE_URL = "https://api.aarecords.tech"
HEADERS = {"x-api-key": "aar_free_access"}

# Multi-seed recommendation with negative seed exclusion
payload = {
    "seeds": ["mal:9253", "mal:19"],       # Steins;Gate + Monster
    "negative_seeds": ["mal:31240"],       # Exclude Re:Zero vibe
    "limit": 5,
    "exclude_franchise": True,
    "scope": {
        "types": ["ANIME"],
        "required_ids": ["mal", "al"]
    }
}

resp = requests.post(f"{BASE_URL}/recommend", json=payload, headers=HEADERS, timeout=10)
resp.raise_for_status()

for item in resp.json().get("results", []):
    ids = item.get("external_ids", {})
    print(f"{item['title']} ({item.get('release_year')}) -> MAL: {ids.get('mal_id')} | AL: {ids.get('anilist_id')}")
```

### 3. JavaScript / TypeScript (Node.js, Next.js & Discord.js Bots)

```javascript
const AAR_API_KEY = process.env.AAR_API_KEY || "aar_free_access";

async function findAnimeByPlot(description) {
  const params = new URLSearchParams({
    q: description,
    type: "anime,manga",
    required_ids: "al,mal",
    limit: "5"
  });

  const response = await fetch(`https://api.aarecords.tech/search?${params}`, {
    headers: { "x-api-key": AAR_API_KEY }
  });

  if (!response.ok) throw new Error(`AAR API error: ${response.status}`);
  const data = await response.json();
  return data.results;
}

// Example usage inside a Discord bot command or web app:
findAnimeByPlot("cyberpunk detective mystery with androids").then(console.log);
```

### 4. Dart / Flutter

```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

Future<List<dynamic>> getRecommendations(int anilistId) async {
  final uri = Uri.parse(
    'https://api.aarecords.tech/recommend/al:$anilistId?limit=10&required_ids=al',
  );

  final response = await http.get(
    uri,
    headers: {'x-api-key': 'aar_free_access'},
  );

  if (response.statusCode == 200) {
    final data = jsonDecode(response.body);
    return data['results'] as List<dynamic>;
  } else {
    throw Exception('Failed to load recommendations: ${response.statusCode}');
  }
}
```

### 5. Kotlin / Android (`OkHttp`)

```kotlin
import okhttp3.OkHttpClient
import okhttp3.Request

val client = OkHttpClient()

fun fetchSimilarByTmdb(tmdbId: Int): String? {
    val url = "https://api.aarecords.tech/recommend/tmdb:$tmdbId?limit=10&required_ids=tmdb"
    val request = Request.Builder()
        .url(url)
        .addHeader("x-api-key", "aar_free_access")
        .build()

    client.newCall(request).execute().use { response ->
        if (!response.isSuccessful) throw RuntimeException("HTTP ${response.code}")
        return response.body?.string()
    }
}
```

---

## Endpoints Overview

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/search` | `GET`, `POST` | Semantic & hybrid plot discovery with 25+ metadata, cast/staff, and external ID filters. |
| `/recommend/{work_id}` | `GET` | Single-work similarity recommendations using any AAR or external platform ID. |
| `/recommend` | `POST` | Multi-seed recommendations with negative seed steering and franchise filtering. |
| `/ask` | `GET`, `POST` | Grounded Q&A over canonical work synopses, character profiles, and lore excerpts. |
| `/suggest` | `POST` | Submit missing works, community plot descriptions, or metadata corrections. |
| `/health` | `GET` | Quota-free service health check and live catalog counts. |

---

## Links & Contact

- **Interactive Web Demo:** [https://aarecords.tech](https://aarecords.tech)
- **Interactive API Docs (Swagger UI):** [https://api.aarecords.tech/docs](https://api.aarecords.tech/docs)
- **ReDoc Reference:** [https://api.aarecords.tech/redoc](https://api.aarecords.tech/redoc)
- **LLM / Agent Context Guide:** [https://aarecords.tech/llms.txt](https://aarecords.tech/llms.txt)
- **Production API Keys & Partnerships:** [contact@aarecords.tech](mailto:contact@aarecords.tech)

---

<div align="center">
  <sub>Anime Akashic Records (AAR) — Built for developers, trackers, Discord bots, and AI agents.</sub>
</div>
