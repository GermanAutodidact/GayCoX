# GayCoX – Smart Bulk Image Downloader & Gallery Scraper

GayCoX ist ein intelligenter Batch-Bilder-Downloader und Galerie-Scraper für Android
(Jetpack Compose, Room, WorkManager, WebView-Deep-Crawl).

## Ablauf (2-Phasen + Vorschau)

1. **Phase 1 – Scan**  
   URL wird per WebView + Jsoup gecrawlt (Scroll, „Load more“, Unterseiten, Modal-Trigger).  
   Gefundene Thumbnails und Fullsize-URLs werden gespeichert.

2. **Vorschau-Auswahl** (`AWAITING_SELECTION`)  
   Alle Kandidaten erscheinen als **kleine Thumbnails**.  
   Du wählst aus, welche als große Gallery-Bilder heruntergeladen werden sollen  
   (oder „Alle auswählen“ → Bulk-Download).

3. **Phase 2 – Download** (`READY_TO_DOWNLOAD` → `DOWNLOADING`)  
   Nur die gewählten Fullsize-URLs werden geladen, gefiltert, nach JPG konvertiert  
   und unter `Pictures/GayCoX/<Galerie>/` gespeichert.

Direkt-Bild-URLs überspringen die Vorschau und starten den Download sofort.

## Hauptmerkmale

- Deep Web Crawling (BFS, max. Seitendiefe konfigurierbar)
- Thumbnail → Fullsize-Upgrade (`UrlNormalizer.tryUpgradeThumbnail`)
- Vorschau mit Auswahl vor dem Download
- Auto-JPG-Konvertierung + Obergrenze (Standard 300 KB)
- Mindestfilter (Größe / Auflösung)
- Clipboard- & Share-Import
- MediaStore-Integration
- Room-Historie vergangener Sitzungen
- Retry bei Fehler-Items
- Matte Pastel Pink / Heart Theme

## Tech-Stack

| Bereich        | Technologie                                      |
|----------------|--------------------------------------------------|
| UI             | Jetpack Compose, Material 3                      |
| Persistenz     | Room, DataStore Preferences                      |
| Hintergrund    | WorkManager (Foreground Service)                 |
| Netzwerk       | OkHttp, Jsoup, WebView                           |
| Bilder         | Coil, BitmapFactory / JPEG-Kompression           |
| minSdk / target| 29 / 36                                          |

## Projektstruktur

```
app/src/main/java/com/example/
├── MainActivity.kt
├── data/          # Room, Settings, Repository, Models
├── scraper/       # WebViewScraper, UrlNormalizer
├── worker/        # DownloadWorker, MediaSaver
└── ui/            # Compose Screens, ViewModel, Theme
```

## Build

```bash
./gradlew :app:assembleDebug
```

Optional: `.env` aus `.env.example` für Secrets-Plugin / Signing.

## Status-Machine (QueueItem)

| Status               | Bedeutung                                      |
|----------------------|------------------------------------------------|
| `WAITING`            | Wartet auf Phase-1-Scan                        |
| `SCANNING`           | Crawl läuft                                    |
| `AWAITING_SELECTION` | Scan fertig – User muss Vorschau bestätigen    |
| `READY_TO_DOWNLOAD`  | Auswahl bestätigt – Phase 2 startet            |
| `DOWNLOADING`        | Bilder werden heruntergeladen                  |
| `COMPLETED`          | Fertig                                         |
| `ERROR`              | Fehler (Retry möglich)                         |
