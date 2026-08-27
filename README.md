# 🎵 Elixir — The Sound Dose

<div align="center">
  <img src="https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white" alt="Dart">
  <img src="https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite">
  <img src="https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android">
</div>

<br>

**Elixir** is a privacy-first, offline-centric local music player built with Flutter. Designed for audiophiles who demand complete control over their music libraries, Elixir combines a stunning dynamic UI with a robust, metadata-aware audio engine. It goes beyond simple playback, featuring automated metadata cleaning, synchronized lyrics, deep usage analytics, and a highly customizable listening experience.

> [!IMPORTANT]
> **Repository Notice:** This is the public showcase repository for Elixir. It contains the project's documentation, architecture overview, and UI previews. The actual source code for Elixir is maintained in a separate, proprietary private repository.

---

## 📑 Table of Contents

1. [Core Features](#-core-features)
2. [System Architecture](#-system-architecture)
3. [Deep Dive: Key Modules](#-deep-dive-key-modules)
4. [UI & User Experience](#-ui--user-experience)
5. [Installation & Setup](#-installation--setup)
6. [Development Roadmap](#-development-roadmap)
7. [Project Structure](#-project-structure)

---

## ✨ Core Features

*   **Offline-First Engine**: Scans device storage using `on_audio_query`, intelligently filtering out short audio clips (<30s) to build a pure music library.
*   **Intelligent Metadata Sync**: Multi-engine fallback strategy (Deezer, iTunes) to automatically fetch missing album artwork, clean track titles, and organize artists.
*   **Dynamic Ambient UI**: Extracts dominant colors from album artwork using `palette_generator` to create immersive, glass-morphic interface themes on the fly.
*   **Synchronized Lyrics**: Integrates with `lrclib.net` to provide real-time, karaoke-style synced lyrics during playback.
*   **Advanced Playback Controls**: Features a built-in 5-band Equalizer, gapless playback, customizable crossfade durations, and volume normalization.
*   **Deep Analytics**: Tracks your listening habits, compiling monthly reports of total listening time, top 5 songs, and top 5 artists.
*   **Persistent State**: Custom navigation layout ensures music continues playing seamlessly in the background and across different app tabs.

---

## 🏗 System Architecture

Elixir utilizes a robust service-based architecture, decoupling the UI from the underlying audio and database engines.

```text
[ User Interface (Flutter) ]
        |
        | (State Management / Listeners)
        v
[ Core Services ]
  ├── AudioService (just_audio, audio_service) -> Handles Background Playback & Lockscreen
  ├── QueueService -> Manages Playlists, Shuffle, Loop & Transitions
  ├── DatabaseService (sqflite) -> Persists Metadata, Analytics, & Play History
  ├── MetadataSyncService -> Interacts with external APIs (Deezer/iTunes)
  └── NewsService -> Discovers new releases for followed artists
        |
        | (Device I/O)
        v
[ Local Storage (MediaStore / File System) ]
```

---

## 🔍 Deep Dive: Key Modules

### 1. The Audio Engine (`just_audio` & `audio_service`)
At the heart of Elixir is a highly optimized audio handler. It interacts directly with Android's MediaStore using content URIs to bypass strict Scoped Storage limitations, ensuring reliable playback. It maintains persistent background notifications, allowing users to control playback from the lock screen.

### 2. Metadata & Database Service (`sqflite`)
Elixir doesn't just play files; it organizes them. Upon scanning, it creates a relational SQLite database (managed via robust migration scripts up to v8). It stores play counts, exact listening durations (in milliseconds) for analytics, custom playlists, and user-edited metadata.

### 3. Automated Enrichment & Sync
Songs downloaded from the internet often have messy titles or missing covers. Elixir's `MetadataSyncService` queries external APIs using a fallback mechanism. It cleans "raw" titles into presentation-ready formats and fetches high-resolution album art, saving it locally to prevent redundant network calls.

### 4. Synced Lyrics Engine
The `SyncedLyricsView` parses `.lrc` formatted timestamps and synchronizes them with the current audio playback position. It uses `ScrollController` animations to keep the active lyric centered and highlighted with a neon accent, providing a true karaoke experience.

---

## 🎨 UI & User Experience

Elixir strictly adheres to a premium dark-theme aesthetic (`AppTheme.darkTheme`) highlighted by its signature neon orange accent (`#FF5F1F`).

*   **Rotating Vinyl**: The custom bottom navigation bar features a central, rotating vinyl record widget that serves as an intuitive anchor to the `NowPlayingPage`.
*   **Animated Equalizer**: Real-time visual feedback during playback with staggered, animated equalizer bars.
*   **Gesture-Based Scrubbing**: Intuitive controls allow users to swipe to change tracks or scrub through a song smoothly.
*   **Customizable Audio Quality**: Users can define streaming fallbacks and tune their experience with a custom 5-band EQ and presets (Bass Boost, Rock, Vocal, etc.).

---

## 🚀 Releases & App Availability

### Availability
As the source code is maintained in a private repository, the application is distributed via pre-compiled releases. 

### Steps to Install

1.  **Download the App**
    Navigate to the [Releases](../../releases) tab of this repository.
    Download the latest `app-release.apk` for Android.

2.  **Android Permissions**
    Upon installation, ensure your Android device grants the necessary storage permissions. Elixir targets API 33+ and requires `READ_MEDIA_AUDIO` permissions to function.

---

## 🗺 Development Roadmap

*   **Phase 1: Core Playback & Scanning (Completed)**
    *   Implement `on_audio_query` for local file detection.
    *   Integrate `just_audio` for reliable stream handling.
*   **Phase 2: UI & Theming (Completed)**
    *   Design the dark-theme glass-morphic interface.
    *   Implement dynamic palette generation from album art.
*   **Phase 3: Data & Analytics (Completed)**
    *   Construct the SQLite database for play history and custom metadata.
    *   Build the Analytics Dashboard for monthly listening reports.
*   **Phase 4: Enrichment (Completed)**
    *   Integrate Deezer/iTunes APIs for metadata cleaning.
    *   Implement the `lrclib` synced lyrics engine.
*   **Phase 5: Future Enhancements (Planned)**
    *   Cloud sync for user profiles and playlists.
    *   Cross-platform support (iOS / Desktop).
    *   Advanced audio effects (Reverb, Pitch shifting).

---

## 📂 Project Structure

```text
lib/
├── main.dart                 # App entry point & initialization
├── models/                   # Data classes (Track, Artist, Playlist)
├── services/                 # Core logic
│   ├── audio_service.dart    # Background playback handler
│   ├── database_service.dart # SQLite management
│   ├── queue_service.dart    # Playback queue & transitions
│   └── metadata_sync_service.dart # API fetching
├── theme/                    # App colors, fonts, and styles
│   └── app_theme.dart
└── ui/                       # UI Components
    ├── screens/              # Main pages (Home, Library, Analytics)
    └── widgets/              # Reusable components (Vinyl, Equalizer, Lyrics)
```

---

<div align="center">
  <b>Built with passion for music and beautiful code.</b><br>
  <i>Designed for those who want their sound dose, locally and privately.</i>
</div>
