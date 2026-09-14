# Covasify v4.1.2

**⚠️ Development transparency:** Covasify has been developed with AI assistance. Code changes are directed, reviewed, and tested by the human maintainer before release. Feedback and code improvements are welcome. **Project lineage:** Covasify was originally created by **DocTrintignant**, restored and substantially developed by **Lag0matic** from **v3.0.0 through v4.1.1**, and is maintained and developed by **DocTrintignant** again from **v4.1.2** onward. **For the sake of simplicity in management, from version 4.1.2 onwards, the two development branches diverge**

Voice-controlled Spotify integration for [COVAS:NEXT](https://ratherrude.github.io/Elite-Dangerous-AI-Integration/). Play music, control playback, and bind tracks to custom voice phrases — all hands-free.

**⚠️ Requires Spotify Premium** — Free accounts cannot use playback control features.

## What It Does

- **Play by voice** — tracks, albums, artists, playlists, and your Liked Songs
- **Full playback control** — pause, skip, seek, volume, shuffle, repeat
- **Liked Songs** — save or remove the current track by voice
- **Track bindings** — bind any track to a custom phrase and play it instantly
- **Ambient now-playing status** — COVAS always knows what's playing without being asked
- **Live now-playing HUD** — ask COVAS to show a now-playing overlay on the GenUI display, complete with album art

## How It Works

Covasify connects to Spotify via OAuth and registers a set of voice-activated tools with COVAS:NEXT. A status generator passively pushes the current track and play/pause state into COVAS's context every turn at no extra cost, so it can reference what's playing naturally in conversation without needing to call a tool first.

---

## Under Development

Covasify is actively maintained and is being expanded further **within its Spotify/music-control scope before broader music-source integration is considered**. The current development target is to expose more of Spotify's useful API surface while preserving the compact, token-efficient COVAS:NEXT integration introduced in v4.0.0.

Planned work includes:

- **Queue management** — add tracks to the Spotify queue and inspect a small bounded set of upcoming items.
- **Spotify Connect device control** — discover available Spotify devices, transfer playback, and support a preferred playback device instead of relying on the first available device.
- **Personal playlist resolution** — resolve the user's own playlists before falling back to general Spotify playlist search.
- **Top Tracks / Top Artists** — use Spotify's existing `user-top-read` permission for short-, medium-, and long-term listening summaries and playback.
- **Recently Played** — expose recent listening history for recall and replay, using Spotify's dedicated recent-history permission.
- **Playlist management** — create playlists and support practical voice-driven operations such as adding, removing, and renaming items/playlists where the current Spotify API permits it.
- **Broader library support** — modernize saved-library operations and extend useful library browsing while remaining compatible with Spotify's current Development Mode API.
- **Bounded artist/album browsing** — expose useful discography and album-track information without returning large catalog dumps to the language model.
- **Spotify 2026 API compatibility** — replace or gracefully retire legacy endpoints no longer available to current Development Mode applications, and improve handling of current quota/rate-limit behavior.

### Development constraints

The expansion is intentionally designed around efficiency:

- Preserve the consolidated COVAS action model rather than returning to many single-purpose tools.
- Keep Spotify searches, ranking, matching, paging, and filtering inside Covasify wherever possible.
- Return only the minimum useful result to the LLM instead of large playlists, libraries, queues, or search-result sets.
- Keep queue, history, library, and playlist-detail requests on demand rather than adding new continuous background polling.
- Keep ambient status concise so new functionality does not add unnecessary prompt cost to unrelated COVAS interactions.
- Preserve existing working playback, track-binding, OAuth, persistent-data, and GenUI behavior unless a compatibility fix specifically requires change.

These items are **development targets, not functionality present in v4.1.2**.

---

## Setup

### Step 1 — Create a Spotify App

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and log in with your Spotify account.
2. Click **Create App**.
3. Enter an app name such as `Covasify` or `COVAS Spotify`.
4. Add this exact **Redirect URI**:
   ```text
   http://127.0.0.1:8888/callback
   ```
5. Accept Spotify's terms and save the app.
6. Open the **User Management** tab, click **Add user**, and add the Spotify account you will use with COVAS:NEXT.
7. Open the app's **Settings** and copy the **Client ID** and **Client Secret**.

### Step 2 — Install the Plugin

> ⚠️ **GitHub extraction note:** When downloading a release from GitHub, the zip file may extract to a folder with a version suffix such as `Covasify-v4.1.2`. Rename this folder to just `Covasify` before placing it in your plugins directory, otherwise COVAS:NEXT may not load it correctly.

1. Download the latest release and extract it.
2. Rename the folder to `Covasify` if necessary.
3. Place the `Covasify` folder in:
   ```text
   %appdata%\com.covas-next.ui\plugins\
   ```
4. Dependencies are bundled — no installation step is required.
5. Restart COVAS:NEXT.

### Step 3 — Configure Covasify

Open the COVAS:NEXT menu → **Covasify Spotify Integration** settings and enter:

| Field | What to enter |
|---|---|
| **Client ID** | Your Spotify app's **Client ID** |
| **Client Secret** | Your Spotify app's **Client Secret** |
| **Redirect URI** | `http://127.0.0.1:8888/callback` |

Save the settings, then start your COVAS chat session. On first authorization, your browser will open so you can log in to Spotify and approve access.

**Requirements:**
- Spotify Premium account (mandatory — free accounts cannot control playback)
- Active Spotify device (desktop app, mobile, or web player must be open)
- Spotify app configured with the exact redirect URI shown above
- Your Spotify account added under the app's **User Management** when required by Spotify Development Mode

---

## Voice Commands

### Playing Music

```
"Play Bohemian Rhapsody"          # Track search
"Play Abbey Road album"           # Album
"Play Queen"                      # Artist (shuffled)
"Play Queen's top tracks"         # Top 10 most popular tracks
"Play workout playlist"           # Playlist by name
"Play Liked Songs"                # Your saved library
```

### Playback Control

```
"Pause" / "Resume" / "Stop"
"Next" / "Previous" / "Restart"
"Seek to 2:30"
"Volume up" / "Volume down" / "Set volume to 50" / "Mute"
"Shuffle on" / "Shuffle off"
"Repeat track" / "Repeat playlist" / "Repeat off"
```

### Library

```
"What's playing?"                 # Full track detail including progress
"Save this track"                 # Add to Liked Songs
"Remove this track"               # Remove from Liked Songs
```

### Track Bindings

Bind any currently playing track to a custom phrase and play it back instantly by saying that phrase.

```
"Bind this to workout intro"      # Bind current track to a phrase
"Workout intro"                   # Play the bound track
"List bindings"                   # See all your bindings
"Unbind workout intro"            # Remove a specific binding
"Unbind all"                      # Clear all bindings
```

---

## Now-Playing HUD (GenUI)

Covasify provides a live now-playing projection to the COVAS:NEXT GenUI overlay system. Once connected, ask the AI to display it:

```
"Show what's playing on the HUD"
"Add a now-playing widget to the display"
"Put the current track on screen"
```

The overlay updates in real time as tracks change — no AI turn required and no token cost after the initial setup. The projection includes track name, artist, album, album art, playback progress, shuffle state, and repeat mode, giving the AI everything it needs to render a rich now-playing card.

To adjust the style:
```
"Make the now-playing widget more transparent"
"Move the now-playing card to the top right"
"Make the album art larger"
```

---

## Troubleshooting

**"No active Spotify devices found"**
- Open Spotify on any device and start playing something first, then try again

**"Not connected to Spotify"**
- Check your credentials are correctly entered in the plugin settings
- Delete `_spotify_cache` from the plugin's data folder and restart COVAS to re-authenticate

**Need to re-authorise**
- Delete `_spotify_cache` from the plugin data folder (found under `%appdata%\com.covas-next.ui\plugin_data\` by plugin GUID)
- Restart COVAS — your browser will open for re-auth on first command

**Binding doesn't play immediately**
- Say the phrase again — first-attempt retries are occasionally needed

---

## What's NOT Possible

Due to Spotify API restrictions introduced in November 2024:
- Radio / recommendations (API deprecated for new apps)
- Endless smart queue (use artist or playlist playback instead)
- Related artists suggestions (API blocked)

---

## Files

```text
Covasify/
  Covasify.py              # Main plugin
  manifest.json            # Plugin metadata
  README.md                # Documentation
  LICENSE-MIT              # Preserved upstream MIT license
  THIRD_PARTY_NOTICE.md    # Upstream lineage and attribution
  deps/                    # Bundled Python dependencies
```

### Bundled Dependencies

Covasify includes its Python dependencies in the `deps/` folder, so users do not need to install them separately.

```text
spotipy>=2.23.0
requests>=2.31.0
```

**Persistent data** (track bindings and OAuth token cache) is stored in COVAS:NEXT's plugin data folder by plugin GUID — not inside the plugin folder itself. Your bindings and login survive updates and reinstalls.

---

## Version History

**v4.1.2** — Maintenance and development resumed by **D. Trintignant**, based on Lag0matic's v4.1.1 development line.
- Fixed playback startup so requesting a new track no longer briefly resumes the previously paused track first
- Moved OAuth cache and track bindings to COVAS:NEXT's persistent plugin-data directory so they survive normal plugin updates and reinstalls
- Removed raw plugin-settings logging that could expose Spotify credentials in COVAS:NEXT logs

**v4.1.1** — Developed by **Lag0matic and AI**. Added GenUI now-playing projection. Track name, artist, album, album art, progress, shuffle and repeat state are all exposed to the GenUI overlay system and update in real time as tracks change — zero token cost after initial setup.

**v4.1.0** — Developed by **Lag0matic and AI**. Improved track search accuracy. Added separate artist field for more precise matching, smarter scoring that heavily penalises covers, remixes, karaoke and live versions.

**v4.0.0** — Major token optimisation refactor by Lag0matic and AI
- Consolidated 15 tools down to 5 — ~65–70% reduction in per-turn LLM token cost
- Added ambient now-playing status — COVAS always knows the current track and play/pause state without a tool call
- Seek moved into `covasify_control` — one less tool in the LLM's context
- Removed background polling thread — no unnecessary Spotify API calls between commands
- Pause/resume state tracked locally at zero API cost

**v3.0.0** — Re-worked by Lag0matic and AI to function again

**v2.0.0** — Settings UI integration, credential management via COVAS:NEXT menu

**v1.0.0** — Initial release

---

## Credits

**Original author: DocTrintignant  
**v3.0.0–v4.1.1 restoration and development**: [Lag0matic](https://github.com/lag0matic/TRINTIGNANT-COVAS-NEXT-PLUGINS/tree/main/Covasify)  
**COVAS:NEXT**: https://ratherrude.github.io/Elite-Dangerous-AI-Integration/  
**Spotify API**: Spotipy library

---

## License

Covasify v4.1.2 incorporates the development line distributed by Lag0matic through v4.1.1 under the **MIT License**. The applicable MIT notice is preserved in [`LICENSE-MIT`](LICENSE-MIT), with lineage documented in [`THIRD_PARTY_NOTICE.md`](THIRD_PARTY_NOTICE.md).

New original contributions by DocTrintignant are distributed under the repository-level **PolyForm Perimeter License 1.0.1**, to the extent applicable. MIT-licensed upstream portions remain subject to their original MIT terms.
