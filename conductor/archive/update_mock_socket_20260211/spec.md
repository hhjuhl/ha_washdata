# Specification - Update mqtt_mock_socket for Enhanced Realism and UI Features

## Overview
This track aims to improve the `mqtt_mock_socket.py` tool to provide more realistic simulation of power data, fix UI issues in the measure feature, improve log visibility, and provide better insights into imported cycle data.

## Functional Requirements

### 1. Enhanced Realism in Data Generation
- **Random Scaling**: Implement random scaling of power amplitude and cycle duration within configurable safe ranges.
- **Real-world Socket Artifacts**:
    - Simulate "early" low values: Occasional reports of low power arriving sooner than the strictly calculated end-of-phase.
    - Polling Rate Non-compliance: Simulate jitter or occasional updates that ignore the strictly configured polling rate, mimicking real-world sensor timing irregularities.

### 2. Measure Feature Fix & Enhancement
- **Fix**: Resolve the bug where highlighting a segment of the cycle graph displays no data.
- **Enhanced Overlay**: The measurement overlay must display:
    - Peak and average power for the highlighted segment.
    - Segment duration.
    - Total energy (Wh) consumed within the segment.
    - Start and end timestamps relative to the beginning of the cycle.
    - Power variance (standard deviation).

### 3. Log Behavior Improvements
- **Auto-scroll Logic**: Implement "Auto-Scroll to Bottom" behavior. New log entries are appended to the bottom.
- **Smart Sticky**: The view automatically scrolls to the bottom to show new entries unless the user has manually scrolled up to inspect the history.

### 4. Imported Cycle Source Data View
- **Cycle Registry**: Display a chronological list of all cycles loaded from files, including name, duration, and peak power.
- **Queue Management**: 
    - Provide a "Next Up" indicator for the currently active or scheduled cycle.
    - Show a visual timeline or queue of upcoming cycles.
    - Allow users to manually select a specific cycle from the list to play next or skip the current one.

## Non-Functional Requirements
- **Performance**: Ensure the UI remains responsive when handling large cycle files or high-frequency logs.
- **Backward Compatibility**: Preserve all existing features of `mqtt_mock_socket.py`.

## Acceptance Criteria
- Data generation includes random scaling and timing artifacts as specified.
- The measure feature displays a comprehensive data overlay when a segment is highlighted.
- The log automatically scrolls to new entries unless the user manualy scrolls up.
- The imported cycle view correctly lists, queues, and allows selection/skipping of cycles.
