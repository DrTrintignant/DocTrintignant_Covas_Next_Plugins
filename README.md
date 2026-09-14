# COVAS-NEXT-PLUGINS

This repository contains plugin packages for [COVAS:NEXT](https://ratherrude.github.io/Elite-Dangerous-AI-Integration/).

The repository now distinguishes between the actively maintained plugin, plugins under development and older plugins retained only for historical/reference purposes. **Covasify is both the active maintained plugin and under continued feature development.**

## Development Transparency

The plugins in this repository are **AI-coded passion projects developed under human direction**. Code development, revision, debugging, and maintenance are performed primarily if not exclusively through generative-AI coding systems.

Project goals, requirements, architecture, design decisions, testing, physical validation where applicable, acceptance criteria, maintenance direction, and publication remain under human control and are directed by **DocTrintignant**. AI-generated code is reviewed and tested before being accepted, but —as with any AI code written without the supervision of a professional coder/developer— users should understand that defects may still exist regarding both code quality and optimization. Use at your own discretion.

In short: **AI-coded, but human-directed, tested, and accepted by a non-professional coder.**

This disclosure is intentional: the repository is provided openly and the use of AI in its development should be equally transparent.

## Active / Maintained Plugins

* **Covasify** — Spotify integration with voice-controlled playback and track binding. This is the currently maintained plugin in this repository, with further Spotify-native functionality under active development.

## Under development

* **Covasify — Spotify capability expansion** — Planned work focuses on cost-effective Spotify functionality before broadening to other music sources: queue management, Spotify Connect device discovery/transfer, correct personal-playlist resolution, Top Tracks/Top Artists, Recently Played, basic playlist creation/editing, broader library/API compatibility, and bounded artist/album browsing. Development will preserve the consolidated low-token COVAS action model and avoid large ambient datasets or unnecessary background polling.
* **Elite Dangerous Lighting** — Reactive RGB lighting engine for Elite Dangerous, driven by live game state and designed for deterministic control of Razer Chroma and compatible hardware (Standalone).
* **Chromas Next** — COVAS:NEXT lighting integration layer under development, providing voice/operator control over Elite Dangerous Lighting (Plugin).

## Archived / Obsolete Plugins

The following plugins are **archived, obsolete, and no longer maintained**. They are retained for historical/reference purposes and may no longer be compatible with current COVAS:NEXT releases.

* **Songbird_ARCHIVED_OBSOLETE** — Former voice-controlled sound-effects plugin using Freesound and a local soundboard.
* **Covinance_ARCHIVED_OBSOLETE** — Former Elite Dangerous commodity trading and market-analysis plugin using the Ardent API.

Do not treat the archived folders as current supported plugins.

## Installation

For the maintained plugin:

1. Download or clone this repository.
2. Copy the `Covasify` folder to `%appdata%\com.covas-next.ui\plugins\`.
3. Restart COVAS NEXT.
4. Configure the Covasify settings in the COVAS NEXT menu.

The archived/obsolete plugin folders are not recommended for installation.

## Registered Actions — Active Plugin

### Covasify

The following actions are registered by the current Covasify plugin. Availability of individual Spotify operations can still depend on Spotify account/API restrictions.

* `covasify_test` — Test plugin and Spotify connection status.
* `covasify_play_track` — Search for and play a track.
* `covasify_play_album` — Play a complete album.
* `covasify_play_artist` — Play music from an artist.
* `covasify_play_top_tracks` — Play an artist's top tracks.
* `covasify_play_playlist` — Play a playlist or Liked Songs.
* `covasify_control` — Pause, resume, next, previous, restart, stop, volume, mute, shuffle, and repeat controls.
* `covasify_seek` — Seek to a time position.
* `covasify_current` — Get current track information.
* `covasify_save_track` — Add the current track to Liked Songs.
* `covasify_remove_track` — Remove the current track from Liked Songs.
* `covasify_bind_track` — Bind the current track to a custom phrase.
* `covasify_play_bound` — Play a previously bound track.
* `covasify_list_bindings` — List track bindings.
* `covasify_unbind` — Remove a specific binding.
* `covasify_unbind_all` — Clear all bindings.
* `covasify_cache_stats` — View Covasify cache statistics.

For installation, setup, voice-command examples, and troubleshooting, see [`Covasify/README.md`](Covasify/README.md).

## Configuration

**Covasify** requires Spotify API credentials and a Spotify Premium account for playback-control features. See the Covasify README for the current setup procedure.

## Canonical Repository

This repository is the canonical upstream for these plugins. Archived copies are retained here only to preserve project history; the active maintained release is Covasify, with further Covasify capability expansion, Elite Dangerous Lighting and Chromas Next under development.

## License

See [`LICENSE`](LICENSE) for the current terms governing use, copying, modification, and redistribution.
