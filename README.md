# 🎵 Elixir — The Sound Dose

<div align="center">
  <img src="https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white" alt="Dart">
  <img src="https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite">
  <img src="https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android">
</div>

<br>

**Elixir** is a premium, privacy-first, offline-centric local music player built from the ground up with Flutter. Designed for audiophiles who demand complete control over their local music libraries, Elixir completely rethinks the local music experience. By combining a stunning, dynamic, glass-morphic UI with a highly robust, metadata-aware audio engine, it offers features usually reserved for massive streaming platforms—completely offline. 

Elixir goes far beyond simple playback. It features automated metadata cleaning, synchronized LRC lyrics, deep usage analytics, and a highly customizable listening experience featuring a 5-band Equalizer and gapless playback.

> [!IMPORTANT]
> **Repository Notice:** This is the public showcase repository for Elixir. It contains the project's documentation, architecture overview, and UI previews. The actual source code for Elixir is maintained in a separate, proprietary private repository to protect the intellectual property and core engine algorithms.

---

## 📑 Table of Contents

1. [Core Features In Detail](#-core-features-in-detail)
2. [Deep Dive: System Architecture](#-deep-dive-system-architecture)
3. [Deep Dive: The Data Layer (SQLite)](#-deep-dive-the-data-layer-sqlite)
4. [UI & User Experience Flow](#-ui--user-experience-flow)
5. [Audio Processing & Playback Engine](#-audio-processing--playback-engine)
6. [Releases & App Availability](#-releases--app-availability)
7. [Development Roadmap](#-development-roadmap)
8. [Project Structure Breakdown](#-project-structure-breakdown)

---

## ✨ Core Features In Detail

Elixir is packed with advanced functionalities that bridge the gap between simple local players and premium streaming services.

*   **Intelligent Offline-First Engine**: 
    Elixir uses `on_audio_query` to deeply scan device storage. It employs an intelligent filtering mechanism to automatically ignore voice notes, ringtones, and short audio clips (under 30 seconds), ensuring your music library remains pure and uncluttered.
*   **Multi-Engine Metadata Sync**: 
    Downloaded local files often suffer from messy titles and missing album art. Elixir implements a `MetadataSyncService` that utilizes a fallback strategy (querying Deezer, then iTunes) to automatically fetch high-resolution album artwork, clean up track titles (e.g., removing "(Official Video)" or "128kbps"), and organize artists.
*   **Synchronized Karaoke Lyrics**: 
    Integrated with `lrclib.net`, Elixir fetches and parses `.lrc` formatted timestamps. As the song plays, the lyrics scroll and highlight in real-time, providing an immersive karaoke-style experience.
*   **Deep Listening Analytics**: 
    Elixir tracks your listening habits with millisecond precision. The Analytics Dashboard compiles monthly reports, showing your total listening time, your top 5 most-played songs, and your top 5 favorite artists, rivaling features like "Spotify Wrapped."
*   **Dynamic Ambient UI**: 
    The interface is not static. Using `palette_generator`, Elixir extracts dominant and vibrant colors from the currently playing album's artwork. It then dynamically applies these colors to the app's background gradients, shadows, and UI accents, creating a seamless, immersive visual experience.

---

## 🏗 Deep Dive: System Architecture

Elixir is built on a highly decoupled, service-oriented architecture, ensuring that heavy audio processing and database queries do not drop UI frames.

```text
[ User Interface Layer (Flutter / Provider / ValueNotifiers) ]
        |
        |  (State Management & Reactive Listeners)
        v
[ Core Service Layer ]
  ├── AudioService     -> Manages `just_audio` streams, Android Lockscreen controls, MediaSession APIs
  ├── QueueService     -> Handles Playlists, Shuffle logic, Loop states, and Crossfade transitions
  ├── DatabaseService  -> Manages sqflite transactions (Play history, custom metadata, favorites)
  ├── MetadataSync     -> REST API integrations for Deezer/iTunes metadata enrichment
  └── NewsService      -> Discovers new releases for followed artists via MusicBrainz
        |
        |  (Platform Channels / Device I/O)
        v
[ Local Storage (Android MediaStore / File System) ]
```

---

## 💾 Deep Dive: The Data Layer (SQLite)

Instead of relying solely on the device's default MediaStore (which can be slow and limited), Elixir builds its own relational database using `sqflite`.

*   **Migration System**: The database utilizes a robust migration system (currently at schema version 8) to seamlessly upgrade tables without losing user data.
*   **Track Metadata**: Stores both the `raw_title` (from the file) and the `clean_title` (fetched from APIs or manually edited by the user). It also caches the local paths to downloaded high-res album banners to prevent redundant network calls.
*   **Analytics Tracking**: Every time a song finishes, the `DatabaseService` logs the timestamp and the exact duration played, allowing the app to calculate total monthly listening time and determine top artists.
*   **Custom Playlists**: Supports creating, editing, and reordering custom user playlists, stored entirely locally and persistently.

---

## 🎨 UI & User Experience Flow

Elixir strictly adheres to a premium dark-theme aesthetic (`AppTheme.darkTheme`), highlighted by its signature neon orange accent (`#FF5F1F`), designed specifically to reduce eye strain in low-light environments.

*   **The Rotating Vinyl Navigation**: 
    The custom bottom navigation bar breaks traditional design by featuring an oversized, rotating vinyl record in the center. When music is playing, the vinyl rotates; when paused, it stops. Tapping it seamlessly expands the `NowPlayingPage`.
*   **Now Playing Experience**: 
    Features an immersive, full-screen playback view. It includes animated, staggered equalizer bars that react to playback state, gesture-based scrubbing (swipe left/right to change tracks), and a beautiful glass-morphism background that blurs the album art.
*   **Manual Track Editor**: 
    If the automated sync fails, users can open the "Edit Track Sheet." Here, they can manually update the title/artist, paste in their own lyrics, or upload a custom cover image directly from their gallery.

---

## 🎛 Audio Processing & Playback Engine

At the core of Elixir is the `just_audio` and `audio_service` combination, strictly configured to bypass Android's Scoped Storage limitations by utilizing MediaStore Content URIs (`content://media/external/audio/...`).

*   **5-Band Equalizer**: Users have access to a fully functional Equalizer (60Hz, 230Hz, 910Hz, 3.6kHz, 14kHz) with pre-built presets (Bass Boost, Treble Boost, Vocal, Rock) and custom manual control.
*   **Gapless Playback & Crossfade**: The audio engine supports true gapless playback for seamless album transitions. Users can also configure a custom crossfade duration (0 to 12 seconds) in the settings.
*   **Volume Normalization**: Built-in volume normalization ensures that a quiet acoustic track followed by a loud EDM track play at a consistent perceived volume level.

---

## 🚀 Releases & App Availability

### Availability
As the source code is maintained in a private repository to protect the application's core logic, the application is distributed directly to users via pre-compiled releases.

### Steps to Install

1.  **Download the App**
    Navigate to the [Releases](../../releases) tab of this repository.
    Download the latest `app-release.apk` for your Android device.

2.  **Installation & Permissions**
    Install the `.apk` file (you may need to allow installation from unknown sources in your Android settings).
    Upon first launch, ensure your Android device grants the necessary storage permissions. Elixir targets Android API 33+ and strictly requires `READ_MEDIA_AUDIO` permissions to scan your local music library.

---

## 🗺 Development Roadmap

Elixir is constantly evolving. Here is the structured roadmap for the project:

*   **Phase 1: Core Playback & Scanning (Completed)**
    *   Implement `on_audio_query` for deep local file detection.
    *   Integrate `just_audio` for reliable background stream handling.
*   **Phase 2: UI & Theming (Completed)**
    *   Design the dark-theme glass-morphic interface and custom navigation.
    *   Implement dynamic palette generation from album art.
*   **Phase 3: Data & Analytics (Completed)**
    *   Construct the relational SQLite database for play history and manual metadata edits.
    *   Build the comprehensive Analytics Dashboard for monthly listening reports.
*   **Phase 4: Enrichment & Lyrics (Completed)**
    *   Integrate Deezer and iTunes APIs for automated metadata cleaning.
    *   Implement the `lrclib` synced lyrics engine with real-time UI highlighting.
*   **Phase 5: Future Enhancements (Planned)**
    *   Cloud sync for user profiles, custom playlists, and listening history.
    *   Cross-platform support framework adaptation (iOS / Desktop).
    *   Advanced audio effects engine (Reverb, Pitch shifting, Speed control).

---

## 📂 Project Structure Breakdown

For those interested in the architecture, here is how the private source code is organized:

```text
lib/
├── main.dart                 # App entry point, Service initialization & Theme setup
├── models/                   # Strongly typed data models (Track, Artist, Playlist, Lyrics)
├── services/                 # Core business logic layer
│   ├── audio_service.dart    # Background playback, MediaSession handling
│   ├── database_service.dart # SQLite management & migrations
│   ├── queue_service.dart    # Playback queue, shuffle arrays, & track transitions
│   └── metadata_sync_service.dart # External API fetching and error handling
├── theme/                    # App design system (Colors, fonts, shadows, borders)
│   └── app_theme.dart
└── ui/                       # User Interface Components
    ├── screens/              # Main routing pages (Home, Library, Analytics, Settings)
    └── widgets/              # Reusable UI parts (RotatingVinyl, AnimatedEqualizer, SyncedLyrics)
```

---

<div align="center">
  <b>Built with passion for music and beautiful code.</b><br>
  <i>Designed for those who want their sound dose, locally and privately.</i>
</div>
