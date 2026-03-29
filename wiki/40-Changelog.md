# Changelog
## [0.5.8] - 29/03/2026
### Added
- Telemetry component: runs as a separate thread, sends event data to an external telemetry service.
- Control component: runs as a separate thread, allows the BS to receive commands from an external control service.
- Sample systemd service file.
### Changed
- `tetra-bluestation` executable renamed to `bluestation-bs`, to be complemented by new `bluestation-telemetry` and `bluestation-control` executables for remote telemetry and control features.
- Various internal refactors.
## [0.5.7] - 28/03/2026
Interim version in non-main branch.
## [0.5.6] - 25/03/2026
### Added
- Running version is now printed on startup.
### Changed
- Revamped SDR device selection, now featuring automatic device detection.
## [0.5.5] - 25/03/2026
### Fixed
- Resolved most LLC issues causing PTT denied behavior.
## [0.5.4] - 23/03/2026
### Fixed
- Improved compatibility with Cleartone MSes.
## [0.5.3] - 19/03/2026
### Added
- Support for PlutoSDR and Pluto+ (additional setup required, see config file and/or documentation).
### Changed
- Various fixes in the SoapySDR layer.
- Restructuring and fixes in time broadcast.

## [0.5.2] - 19/03/2026
### Added
- Optional broadcast of the current time to connected MSes: set timezone = "Europe/Amsterdam" (or the appropriate timezone) in the [cell_info] configuration section.

## [0.5.1] - 19/03/2026
### Changed
- Major LLC layer revamp: transmission logic is now substantially more standards-compliant, including message withholding until a previous message has been acknowledged by the MS, and retransmission of lost messages.


## [0.5.0] - 13/03/2026
### Added
- SDS support, including interconnection with Brew.
- feature_sds_enabled flag in the [brew] configuration section, to disable SDS forwarding to Brew.
- UL inactivity timeout to detect stuck or disappeared radios mid-TX.
- LLC TX retransmission support for reliable message delivery
### Changed
- Brew default port updated to 443 and default username updated to SSID.
- Bitbuffer improvements.

## [0.4.10] - 10/03/2026
### Changed
- Scheduler improvements.

## [0.4.9] - 27/02/2026
### Added
- Dynamic ISSI → Brew attachment support.
- Improved filtering to control which ISSIs are routed to Brew and which remain local.

### Changed
- Removed the `issi` field from the `[brew]` configuration section: leaving this field in place will prevent the application from starting.
  
## [0.4.8] - 26/02/2026
### Added
- Support for uplink second-slot stealing (low-level feature).
- Receipt-based transmission tracking for CMCE/MM/SNDCP:
  - Allows components to retain a receipt when sending a message.
  - Enables verification that the message was transmitted over the air.
  - Tracks whether the MS acknowledged reception.
- Self-throttled D-SETUP repeats when MAC is heavily loaded (leveraging the new receipt mechanism).

### Changed
- Major configuration system rewrite.
- Improvements and updates to the unit testing framework.

## [0.4.6] - 24/02/2026
### Added
- Dynamic group attachment and detachment over Brew (no more hard-coded group configuration required).
- Optional SSI range configuration in `[cell]` to force traffic to remain local.
- Optional SSI whitelist in `[brew]` to restrict which SSIs may be routed through Brew.

### Changed
- Internal refactors and cleanup.

### Notes
- ISSI registration remains static for now; dynamic ISSI handling is planned for a future release.

---

## [0.4.5] - 23/02/2026
### Added
- Dynamic Lone Site Trunking signaling when Brew is enabled.
  - Automatically signals connected radios if Tetrapack / network connectivity is lost.
  - Falls back to config-defined LST behavior when Brew is disabled.

---

## [0.4.4] - 22/02/2026
### Improved
- Stability improvements in call hang time handling.
- Refinements to the call state machine.
- General call reliability improvements (local and via Brew / TetraPack / Brandmeister).

### Notes
- Calls considered reasonably stable.
- Still alpha quality, but significant progress in overall stability.

---

## [0.4.3] - 21/02/2026
### Added
- Initial support for SSI ranges.
- Adaptive jitter buffer for Brew-originated calls.

### Changed
- Renamed `LocalCall*` to floor-control terminology.
- PHY layer now tolerates ±1 sample RX timestamp jitter.

### Fixed
- Network call hangtime noise.
- Timeslot leak caused by missing return after speaker-change handling in `rx_network_call_start`.

---

## [0.4.1] - 20/02/2026
### Added
- Voice-related improvements and Brew / TetraPack / Brandmeister integration enhancements.
- Community contributions:
  - Voice and Brew/TetraPack/Brandmeister improvements by Natanielius and @misadeks.
  - Fixes and tweaks by @russel053.

### Fixed
- Multiple critical bugs.

### Changed
- Configuration file updated.
  - Brew must now be enabled/configured according to the example config file.

### Planned
- Further voice improvements.
- SDS integration work based on existing contributions.

---

## [0.3.4] - 11/02/2026
### Added
- First draft of a networked TETRA entity with working tests.
- Brew integration with local and networked group calls.
- Working group calls with Brew audio forwarding.
- Centralized group call management in CMCE.
- Safeguards around SAP message creation for Brew.

### Changed
- Massive restructuring and refactor of the codebase.
- Standardized code formatting and linting conventions.
- Refactors in Brew configuration.
- Reduced logging verbosity.

### Improved
- Downlink fragmentation development.
- Channel allocation and voice call work (WIP).
- Improved bitbuffer sanity checking.

### Fixed
- Downlink fragmenter bug.

---

## [0.2.5] - 15/01/2026
### Added
- Major improvements to MAC fragmentation.
- Scheduler optimizations.
- Indicated PPM error as float in example config.
- Updated bug report templates.

### Changed
- Cleanup and consolidation of previous Motorola-related fixes.
- Significant restructuring and internal rewrites.

### Fixed
- Resolved Motorola periodic disconnect issue.

### Notes
- Development snapshot.
- Intermediate work during MAC fragmentation implementation.

---

## [0.2.4] - 07/01/2026
### Added
- Re-implemented AACH handling (testing branch).
- Support for full-slot uplink messages (contributed by @Freddruppel).
- Group attach support for Motorola radios.
- Enhanced logging:
  - Structured log output.
  - Text log to file.
  - Raw RF data logging with non-blocking writes in separate threads.

### Changed
- Significant rewrites for more robust and standardized protocol field parsing and serialization.

### Fixed
- Multiple Motorola interoperability issues.

### Known Issues
- Motorola radios may still disconnect periodically despite successful registration and group attachment.
  - Root cause investigation ongoing.