## General

### What is tetra-bluestation?

A free and open-source TETRA base station software stack for experimentation and research. See the [[Introduction|00-Introduction]] for a full description, and the *What works / Not functional* sections there for the current feature status.


### Is this ready for production or operational use?
No. See the warning on the [[Home|Home]] page.

### May I connect it to TETRAPack - tmo.services - xxx?

At the time of writing this FAQ, only a specific fork supports connection to TETRAPack. Keep in mind you must be holder of a valid amateur radio license to join that network.

## SDR Hardware Compatibility

### Is my SDR compatible?

See the hardware compatibility table on the [[Requirements|01-Requirements]] page. The short rule: only SDRs with reliable **full-duplex hardware timestamping** are supported.


### Is the PlutoSDR (or Pluto+, LibreSDR ZynqSDR, OpenSourceSDRLab 7010/7020) compatible?

No, not at this time. These devices lack reliable hardware timestamping, which is a hard requirement for base station operation. This may change in the future — see [[Requirements|01-Requirements]] for details and a reference link.


### Is the HackRF compatible?

No. The HackRF is half-duplex — it cannot transmit and receive at the same time, which a TETRA base station must do. This is a fundamental hardware limitation.


## Host System

### Can I run this on a Raspberry Pi?

Yes — a Raspberry Pi 4 (2 GB) has been confirmed to work. See [[Requirements|01-Requirements]] for minimum specs and configuration tips (real-time scheduling, SD card speed, etc.). 
You might be able to get away with lower-spec hardware, but as this software grows, expect the requirements to increase as well. 

BlueStation officialy aims to support Pi5, so if in the future the compute loads turns out to be too heavy, Pi4 support might have to be dropped.

### Is there a ready-made SD card image or binary I can download?

No. The software is evolving too fast for packaged images to stay useful. You need to build from source on a Debian-based system — see [[Dependencies and Building|02-Dependencies-and-Building]].


## Software

### Should I use the main branch or a fork?

Use the main upstream repository for the most stable experience. Forks exist for experimental features (e.g. voice calls via Tetrapack/Brew). See [[Contributions, Forks and Issues|10-Contributions,-forks-and-Issues]] for the full list of known forks and what they offer.

### Does it support voice calls?

Not in the main branch, yet, expect this to change very soon. 

Experimental voice support exists in the [`tetrapack-calls`](https://github.com/misadeks/tetra-bluestation/tree/tetrapack-calls) fork — see [[Contributions, Forks and Issues|10-Contributions,-forks-and-Issues]].

## Configuration

### Where do I start?

Copy the example config and follow the [[Configuration|03-Configuration]] page. That page lists the minimum fields you must change and explains every parameter.

## Legal

### Is it legal to run this?

Transmitting requires authorization. You are solely responsible for compliance. See the *Licensing and Regulatory Compliance* section on the [[Requirements|01-Requirements]] page.

## Getting Help

### I have a question or need help getting started

Join the community Telegram group: https://t.me/+ZhT3bvs_LHUwNmQ0

It's the best place for quick questions, setup help, and general discussion.

### I think I've found a bug

1. Check the [open issues](https://github.com/MidnightBlueLabs/tetra-bluestation/issues) to see if it's already reported.
2. If not, open a new issue using the provided issue template in the repository and fill it out as completely as possible. Issues without enough detail are hard to act on and may be deferred.

See [[Contributions, Forks and Issues|10-Contributions,-forks-and-Issues]] for more on the contribution and issue process.
