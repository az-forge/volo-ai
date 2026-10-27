# Changelog

## v0.3.0

### Highlights

- Audio History is now paginated into numbered pages of 20 entries, with direct page links, previous/next controls, and the total entry count.
- Logs can be opened from the first-run setup screen before providers are ready, and a report exported while the engine is offline is saved as a frontend-only report marked incomplete.
- Exported diagnostics now include failed-request context — request ID, provider, setup stage or asset, elapsed time, and sanitized stack details — while still redacting home paths and excluding request text and generated audio. OS error failures also record the error number, Windows error code when present, and OS error text, never the file path.
- OmniVoice model loading no longer fails when the desktop's error stream is unavailable; a failed write is itself recorded in local diagnostics.
- The app stays responsive while a render is in flight: status and voice requests keep answering, and a synthesis still waiting in queue can be cancelled.
- Settings reports each provider's compute backend (NVIDIA CUDA, Apple Silicon, or CPU) and the engine's peak and GPU memory usage.
- The app and website now include a Ko-fi link for anyone who wants to support the project.
- On Windows, status checks on the packaged CPU-only build report readiness and device without waiting for the Torch runtime to load.

## v0.2.0

### Highlights

- Audio History entries can be renamed and their local file path copied to the clipboard.
- An opt-in local REST API exposes desktop speech and voice profiles at `http://127.0.0.1:8765/api/v1`. It starts only from Settings, binds to loopback, and stops when the app closes.
- The desktop app checks for newer stable releases and shows an update notice with a link to the official release.
- The website now serves a localized changelog page that reads this file and links each version to its exact installer downloads.
- A Transcribe view turns a public YouTube or Facebook video of up to 60 minutes into a timestamped `.srt` file with the local Whisper model.
- Long scripts no longer fail after five minutes of rendering; a running render shows its elapsed time and can be cancelled.
- MCP tools now return the reason a call failed instead of a generic "Error executing tool" message.

## v0.1.1

### Highlights

- First-run setup prepares both OmniVoice and VieNeu-TTS before opening the synthesis workspace.
- Missing model assets download together, with progress and errors shown for each provider. Complete assets are reused.
- Settings still lets you prepare either provider individually.
