
---

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
