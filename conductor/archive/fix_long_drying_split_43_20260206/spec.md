# Specification: [BUG] wash data power doesnt match smart plug power #43

## Overview
A dishwasher cycle with a long passive drying phase (e.g., 2 hours at 0W) is being split into multiple cycles because the integration fails to wait long enough. While some profile-aware deferral logic exists, it appears insufficient for multi-hour pauses, or the verification mechanisms are failing during these extended periods of silence.

## Functional Requirements
- **Extended Pause Support**: Ensure the integration can maintain a single cycle during 0W pauses of 2+ hours if they align with the matched profile's envelope.
- **Robust Envelope Verification**: Investigate and improve `async_verify_alignment` and `verify_profile_alignment_worker` to ensure they accurately identify long drying phases and maintain `verified_pause = True`.
- **Profile-Aware Watchdog Adjustment**: Update the watchdog in `manager.py` to be more lenient during confirmed profile pauses, extending the `effective_low_power_timeout` dynamically based on the expected pause duration in the profile.
- **Dishwasher-Specific Tuning**: Review and potentially increase `DEFAULT_MAX_DEFERRAL_SECONDS` and adjust `min_duration_ratio` logic for dishwashers to accommodate these long terminal pauses.
- **Look-Ahead Logic**: Ensure the system "looks ahead" in the profile to anticipate long 0W periods and locks the cycle state accordingly.

## Non-Functional Requirements
- **Documentation**: Update `IMPLEMENTATION.md` (specifically the "Smart Termination & End Spike Logic" section) to reflect support for multi-hour drying phases and how the envelope verification handles them.
- **Test Coverage**: Add a reproduction test using the user's provided data (if possible) or a synthetic equivalent with a 2-hour 0W pause.

## Acceptance Criteria
- [ ] A cycle with a 2-hour 0W drying phase (matching its profile) completes as a single cycle.
- [ ] `verified_pause` remains active throughout the drying phase as long as power stays at 0W and the profile expects it.
- [ ] The watchdog does not force-end the cycle during a profile-validated long pause.
- [ ] `IMPLEMENTATION.md` and `CHANGELOG.md` are updated with the improvements.

## Out of Scope
- Re-implementing auto-merging of split cycles.
- Global changes to the state machine that affect all devices without profile matching.
