
---
# IMPORTARR

IMPORTARR is a tiny CLI helper that mirrors Sonarr’s **Activity → Queue → Manual Import** flow in your terminal.  
It scans a downloads folder, extracts a **series prefix** (e.g. `Bluey.2019` from `Bluey.2019.S01E01…`), asks you to confirm the matching series, and then triggers Sonarr’s `manualImport` **command** for each episode (with language, quality, release group, and remote path mapping handled).

> Designed for stubborn releases Sonarr won’t auto-import due to ID mismatches. You approve once, IMPORTARR does the rest.

---

## Features

- 🔎 **Interactive series confirmation** per prefix (cached for subsequent episodes)
- 🧠 **Episode detection** from filenames (`SxxEyy`)
- 🌐 **Language injection** using Sonarr’s `/api/v3/language` (fixes “Languages not-null” DB error)
- 🎚 **Quality detection** from filename (720p/1080p/2160p → `WEBDL-*`)
- 🔁 **Remote Path Mapping aware** (host → container) so Sonarr can see files inside Docker
- 📦 **Copy or Move** import mode
- 🧹 (Optional) **Queue cleanup** after import
- 🧪 **Dry-run** mode

---

## Requirements

- Sonarr v3 (API v3)
- Your Sonarr API key
- Python 3.9+
- Network access from the machine running SORTARR to your Sonarr instance
- Correct **Remote Path Mapping** in Sonarr (Settings → Download Clients)

---

## How it works

1. **Scan** the base folder for video files (skips samples).
2. Extract a **prefix** up to `SxxEyy` (e.g. `Bluey.2019`).
3. Query Sonarr’s `/series/lookup` and let you **confirm** the series.
4. Resolve the **library series** (must be added to Sonarr already).
5. For each file:
   - Parse `SxxEyy`, fetch the **episode** via `/episode?seriesId=…`.
   - Detect **quality**, **release group**, and **language(s)**.
   - Map host path → container path via your `PATH_MAP`.
   - Trigger **`POST /api/v3/command`** with `{"name":"manualImport", …}`.
   - (Optional) Remove the matching entries from **Activity → Queue**.

---

## Configuration

Edit these constants at the top of the script:

```python
API_URL = "http://localhost:8989/api/v3"
API_KEY = "YOUR-SONARR-API-KEY"

# Host → Container mapping (source paths visible to Sonarr inside Docker)
PATH_MAP = {
    "/mnt/user/usenet/complete": "/data/complete",
    # add more if needed:
    # "/mnt/cache/usenet/complete": "/data/complete",
}


## Make your console messages English (fastest path)

Below are **search → replace** pairs for the messages we’ve used. You can apply them with your editor or with `sed`. (Safe—no logic changes.)

**Most common replacements:**

- `📂 Scanning` → `📂 Scanning`
- `🔧 API URL:` → `🔧 API URL:`
- `🗂️ Path Mappings:` → `🗂️ Path Mappings:`
- `✅ Sonarr API Verbindung erfolgreich` → `✅ Connected to Sonarr API`
- `❌ Sonarr API Verbindung fehlgeschlagen:` → `❌ Sonarr API connection failed:`
- `🔍 Suche Serie für Prefix:` → `🔍 Looking up series for prefix:`
- `❌ Keine Treffer für` → `❌ No matches for`
- `🎬 Option` → `🎬 Option`
- `   Serie:` → `   Series:`
- `   Übersicht:` → `   Overview:`
- `Ist das die richtige Serie? (y/n/s=skip):` → `Is this the correct series? (y/n/s=skip):`
- `✅ Serie gewählt:` → `✅ Selected series:`
- `🔄 Verarbeite` → `🔄 Processing`
- `⚠️ Kein Path Mapping gefunden für:` → `⚠️ No path mapping found for:`
- `   Host:` → `   Host:`
- `   Cont:` → `   Container:`
- `✅ Episode:` → `✅ Episode:`
- `💡 DRY-RUN:` → `💡 DRY-RUN:`
- `🚀 POST /command` → `🚀 POST /command`
- `❌ Import-Command abgelehnt` → `❌ Import command rejected`
- `✅ Command angenommen` → `✅ Command accepted`
- `– warte kurz auf Status…` → `– waiting for status…`
- `   📊 Status:` → `   📊 Status:`
- `   ❌ Command failed:` → `   ❌ Command failed:`
- `   ↪️ Fallback: Host-Pfad probieren…` → `   ↪️ Fallback: trying host path…`
- `🎉 Imported` → `🎉 Imported`
- `💥 Import gescheitert:` → `💥 Import failed:`
- `➕ Zusätzliches Mapping:` → `➕ Extra mapping:`
- `❌ Ungültiges Mapping. Format:` → `❌ Invalid mapping format. Use:`
- `🔁 Verwende gecachte Serie für` → `🔁 Using cached series for`
- `⏭️ Übersprungen:` → `⏭️ Skipped:`
- `🌐 Verfügbare Sonarr Sprachen:` → `🌐 Available Sonarr languages:`
- `🔄 Path Mapping:` → `🔄 Path mapping:`
- `🔎 /manualimport scan: … → X Kandidaten` → `🔎 /manualimport scan: … → X candidates`
- `❌ Keine Candidates gefunden – prüfe Mapping und Ordner.` → `❌ No candidates found – check path mapping and folder.`

**One-liners with `sed` (example):**
```bash
# Mac: use sed -i '' … ; Linux: sed -i …
sed -i 's/Sonarr API Verbindung erfolgreich/Connected to Sonarr API/g' sortarr.py
sed -i 's/Sonarr API Verbindung fehlgeschlagen:/Sonarr API connection failed:/g' sortarr.py
sed -i 's/Suche Serie für Prefix:/Looking up series for prefix:/g' sortarr.py
sed -i 's/Keine Treffer für/No matches for/g' sortarr.py
sed -i 's/Serie gewählt:/Selected series:/g' sortarr.py
sed -i 's/Verarbeite/Processing/g' sortarr.py
sed -i 's/Kein Path Mapping gefunden für:/No path mapping found for:/g' sortarr.py
sed -i 's/Episode:/Episode:/g' sortarr.py
sed -i 's/Import-Command abgelehnt/Import command rejected/g' sortarr.py
sed -i 's/Command angenommen/Command accepted/g' sortarr.py
sed -i 's/warte kurz auf Status…/waiting for status…/g' sortarr.py
sed -i 's/Fallback: Host-Pfad probieren…/Fallback: trying host path…/g' sortarr.py
sed -i 's/Imported/Imported/g' sortarr.py
sed -i 's/Import gescheitert:/Import failed:/g' sortarr.py
sed -i 's/Zusätzliches Mapping:/Extra mapping:/g' sortarr.py
sed -i 's/Ungültiges Mapping. Format:/Invalid mapping format. Use:/g' sortarr.py
sed -i 's/Verfügbare Sonarr Sprachen:/Available Sonarr languages:/g' sortarr.py
