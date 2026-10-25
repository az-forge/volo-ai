# Volo AI 0.3.0

## Highlights

- Audio History is now paginated into numbered pages of 20 entries, with direct page links, previous/next controls, and the total entry count.
- Logs can be opened from the first-run setup screen before providers are ready, and a report exported while the engine is offline is saved as a frontend-only report marked incomplete.
- Exported diagnostics now include failed-request context — request ID, provider, setup stage or asset, elapsed time, and sanitized stack details — while still redacting home paths and excluding request text and generated audio. OS error failures also record the error number, Windows error code when present, and OS error text, never the file path.
- OmniVoice model loading no longer fails when the desktop's error stream is unavailable; a failed write is itself recorded in local diagnostics.
- The app stays responsive while a render is in flight: status and voice requests keep answering, and a synthesis still waiting in queue can be cancelled.
- Settings reports each provider's compute backend (NVIDIA CUDA, Apple Silicon, or CPU) and the engine's peak and GPU memory usage.
- The app and website now include a Ko-fi link for anyone who wants to support the project.
- On Windows, status checks on the packaged CPU-only build report readiness and device without waiting for the Torch runtime to load.
