 # Changelog

## [Named AI Providers] - {PR_MERGE_DATE}

### Added
- **OpenAI**, **Anthropic**, and **OpenRouter** as explicit options in the `AI Refinement Method` dropdown. Pick one, provide your API key, and pick a model — the extension uses the right endpoint automatically. No need to figure out base URLs.

### Changed
- The previous `Ollama/External API` option is now labeled `Ollama / Custom OpenAI-compatible` to reflect that it covers any user-provided OpenAI v1 endpoint.
- `Custom API Endpoint` (formerly `Ollama/API Endpoint`) and related field labels/descriptions clarified — endpoint is only consulted for the Ollama/Custom option; the named providers use built-in endpoints.
- Renamed internal helper `refineWithOllama` to `refineWithOpenAICompatible` since it now serves four providers. Error and status messages interpolate the actual provider name instead of always saying "Ollama".

## [Recording Elapsed Time] - {PR_MERGE_DATE}

### Added
- Recording-screen waveform header now shows the elapsed recording time (`M:SS`) updated every animation tick (~150ms). Helpful when you're not sure if recording is still active, and useful for staying under model context windows on long dictations.

## [History Improvements] - {PR_MERGE_DATE}

### Added
- **History Size Limit** preference — replaces the hardcoded 100-item cap. Set any positive integer to choose a custom cap, or set to `0` (or leave empty) for unlimited.
- **Save Dictations to History** preference (checkbox, default on) — turn off if you rely on Raycast Clipboard History (Pro) for retroactive access and don't want a duplicated copy in this extension's storage. When off, the Dictation History command will just show whatever was already saved before.
- Dictation History search now matches the full transcription text, not just the first 70 characters of each entry's title. Implemented by passing the full text via `keywords` on each `List.Item` so Raycast's built-in filter sees all of it.

## [Whisper Engine Preferences] - {PR_MERGE_DATE}

### Added
- **Language** preference — pass a specific ISO 639-1 code to `whisper-cli` (`-l`) instead of always auto-detecting. Forcing the correct language usually beats auto-detect on short clips. Defaults to `auto`.
- **CPU Threads** preference — pass a thread count to `whisper-cli` (`-t`). Leave empty for the whisper.cpp default. Useful for tuning transcription speed vs system responsiveness.
- **Initial Prompt** preference — pass an initial prompt to `whisper-cli` (`--prompt`) to bias transcription toward specific vocabulary, punctuation, or style. Distinct from AI refinement — this happens inside Whisper itself, before transcription completes.
- **Translate to English** preference — pass `--translate` to `whisper-cli` so the output is English translation rather than source-language transcription.

 ## [0.1.0] - 2025-06-05

 ### Added
- Initial release of **Whisper Dictation** extension
  - Local transcription using `whisper.cpp`
  - Download and manage Whisper models within Raycast
  - AI-based refinement via Raycast AI or Ollama/OpenAI-compatible APIs
  - Dictation history with browse, copy, and paste capabilities
  - Configurable default actions (paste, copy, or manual)

## [0.1.1] - {PR_MERGE_DATE}

### Added
- Preference to both copy and paste transcribed text automatically
- Added separate commands for dictation and dictation with AI refinement
  - This gives more flexibility and how and when each command is called
- Added shortcut to skip refinement for a sesssion during the prompt selection menu (if configured)

## [Configurable Waveform Width] - {PR_MERGE_DATE}

### Added
- New `Waveform Width` preference (dropdown of presets: 50 / 60 / 70 / 80 / 90 / 105 / Custom) to control the width of the recording waveform animation. Pick a smaller value if the animation wraps onto multiple lines for your Raycast window mode/text size combination.
- New `Custom Waveform Width` preference (text field) for users who want an exact value outside the presets. Used only when `Waveform Width` is set to `Custom`. Falls back to 70 on invalid input.

### Changed
- Default waveform width reduced from 105 to 70 characters so the animation fits a default-width Raycast window without wrapping. Users who prefer the original look can select `105 (Original)` in preferences.
- Recording header switches to a shorter `RECORDING (Enter to stop)` label for waveform widths below 40 columns so it doesn't overflow the visualizer area on very narrow custom widths.

### Performance
- Waveform renderer hoists the per-column base-amplitude curve into a `useMemo` keyed on `waveformWidth` and precomputes the per-column normalized amplitude once per frame instead of inside the per-cell loop. Cuts `Math.sin` calls per frame from `4 × width × height` to `width` (~72× fewer at default settings) and removes redundant work from the inner loop.

- Added shortcut to skip refinement for a session during the prompt selection menu (if configured)
