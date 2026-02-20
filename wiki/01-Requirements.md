# Minimum Required Equipment

## Computer / Host System

A “good enough” computer is one that can reliably handle real-time DSP, scheduling, and I/O without excessive latency.

As a reference point:

- A Raspberry Pi 4 (2 GB) has been shown to run tetra-bluestation reliably in typical test setups. Raspberry Pi 3, Pi Zero and Pi Zero 2 have shown to be unstable in quite a few occurences.
- Any modern x86_64 system (laptop, desktop, or small server) with comparable or better performance should be sufficient.

Minimum practical requirements:

- CPU:  
  - Quad-core ARMv8 or x86_64 CPU  
  - Comparable or better than Raspberry Pi 4
- RAM:  
  - 2 GB minimum (4 GB recommended)
- Storage:  
  - SSD or fast SD card (slow storage can cause startup or logging issues)
- USB:  
  - Stable USB 2.0/3.0 connectivity for SDR hardware. 

Real-time scheduling and CPU frequency scaling may significantly affect performance. Systems with aggressive power saving features should be configured accordingly. Plan a Serial console or remote access (SSH) for headless operation.

Performance will also be affected if logging is enabled.

## Operating System

- Tested with recent stable Debian releases.
- On Raspberry Pi, most tests have been performed with [Raspberry Pi OS **Lite** - Debian 13 (Trixie)](https://www.raspberrypi.com/software/operating-systems/).
- Other Linux distributions may work but are not documented yet.

The documentation and examples assume a Debian-based system layout, package names, and tooling.

## SDR Hardware list

| Hardware         | Supported | Notes |
|------------------|-----------|-------|
| **LimeSDR / LimeSDR Mini**      | ✅        | Full-duplex operation with adequate timestamping support; commonly used for prototyping and testing. |
| **USRP**         | ✅        | Well-supported hardware family with robust timing and synchronization features. |
| **LibreSDR B205/B210**     | ⚠️       | Supported. Will require a [custom bitstream](https://github.com/lmesserStep/LibreSDRB210). RF performance isn't guaranteed. |
| **SxCeiver**     | ✅        | Supported via the [SoapySX driver](https://github.com/tejeez/sxxcvr). |
| **PlutoSDR / Pluto+ / LibreSDR ZynqSDR / OpenSourceSDRLab 7010/7020** | ❌  | Not supported due to lack of reliable timestamping. Might change in the future, [more details here.](https://www.quantulum.co.uk/blog/private-lte-with-plutoplus-sdr/) |
| **HackRF**       | ❌        | Half-duplex design and timing limitations make it unsuitable for base-station use. |
| **BladeRF**      | ⚠️        | Expected to be compatible from a hardware perspective, but not implemented yet. Proof-of-concept [here](https://github.com/sg217/tetra-bluestation/tree/feature/bladerf) |
| **uSDR**      | ⚠️        | Expected to be compatible from a hardware perspective, but not implemented yet.  |



## Recommended accessories

Having a way to check the transmitted RF signal is vital. A spectrum analyzer is a good thing, but an RTL-SDR will actually give you more information. 
Recommended software: 
- [SDR++](https://www.sdrpp.org/): general-purpose SDR receiver software
- [SDRpp-tetra-demodulator](https://github.com/cropinghigh/sdrpp-tetra-demodulator): SDR++ TETRA demod plugin
A similar SDR# setup is possible.


# Licensing and Regulatory Compliance

Operating a TETRA base station involves radio transmission, and therefore falls under national and international telecommunications regulations. Users are solely responsible for ensuring that their use of *tetra-bluestation* complies with all applicable laws and licensing requirements.

## General Responsibilities

- Ensure you are legally permitted to transmit on the selected frequencies.
- Use only power levels, bandwidths, and emission types allowed by your license.
- Comply with national spectrum regulations and ITU region allocations.
- Cease transmission immediately if harmful interference is observed or reported.

This software is provided for **research, experimentation, and educational use**. It does not grant any right to transmit on licensed or protected spectrum.

## Amateur (HAM) Radio Use

For licensed radio amateurs:

- Operate **only within amateur radio bands** permitted in your jurisdiction.
- Verify that the chosen frequency is not already in use by:
  - Repeaters
  - Beacons
  - Other digital or analog services
- Avoid continuous or unattended transmission unless explicitly allowed by local regulations.
- Ensure that your transmissions do not cause interference to other amateur services.
- Always verify local rules before transmitting.

## Experimental and Research Use

For security research and protocol experimentation:

- Prefer shielded environments, dummy loads, or attenuated setups.
- Use conducted RF connections or RF enclosures whenever possible.
- Avoid over-the-air transmission unless explicitly licensed or authorized.
- Be aware that decoding, impersonation, or simulation of real networks may be regulated or restricted.

## Commercial and Public Safety Spectrum

TETRA is widely used in commercial, government, and public safety networks.

- Never transmit on frequencies allocated to operational TETRA networks without explicit authorization.
- Unauthorized transmission on these bands may disrupt critical services and can result in severe legal consequences.

## Disclaimer

The authors and contributors of tetra-bluestation assume no responsibility for improper or unlawful use of this software. All transmissions are performed at the user’s own risk and responsibility.
