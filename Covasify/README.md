# Covasify v4.0.0

Covasify is the maintained Spotify integration plugin for [COVAS:NEXT](https://ratherrude.github.io/Elite-Dangerous-AI-Integration/), providing voice-controlled music playback, playback controls, track bindings, and Spotify library actions.

> **Project lineage:** Covasify v4.0.0 is the updated maintained version of the original Covasify plugin by **DocTrintignant**. It builds on the restoration and improvements made by **Lag0matic** in his [TRINTIGNANT-COVAS-NEXT-PLUGINS / Covasify fork](https://github.com/lag0matic/TRINTIGNANT-COVAS-NEXT-PLUGINS/tree/main/Covasify). Development has since continued here against current COVAS:NEXT and Spotify Web API behavior.

**Spotify Premium is required** for Spotify playback-control features.

## Development Transparency

Covasify is an **AI-coded software project developed under human direction**. Code development, revision, debugging, and maintenance are performed primarily through generative-AI coding systems.

Project goals, requirements, testing, acceptance, maintenance direction, and publication remain under human control and are directed by **DocTrintignant**. AI-generated changes are reviewed and tested before being accepted.

## What It Does

- Play Spotify tracks by voice
- Play albums, artists, artist top tracks, playlists, and Liked Songs
- Pause, resume, stop, skip, restart, seek, and control volume
- Control shuffle and repeat modes
- Get information about the currently playing track
- Save or remove the current track from Liked Songs
- Bind the current track to a custom voice phrase
- Play, list, remove, or clear saved track bindings
- Report Covasify cache statistics

## How It Works

Covasify connects COVAS:NEXT to Spotify through the Spotify Web API using Spotipy and OAuth.

The plugin registers Spotify actions with COVAS:NEXT and keeps persistent runtime data outside the plugin installation folder. OAuth tokens and track bindings are stored in COVAS:NEXT's plugin-data directory so they survive normal plugin updates.

Covasify also uses a small reliability/cache layer around appropriate Spotify API requests to reduce unnecessary repeated calls.

## Setup

### 1. Create a Spotify Developer App

1. Open the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard).
2. Create a new app.
3. Select **Web API**.
4. Add this exact Redirect URI:

   ```text
   http://127.0.0.1:8888/callback
   ```

5. Copy the app's **Client ID** and **Client Secret**.
6. If your Spotify app is operating in Development Mode, add the Spotify account that will use Covasify under the app's **User Management** section.

Covasify does **not** require the Spotify Web Playback SDK.

### 2. Install Covasify

Copy the `Covasify` folder to:

```text
%APPDATA%\com.covas-next.ui\plugins\
```

Then restart COVAS:NEXT.

### 3. Configure Spotify Credentials

Open the COVAS:NEXT settings for **Covasify Spotify Integration** and enter:

```text
Client ID:      <your Spotify app Client ID>
Client Secret:  <your Spotify app Client Secret>
Redirect URI:   http://127.0.0.1:8888/callback
```

Start or restart the COVAS session. On first authorization, Spotify should open in your browser and ask you to approve access.

### Requirements

- Spotify Premium account
- A Spotify Developer app with **Web API** enabled
- An active Spotify playback device such as the desktop app, mobile app, or web player
- The Spotify account authorized for the app when Development Mode requires explicit user access

## Voice Commands

Natural-language phrasing is handled by COVAS:NEXT, so exact wording may vary.

### Play Music

```text
"Play Bohemian Rhapsody"
"Play Abbey Road album"
"Play Queen"
"Play Queen's top tracks"
"Play workout playlist"
"Play Liked Songs"
```

### Playback Control

```text
"Pause"
"Resume"
"Stop"
"Next"
"Previous"
"Restart"
"Seek to 2:30"
"Volume up"
"Volume down"
"Set volume to 50"
"Mute"
"Shuffle on"
"Shuffle off"
"Repeat track"
"Repeat playlist"
"Repeat off"
```

### Library

```text
"What's playing?"
"Save this track"
"Remove this track"
```

### Track Bindings

```text
"Bind this to workout intro"
"Workout intro"
"List bindings"
"Unbind workout intro"
"Unbind all"
```

### Diagnostics

```text
"Test Covasify"
"Show Covasify cache stats"
```

## Persistent Data and Privacy

Covasify does not require Spotify credentials or OAuth tokens to be committed with the plugin source.

The supported configuration path is the COVAS:NEXT settings interface. Runtime data is stored under COVAS:NEXT's plugin-data directory, including:

- `_spotify_cache` — Spotify OAuth token cache
- `spotify_bindings.json` — saved phrase-to-track bindings

These runtime files are excluded from this repository.

Do not share the contents of `_spotify_cache` or your Spotify Client Secret.

## Re-Authorizing Spotify

If you replace the Spotify Developer app, rotate credentials, or need to force a clean OAuth authorization:

1. Close COVAS:NEXT.
2. Delete Covasify's `_spotify_cache` from its COVAS:NEXT plugin-data folder.
3. Update the Client ID and Client Secret in Covasify settings if they changed.
4. Restart COVAS:NEXT and start a COVAS session.
5. Complete the Spotify authorization in the browser.

Deleting the OAuth cache does not remove your source files or Git repository.

## Troubleshooting

### "Not connected to Spotify"

- Confirm the Client ID and Client Secret belong to the current Spotify Developer app.
- Confirm the Redirect URI is exactly:

  ```text
  http://127.0.0.1:8888/callback
  ```

- Confirm **Web API** is enabled for the Spotify app.
- If using Development Mode, confirm the Spotify account is listed under the app's User Management.
- Delete `_spotify_cache` and authorize again.
- Run `Test Covasify` and check the COVAS:NEXT log for `COVASIFY:` initialization messages.

### "No active Spotify devices found"

Open Spotify on at least one device and begin playback once, then try the command again.

### A previously paused song starts briefly before the requested song

Covasify v4.0.0 no longer forces a playback transfer that can resume the previous paused track before the requested song starts. If this behavior is still observed, capture the relevant `COVASIFY:` log lines and report it as a playback-start issue.

## Spotify API Limitations

Spotify API availability can change independently of Covasify. Some recommendation/radio-style functions that were available to older Spotify applications are not available to newer applications, so Covasify focuses on supported playback, library, search, playlist, and control operations.

## Files

```text
Covasify/
├── Covasify.py
├── manifest.json
├── README.md
└── deps/
```

Persistent OAuth and binding data is stored separately under COVAS:NEXT's plugin-data directory.

## Version History

**v4.0.0** - Updated Covasify for current Spotify application and OAuth behavior, including current Web API setup, loopback authorization through `127.0.0.1`, Development Mode authorization handling, clearer initialization diagnostics, and corrected persistent OAuth-cache handling. Playback startup was also changed so requesting a new song does not first force the previously paused Spotify track to resume.

**v3.0.3** - Prevented a previously paused track from briefly resuming before a newly requested track starts.

**v3.0.2** - Added Spotify initialization diagnostics so connection failures can be surfaced directly through the plugin test action.

**v3.0.1** - Updated Spotify OAuth handling for current Development Mode behavior and corrected OAuth-cache troubleshooting.

**v3.0.0** - Re-worked by Lag0matic and AI to function again

**v2.0.0** - Settings UI integration and credential management through the COVAS:NEXT menu.

**v1.0.0** - Initial release.

## Credits

**Author / Maintainer**: D. Trintignant  
**v3 restoration and improvement basis**: [Lag0matic / TRINTIGNANT-COVAS-NEXT-PLUGINS](https://github.com/lag0matic/TRINTIGNANT-COVAS-NEXT-PLUGINS/tree/main/Covasify)  
**COVAS:NEXT**: https://ratherrude.github.io/Elite-Dangerous-AI-Integration/  
**Spotify integration**: Spotipy / Spotify Web API

## License

This repository is distributed under the **PolyForm Perimeter License 1.0.1**. See the repository-level [`LICENSE`](../LICENSE) file for the governing terms.
