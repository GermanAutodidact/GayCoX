# GayCoX – Changelog

## Version 2.1 – Vorschau-First-Download + Hardening

### Neuer Ablauf
- Nach Phase-1-Scan stoppt die App bei **AWAITING_SELECTION**
- User sieht Thumbnails in der Vorschau und wählt aus
- Erst nach Bestätigung startet Phase-2-Download der Fullsize-Bilder
- Neue Status: `AWAITING_SELECTION`, `READY_TO_DOWNLOAD`
- Kandidaten-JSON: `[{"t":"thumbUrl","f":"fullUrl"}, …]`

### Weitere Verbesserungen
- Retry-Button für ERROR-Items (`retryItem` / `retryAllErrors`)
- Speicherordner: `Pictures/GayCoX/…`
- WorkManager / Notification / DataStore auf „GayCoX“ umbenannt
- WebView: Hardware-Acceleration, Cache-Mode, Media ohne User-Gesture
- `ImageCandidate` (Thumbnail + Fullsize) im Scraper
- `confirmSelectionAndDownload` im Repository

## Version 2.0 – Bugfixes, Syntaxbereinigung & Volle Lauffähigkeit

- Syntax-/Parsing-Fehler in UI-Dateien behoben
- Version Catalog & Dependencies synchronisiert
- App-Identität auf GayCoX vereinheitlicht
- Unit-/Robolectric-Tests ergänzt
