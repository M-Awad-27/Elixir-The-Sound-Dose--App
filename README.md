# <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Objects/Headphone.png" alt="Headphone" width="55" height="55" align="center" /> Elixir — The Sound Dose

<div align="center">
  <br>
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=30&pause=1000&color=FF5F1F&center=true&vCenter=true&width=600&lines=The+Ultimate+Sound+Dose;Privacy-First+Audio+Engine;Offline+Music%2C+Reimagined;Premium+Flutter+Experience" alt="Typing SVG" />
  </a>
  <br><br>
</div>

<div align="center">
  <img src="https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white" alt="Flutter">
  <img src="https://img.shields.io/badge/Dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white" alt="Dart">
  <img src="https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite">
  <img src="https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android">
  <img src="https://img.shields.io/badge/UI-Glassmorphism-FF5F1F?style=for-the-badge" alt="UI">
</div>

<br><br>

> [!IMPORTANT]
> **Repository Notice:** This is the public showcase repository for Elixir. It contains the project's documentation, architecture overview, and UI previews. The actual source code for Elixir is maintained in a separate, proprietary private repository to protect the intellectual property and core engine algorithms.

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Activities/Sparkles.png" alt="Sparkles" width="35" height="35" /> Introduction

**Elixir** is a premium, privacy-first, offline-centric local music player built from the ground up with Flutter. 

In an era dominated by subscription-based cloud streaming services, audiophiles and privacy-conscious users are often left behind. Elixir was designed for those who demand complete control over their local music libraries without sacrificing the beautiful aesthetics and smart features of modern apps. 

By combining a stunning, dynamic, glass-morphic UI with a highly robust, metadata-aware audio engine, Elixir offers features usually reserved for massive streaming platforms—completely offline, directly on your device.

---

## 📑 Table of Contents

1. [Core Features Showcase](#-core-features-showcase)
2. [Deep Dive: The Audio Architecture](#-deep-dive-the-audio-architecture)
3. [Deep Dive: Data Persistence & SQLite](#-deep-dive-data-persistence--sqlite)
4. [The UI/UX Experience](#-the-uiux-experience)
5. [Analytics & Insights Engine](#-analytics--insights-engine)
6. [Releases & Installation](#-releases--installation)
7. [Development Roadmap](#-development-roadmap)
8. [Project Structure](#-project-structure)
9. [License & Author](#-license)

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Objects/Musical%20Notes.png" alt="Musical Notes" width="35" height="35" /> Core Features Showcase

Elixir is packed with advanced functionalities that bridge the gap between simple local players and premium streaming services. Every feature is built to run optimally on the device with minimal network overhead.

### 🧠 Intelligent Offline-First Engine
Elixir uses advanced queries (`on_audio_query`) to deeply scan device storage. Instead of blindly importing every audio file on your phone, it employs an intelligent filtering mechanism to automatically ignore voice notes, WhatsApp audios, ringtones, and short audio clips (under 30 seconds), ensuring your music library remains pure and uncluttered.

### 🔄 Multi-Engine Metadata Sync
Downloaded local files often suffer from messy titles (like `track_name_official_video_128kbps.mp3`) and missing album art. Elixir implements a `MetadataSyncService` that utilizes a fallback strategy:
1. It queries the **Deezer API** first for highly accurate metadata.
2. If unsuccessful, it falls back to the **iTunes API**.
It automatically fetches high-resolution album artwork, cleans up track titles, and organizes artists perfectly.

### 🎤 Synchronized Karaoke Lyrics
Why just listen when you can sing along? Integrated with `lrclib.net`, Elixir fetches and parses `.lrc` formatted timestamps. As the song plays, the lyrics scroll and highlight in real-time on your screen, providing an immersive karaoke-style experience. Plain text lyrics are also supported and gracefully rendered.

### 🎨 Dynamic Ambient UI
The interface is not static. Using `palette_generator`, Elixir extracts dominant, vibrant, and muted colors from the currently playing album's artwork. It then dynamically applies these colors to the app's background gradients, shadows, text highlights, and UI accents, creating a seamless, immersive visual experience that changes with every song.

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Objects/Control%20Knobs.png" alt="Control" width="35" height="35" /> Deep Dive: The Audio Architecture

At the core of Elixir is a highly decoupled, service-oriented audio engine, ensuring that heavy processing and database queries never drop UI frames.

<details>
<summary><b>Click to expand Audio Engine Details</b></summary>
<br>

Elixir uses a combination of `just_audio` and `audio_service`, strictly configured to bypass Android's Scoped Storage limitations by utilizing MediaStore Content URIs (`content://media/external/audio/...`).

*   **5-Band Equalizer**: Users have access to a fully functional, real-time Equalizer spanning 5 critical frequency bands (60Hz, 230Hz, 910Hz, 3.6kHz, 14kHz). It comes with pre-built presets (Bass Boost, Treble Boost, Vocal, Rock) and allows for custom manual tuning.
*   **Gapless Playback & Crossfade**: The audio engine supports true gapless playback for seamless album transitions. For a DJ-like experience, users can configure a custom crossfade duration (0 to 12 seconds) in the settings.
*   **Queue Service Architecture**: Implements a dedicated `_loadAndPlay` mechanism. This buffers the next track dynamically, allowing seamless track switching without destroying the background media handler, ensuring your lock-screen and notification controls remain stable.
*   **Volume Normalization**: Built-in volume normalization ensures that a quiet acoustic track followed by a loud EDM track play at a consistent perceived volume level, saving your ears from sudden spikes.

</details>

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Objects/Floppy%20Disk.png" alt="Database" width="35" height="35" /> Deep Dive: Data Persistence & SQLite

Instead of relying solely on the device's default MediaStore—which can be slow, limited, and prone to losing custom edits—Elixir builds its own robust relational database using `sqflite`.

<details>
<summary><b>Click to expand SQLite Database Details</b></summary>
<br>

*   **Advanced Migration System**: The database utilizes a robust versioning and migration system (currently at schema version 8) to seamlessly upgrade tables, add new columns for features, and alter schemas without ever losing user data.
*   **Dual-State Metadata**: The database intelligently stores both the `raw_title` (the original filename metadata) and the `clean_title` (fetched from APIs or manually edited by the user). This ensures the app can always fall back to the original file data if needed.
*   **Image Caching Engine**: To prevent redundant network calls and save user data, Elixir caches the local device paths to downloaded high-res album banners directly in the database.
*   **Custom Playlists**: Supports creating, editing, renaming, and reordering custom user playlists, stored entirely locally and persistently.

</details>

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Activities/Artist%20Palette.png" alt="Palette" width="35" height="35" /> The UI/UX Experience

Elixir strictly adheres to a premium dark-theme aesthetic (`AppTheme.darkTheme`), highlighted by its signature neon orange accent (`#FF5F1F`). It is designed specifically to reduce eye strain in low-light environments while looking undeniably modern.

### The Rotating Vinyl Navigation
Breaking away from traditional, boring bottom navigation bars, Elixir features an oversized, rotating vinyl record in the center of the screen. 
- When music is playing, the vinyl physically rotates on screen.
- When paused, it stops. 
- Tapping the vinyl seamlessly expands the `NowPlayingPage` with a smooth, hero-style animation.

### The Now Playing Experience
The core of the visual experience. It features:
- An immersive, full-screen playback view.
- Animated, staggered equalizer bars that physically react to the playback state.
- Gesture-based scrubbing (swipe left/right anywhere on the screen to change tracks).
- A beautiful glass-morphism background that deeply blurs the album art behind the controls.

### The Manual Track Editor
Because automated algorithms aren't perfect, Elixir provides ultimate control. If the automated sync fails, users can open the "Edit Track Sheet." Here, they can manually override the title/artist, paste in their own lyrics, upload a custom cover image directly from their gallery, or trigger a highly targeted in-app web fetch using the Deezer API.

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Objects/Chart%20Increasing.png" alt="Analytics" width="35" height="35" /> Analytics & Insights Engine

One of Elixir's most unique features is its ability to act as your personal listening historian.

Every single time a song finishes, the `DatabaseService` quietly logs the timestamp and the exact duration played down to the millisecond. 

The **Analytics Dashboard** takes this raw data and compiles it into beautiful, easy-to-read monthly reports. It provides:
1. **Total Listening Time**: Calculated and formatted into hours and minutes for the current month.
2. **Top 5 Songs**: Your most repeated tracks, ranked.
3. **Top 5 Artists**: Your most heavily played artists.

It provides a "Spotify Wrapped" experience, available year-round, completely locally, without sending your data to any servers.

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Travel%20and%20places/Rocket.png" alt="Rocket" width="35" height="35" /> Releases & Installation

### Availability
As the source code is maintained in a private repository to protect the application's core logic, the application is distributed directly to users via pre-compiled, optimized releases.

### Installation Steps

1.  **Download the APK**
    Navigate to the [Releases](../../releases) tab of this GitHub repository.
    Download the latest `app-release.apk` for your Android device.

2.  **Allow Unknown Sources**
    Before installing, you may need to navigate to your Android Settings > Security and enable "Install from unknown sources" for your web browser or file manager.

3.  **Permissions on First Launch**
    Upon the first launch, Elixir will request storage permissions. Ensure your Android device grants this. Elixir targets Android API 33+ and strictly requires `READ_MEDIA_AUDIO` permissions to scan your local music library. Without this, the app cannot function.

---

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Animated-Fluent-Emojis/master/Emojis/Travel%20and%20places/Map%20of%20Japan.png" alt="Roadmap" width="35" height="35" /> Development Roadmap

Elixir is constantly evolving. Here is the structured roadmap detailing what has been achieved and what is coming next:

*   **Phase 1: Core Playback & Scanning (Completed)**
    *   Implement `on_audio_query` for deep local file detection.
    *   Integrate `just_audio` for reliable background stream handling.
    *   Build media session controls for lock-screen support.
*   **Phase 2: UI & Theming (Completed)**
    *   Design the dark-theme glass-morphic interface and custom navigation.
    *   Implement dynamic palette generation from album art.
    *   Build the rotating vinyl widget.
*   **Phase 3: Data & Analytics (Completed)**
    *   Construct the relational SQLite database for play history and manual metadata edits.
    *   Build the comprehensive Analytics Dashboard for monthly listening reports.
    *   Implement robust schema migrations.
*   **Phase 4: Enrichment & Lyrics (Completed)**
    *   Integrate Deezer and iTunes APIs for automated metadata cleaning.
    *   Implement the `lrclib` synced lyrics engine with real-time UI highlighting.
    *   Build the manual track editor.
*   **Phase 5: Future Enhancements (Planned)**
    *   **Cloud Sync**: Optional opt-in sync for user profiles, custom playlists, and listening history to Firebase.
    *   **Cross-Platform Expansion**: Adapt the framework to support iOS and Desktop (Windows/macOS) targets.
    *   **Advanced Audio FX**: A new DSP engine for Reverb, Pitch shifting, and Speed control.

---

## 📂 Project Structure

For developers and architects interested in the organization, here is how the private source code is laid out:

```text
lib/
├── main.dart                 # App entry point, Service initialization & Theme setup
├── models/                   # Strongly typed data models (Track, Artist, Playlist, Lyrics)
├── services/                 # Core business logic layer
│   ├── audio_service.dart    # Background playback, MediaSession handling
│   ├── database_service.dart # SQLite management & migrations
│   ├── queue_service.dart    # Playback queue, shuffle arrays, & track transitions
│   ├── news_service.dart     # Web scraping for new artist releases
│   └── metadata_sync_service.dart # External API fetching and error handling
├── theme/                    # App design system
│   └── app_theme.dart        # Colors, fonts, shadows, and border radii configurations
└── ui/                       # User Interface Components
    ├── screens/              # Main routing pages
    │   ├── home_page.dart    # Library overview & recent plays
    │   ├── library_page.dart # Full track/artist list views
    │   ├── analytics_page.dart # Data visualization & history
    │   └── settings/         # Configuration screens (Audio Quality, Profile)
    └── widgets/              # Reusable UI parts
        ├── rotating_vinyl.dart # The custom nav bar centerpiece
        ├── animated_equalizer.dart # Staggered audio visualizer
        ├── edit_track_sheet.dart # Modal for manual overrides
        └── synced_lyrics.dart # LRC parser and scrolling view
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<br/>

## 👨‍💻 Author

<h1 align="center">
  🍁 Made by <span style="color: #FF0055;">M</span><span style="color: #00E5FF;">A</span> 🍁
</h1>
