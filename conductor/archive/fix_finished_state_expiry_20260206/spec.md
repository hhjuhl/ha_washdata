# Specification: [BUG] Finished state is not removed

## Overview
The "Finished" state (and other terminal states like "Interrupted") is intended to persist for 30 minutes after a cycle ends to provide visibility in the UI. However, the current implementation relies on new power readings to trigger the expiration, which fails if the smart socket stops publishing updates after a cycle.

## Functional Requirements
- **Background Expiry Timer**: Implement a background timer in `manager.py` that triggers 30 minutes after a cycle enters a terminal state (`finished`, `interrupted`, `force_stopped`).
- **Unified Reset Logic**: Consolidate the state expiration and progress reset. When the 30-minute timer expires:
    - Force the detector state to `OFF`.
    - Reset cycle progress to 0%.
    - Log a debug message indicating auto-expiration.
- **Auto-Cancellation**: If a new cycle starts (new readings arrive that transition the detector out of a terminal state), the expiry timer must be cancelled immediately.
- **Configuration Alignment**: Ensure the existing `progress_reset_delay` configuration is either used for this 30-minute window or documented as being superseded by this unified expiry logic.

## Non-Functional Requirements
- **Reliability**: The timer must survive Home Assistant reloads if possible (via state restoration) or at least be robust during normal operation.
- **Resource Efficiency**: Use `async_track_time_interval` or `async_call_later` efficiently to avoid unnecessary polling.

## Acceptance Criteria
- [ ] The "Finished" state is removed and changed to "Off" after 30 minutes of inactivity, even if no new power readings are received.
- [ ] Progress resets to 0% at the same time the state resets to "Off".
- [ ] Starting a new cycle within the 30-minute window correctly cancels the expiry timer.
- [ ] Debug logs confirm the auto-expiration event.

## Out of Scope
- Changing the 30-minute hardcoded limit (unless decided to make it configurable).
- Modifying how "Finished" is detected (this track focuses only on its removal).
