# Implementation Plan: Fix 'Finished' state is not removed

## Phase 1: Investigation & Reproduction (Red Phase)
- [x] Task: Analyze current timer implementation in `manager.py` (`_check_progress_reset` and `_start_progress_reset_timer`). 5ee1892
- [x] Task: Create a reproduction test `tests/repro/test_state_expiry.py` that simulates a cycle finishing and then "silence" (no readings) for 31 minutes. 5ee1892
- [x] Task: Verify that the test fails (state remains "Finished" after 31 minutes). 5ee1892
- [x] Task: Conductor - User Manual Verification 'Phase 1: Investigation & Reproduction' (Protocol in workflow.md) 458e20e

## Phase 2: Implementation (Green Phase) [checkpoint: 5811dfb]
- [x] Task: Consolidate `_check_progress_reset` into a unified `_handle_state_expiry` logic in `manager.py`. 60d064a
- [x] Task: Update `_start_progress_reset_timer` (or rename to `_start_state_expiry_timer`) to use the 30-minute threshold. 60d064a
- [x] Task: Implement the force-reset logic (Detector state -> OFF, Progress -> 0%) in the timer callback. 60d064a
- [x] Task: Ensure the timer is stopped when a new cycle is detected in `manager.py`. 60d064a
- [x] Task: Verify the reproduction test now passes (Green Phase). 60d064a
- [x] Task: Conductor - User Manual Verification 'Phase 2: Implementation' (Protocol in workflow.md) 5811dfb

## Phase 3: Verification & Quality Assurance
- [x] Task: Run integration tests (`test_integration_flow.py`) to ensure no regressions in cycle transitions. 3356c8a
- [x] Task: Verify code coverage for the new expiry logic (>80%). 545a236
- [ ] Task: Conductor - User Manual Verification 'Phase 3: Verification & Quality Assurance' (Protocol in workflow.md)
