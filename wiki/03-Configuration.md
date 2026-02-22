

All aspects of tetra-bluestation are configured through a single toml file. A template is present in the **example_config** folder.

Make a working copy: 
```bash
cp example_config/config.toml config.toml
```

Open the config file with your favorite file editor: 
```bash
nano config.toml
```

**TL;DR:** for proper operation, you must change the following values from default: 
* `tx_freq` : match your desired Tx frequency
* `rx_freq` : match your desired Rx frequency
* Uncomment the relevant SDR platform you're using ([phy_io.soapysdr.iocfg_*]). Leave the default values for less headache. Comment out the ones you aren't using. 
* Replace `mcc` and `mnc` with your network's values (examples: TETRAPACK uses MCC 901, MNC 9999. Tmo.services uses MCC 901, MNC 9998).
* `main_carrier` : change value to match the Tx frequency downlink (see calculation procedure in the notes below). Edit `freq_offset` if needed.
* `duplex_spacing` : if you want to use a custom duplex spacing, change this value to 7, uncomment `custom_duplex_spacing` and set the offset there. 




The configuration is split in multiple sections:
* Global configuration
* [phy_io]: PHY Layer configuration
* [net_info]: Network Information
* [cell_info]: Cell Information


> ### Configuration Reminder
>
> - Configuration files are **TOML** and therefore **case-sensitive** (`"Bs"` ≠ `"bs"`).
> - **All frequencies are expressed in Hz**, never in MHz.
> - RF parameters in `[phy_io]` **must be consistent** with those derived from `[cell_info]`.
> - Only **one** SDR device configuration (`iocfg_*`) may be active at a time.
> - Commented lines are **not parsed** and have no effect.
> - Using incorrect **MCC/MNC values or frequencies may violate local regulations**.
> - Debug logging and RF sample capture generate **very large files** and additional system load.
> - When unsure, consult the relevant **ETSI TS 100 392** specifications before changing values.



# Global Configuration
| Parameter        | Default / Example | Possible Values                     | Notes |
|------------------|-------------------|--------------------------------------|-------|
| `config_version` | `"0.5"`(version dependent)           | String (version identifier)          | Must match the configuration format expected by the running BlueStation version. |
| `stack_mode`     | `"Bs"`            | String: `"Bs"`, `"Ms"`, `"Mon"`              | Base Station, Mobile Station, or Monitor mode (the latter two aren't available yet). `"Bs"` required for cell operation. |
| `debug_log`      | _disabled_        | String (File path)                            | Enables verbose logging; large files and additional system load. |

# [phy_io]: PHY Layer configuration

| Parameter | Default / Example | Possible Values | Notes |
|----------|-------------------|-----------------|-------|
| `backend` | `"SoapySdr"` | String: `"SoapySdr"` | RF backend abstraction layer. Other backends may be added in the future. |
| `dl_tx_file` | _disabled_ | String: File path | Dumps generated downlink baseband samples to file (debug only). |
| `ul_rx_file` | _disabled_ | String: File path | Dumps received uplink baseband samples to file (debug only). |


## [phy_io.soapysdr]: SoapySDR Core Settings

| Parameter | Default / Example | Possible Values | Notes |
|---------|-------------------|-----------------|-------|
| `tx_freq` | `438025000` | Integer (Hz) | Downlink transmit frequency. Must match `cell_info` parameters listed below. |
| `rx_freq` | `433025000` | Integer (Hz) | Uplink receive frequency. Must match duplex spacing and band rules. |
| `ppm_err` | `0.0` | Float | Frequency correction for SDR oscillator error. |

## [phy_io.soapysdr.iocfg_*]: Device-specific information
Uncomment the SDR you're using, comment out any others. Only **one** `iocfg_*` subsection should be enabled at a time.

The following table only lists generic settings, that will vary by device.
| Parameter | Default / Example | Possible Values | Notes |
|---------|---------|-----------------|-------|
| `rx_ant` | `"TX/RX"` | Device-specific strings | Selects RX antenna port. |
| `tx_ant` | `"TX/RX"` | Device-specific strings | Selects TX antenna port. |
| `rx_gain_*` | `18.0 – 50.0` | Float (dB) | RX gain stages depend on hardware. |
| `tx_gain_*` | `3.0 – 35.0` | Float (dB) | TX gain stages depend on hardware. |

# [net_info]: Network Information

| Parameter | Default / Example | Possible Values | Notes |
|----------|-------------------|-----------------|-------|
| `mcc` | `204` | Integer (3 digits) | Mobile Country Code. Use only assigned or permitted values. |
| `mnc` | `1337` | Integer (2–3 digits) | Mobile Network Code. Use only assigned or permitted values. |


# [cell_info]: Cell Information

| Parameter | Default / Example | Possible Values | Notes |
|----------|-------------------|-----------------|-------|
| `freq_band` | `4` | `1–5` (band-dependent) | `4` corresponds to the 400 MHz TETRA band. |
| `main_carrier` | `1521` | Integer | Carrier index; see note below for calculation. |
| `duplex_spacing` | `4` | Band-dependent integer | `4` = 5 MHz spacing in 400 MHz band. |
| `custom_duplex_spacing` | _disabled_ | Integer (Hz) | Non-standard duplex spacing; requires radio support. |
| `freq_offset` | `0` | `0`, `±6250`, `12500` | Frequency offset in Hz per ETSI specification. |
| `reverse_operation` | `false` | `true`, `false` | Controls UL/DL ordering. |
| `location_area` | `2` | Integer | Location Area Identifier (LAI). |
| `colour_code` | `1` | `0–3` | Distinguishes adjacent cells on same frequency. |

> **Main carrier (N) calculation**

> `main_carrier = (DL_frequency_Hz - base_frequency_Hz - freq_offset_Hz) / 25_000`

> Where (per ETSI TS 100 392-15):
> - `base_frequency_Hz` is the **reference/base frequency for the selected band**
> - `freq_offset_Hz` is one of: `-6250`, `0`, `+6250`, `+12500`

> **Example (freq_band = 4, base/reference = 400 MHz)**

> - `base_frequency_Hz = 400 000 000`
> - `DL_frequency_Hz   = 438 025 000`
> - `freq_offset_Hz    = 0`

> `(438 025 000 - 400 000 000 - 0) / 25 000 = 1521`
> So:
> `main_carrier = 1521`

Optional Cell Parameters (Broadcast & Mobility)

| Parameter | Default / Example | Possible Values | Notes |
|---------|---------|-----------------|-------|
| `neighbor_cell_broadcast` | `0` | Integer | Controls neighbor cell information broadcast. |
| `cell_load_ca` | `0` | Integer | Advertised cell load (channel allocation). |
| `late_entry_supported` | `false` | `true`, `false` | Allows joining ongoing group calls. |
| `subscriber_class` | `0xFFFF` | Bitmask | Defines allowed MS classes. |
| `registration` | `true` | `true`, `false` | Enables registration support. |
| `deregistration` | `true` | `true`, `false` | Enables deregistration support. |
| `priority_cell` | `false` | `true`, `false` | Marks cell as priority for reselection. |
| `migration` | `false` | `true`, `false` | Enables migration features (WIP). |
| `system_wide_services` | `true` | `true`, `false` | If `false`, mobiles may enter fallback mode. |
| `voice_service` | `true` | `true`, `false` | Voice service flag; may be required for MS compatibility. |
| `circuit_mode_data_service` | `false` | `true`, `false` | Circuit Mode Data support (not implemented). |
| `sndcp_service` | `false` | `true`, `false` | Packet data service (not implemented). |
| `aie_service` | `false` | `true`, `false` | Air Interface Encryption support flag. |
| `advanced_link` | `false` | `true`, `false` | Advanced link features (WIP). |
| `system_code` | `0` | `0–15` | Identifies TETRA system version. |
| `sharing_mode` | `0` | Integer | Main control channel sharing mode. |
| `ts_reserved_frames` | `0` | Integer | Reserved frames for timeslot allocation. |
| `u_plane_dtx` | `false` | `true`, `false` | Enables user-plane DTX. |
| `frame_18_ext` | `false` | `true`, `false` | Frame 18 extension support. |


# [brew]: Brew (TETRA Homebrew Protocol)


| Parameter | Default / Example | Possible Values | Notes |
|----------|--------------------|-----------------|-------|
| `host` | System-dependent | String: Hostname or IP address | Brew server address. |
| `port` | `3003` | Integer (TCP port) | Brew server port. |
| `tls` | `true` | `true`, `false` | TLS is required (`wss://` / `https://`). |
| `username` | _none_ | Integer | HTTP Digest auth username (required when enabled). For TPC: DMRID (plus optional SSID) |
| `password` | _none_ | String | HTTP Digest auth password (required when enabled). For TPC: hotspot password |
| `issi` | _none_ | Integer | ISSI used to register with the server. For TPC: DMRID |
| `reconnect_delay_secs` | `15` | Integer (seconds) | Delay before reconnect attempts after disconnect. |