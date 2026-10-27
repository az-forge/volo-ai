# Delta

Merged into docs/llm/: 2026-10-25

Issue: 21
Approved: 2026-10-25 — record safe OS error details for failed-request
diagnostics and keep nonessential model-load stderr logging from aborting
synthesis

## MODIFIED Requirements

### DSET-local-diagnostics

Previously: bounded local diagnostic events, Logs and export during first-run
setup, and sanitized failed-request context that keeps only the exception type
for most operations.

Baseline:

````md
### DSET-local-diagnostics

The desktop app SHALL retain bounded local diagnostic events, make Logs and user-triggered export available from first-run setup before providers are ready, and include sanitized failed-request context in home-path-redacted reports without request payloads or generated content.

#### Scenario: Open Logs during setup

- GIVEN one or more providers are not ready or the sidecar is unavailable
- WHEN the user opens Logs from first-run setup
- THEN the app opens Logs and allows an explicit export, using a frontend-only report marked incomplete when the sidecar cannot respond

#### Scenario: Export a failed request

- GIVEN a failed request has diagnostic details
- WHEN the user chooses Export
- THEN the report includes its timestamp, operation, request ID, provider and last-known setup stage or asset when available, error code, sanitized error message, elapsed time, and sanitized stack details, while excluding request payloads, synthesized text, audio, and voice content

Evidence: `apps/desktop/src/App.tsx`, `apps/desktop/src/lib/diagnostics.ts`, `apps/desktop/src/lib/sidecar.ts`, `src/tts_mcp/desktop.py`, `src/tts_mcp/desktop_diagnostics.py`, `tests/test_desktop_diagnostics.py`
````

Replacement:

````md
### DSET-local-diagnostics

The desktop app SHALL retain bounded local diagnostic events, make Logs and user-triggered export available from first-run setup before providers are ready, include sanitized failed-request context in home-path-redacted reports without request payloads or generated content, and keep nonessential model-load stderr logging from aborting synthesis.

#### Scenario: Open Logs during setup

- GIVEN one or more providers are not ready or the sidecar is unavailable
- WHEN the user opens Logs from first-run setup
- THEN the app opens Logs and allows an explicit export, using a frontend-only report marked incomplete when the sidecar cannot respond

#### Scenario: Export a failed request

- GIVEN a failed request has diagnostic details
- WHEN the user chooses Export
- THEN the report includes its timestamp, operation, request ID, provider and last-known setup stage or asset when available, error code, sanitized error message, elapsed time, and sanitized stack details, while excluding request payloads, synthesized text, audio, and voice content

#### Scenario: Failed request raises an OS error

- GIVEN a failed request whose exception is an OSError, or whose exception was raised from an OSError
- WHEN the user exports diagnostics
- THEN the event's error message retains only safe OS details — errno, the Windows winerror code when present, and the OS strerror text — and never the exception's filename, request content, or full exception formatting

#### Scenario: stderr is unavailable during model load

- GIVEN the sidecar's stderr stream raises OSError when written
- WHEN the OmniVoice engine logs its nonessential model-load lines
- THEN loading continues, and each failed stderr write is itself recorded as a bounded local diagnostic event

Evidence: `apps/desktop/src/App.tsx`, `apps/desktop/src/lib/diagnostics.ts`, `apps/desktop/src/lib/sidecar.ts`, `src/tts_mcp/desktop.py`, `src/tts_mcp/desktop_diagnostics.py`, `src/tts_mcp/engine.py`, `tests/test_desktop_diagnostics.py`, `tests/test_desktop.py`, `tests/test_engine.py`
````
