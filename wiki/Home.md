# Home
```
░▀█▀░█▀▀░▀█▀░█▀▄░█▀█░░░░░█▀▄░█░░░█░█░█▀▀░█▀▀░▀█▀░█▀█░▀█▀░▀█▀░█▀█░█▀█
░░█░░█▀▀░░█░░█▀▄░█▀█░▄▄▄░█▀▄░█░░░█░█░█▀▀░▀▀█░░█░░█▀█░░█░░░█░░█░█░█░█
░░▀░░▀▀▀░░▀░░▀░▀░▀░▀░░░░░▀▀░░▀▀▀░▀▀▀░▀▀▀░▀▀▀░░▀░░▀░▀░░▀░░▀▀▀░▀▀▀░▀░▀
```

Welcome to the TETRA-BlueStation Wiki, the documentation platform for BlueStation: a FOSS TETRA stack aimed at providing an extensible basis for TETRA experimentation and research. 

The following Wiki is structured as follows: 
* **Introduction**: lists all working details, implementation choices, working and non-working features
* **Requirements**: summarizes the supported hardware, and legal considerations
* **Dependencies and building**: goes through the dependencies install process as well as install procedure
* **Configuration**: explains how to properly configure tetra-bluestation for execution
* **Running TETRA-BlueStation**: lists the steps to start tetra-bluestation and how to improve execution
* **Contributions, forks and issues**: explains the contribution policies, lists known forks and how to submit an issue
* **Miscellaneous**: few aspects not tackled in the other chapters

For quick answers to common questions, see the [Frequently Asked Questions](./30-Frequently-Asked-Questions.md) page.

Join the community Telegram group: [https://t.me/+WtWvrlb43NVkNjE0]

> ⚠️ **Experimental Software Warning**
>
> *tetra-bluestation* is **highly experimental** software.
>
> - Interfaces, features, and internal behavior may change at any time.
> - Things **will break** occasionally, sometimes in creative ways.
> - New features and bug fixes are added frequently.
> - Documentation and wiki content may **lag behind the current code** and should not be assumed to be fully up to date.
>
> This project is intended for experimentation, research, and learning.  
> Do not rely on it for production use, safety-critical systems, or operational networks.
>
> If you encounter unexpected behavior, consult the source code and recent commits first, and feel free to report issues or contribute improvements.

## Getting started

This section is intended for readers who want to get a basic setup running quickly.  
It assumes familiarity with SDRs, Linux systems, and basic RF concepts.

If something does not work as expected, take the time to read the linked sections in detail.

### 1. Check hardware requirements

Before doing anything else, make sure your setup meets the basic requirements:

- A supported SDR with adequate TX/RX capability  
- A computer capable of handling an SDR workload 

See the relevant hardware and SDR requirements section for details and known-good configurations.

---

### 2. Build the software

Install dependencies (general and those linked to the SDR hardware you're using), clone the repository and build a release binary. 

Follow the build instructions in the build and installation section to ensure all dependencies are correctly installed.

---

### 3. Edit the configuration file

Start from the provided example configuration and adapt it to your setup:

- Select the correct SDR hardware and device-specific settings  
- Configure frequencies, band, and cell parameters consistently  
- Verify network identifiers (MCC, MNC, etc.)  

The configuration file reference explains each field in detail and should be consulted carefully.

---

### 4. Run the base station

Once built and configured, start the base station stack by pointing the binary at your TOML file.

For improved scheduling stability, consider running the process with real-time priority, as described in the runtime section.
