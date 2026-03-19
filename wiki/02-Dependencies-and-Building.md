The following instructions assume a Debian-based system. All commands are written and tested with Debian in mind.

The setup follows this process:
1. **General dependencies**  
   Install the Rust toolchain and common system libraries required to build and run the project.

2. **Hardware-specific dependencies**  
   Install additional drivers or libraries depending on the selected SDR hardware (for example, SxCeiver-specific requirements).

3. **Clone and build tetra-bluestation**  
   Fetch the source code from the Git repository and compile the software.

Each step is described in detail in the sections below.

> ⚠️ **These commands have no brain, please use your own!**
>
> As a general rule, please take some time to examine (_and understand_) the commands and what they do before copy-pasting them. 
> Safest practice is not to copy-paste at all but re-typing them. [Here's a small example of why.](https://www.bleepingcomputer.com/news/security/dont-copy-paste-commands-from-webpages-you-can-get-hacked/)
> We do our best to document as precisely as possible, but honest mistakes can also happen. 
> 
> All of it could potentially ruin your setup (from which we cannot be held responsible).


## General Dependencies
First, update your package list and install the required build and runtime dependencies:

```bash
sudo apt update
sudo apt install -y --no-install-recommends \
  git make g++ cmake \
  libsoapysdr-dev \
  soapysdr-tools \
  libasound2-dev \
  clang llvm-dev libclang-dev
```

Then, install Rust. Rustup is the easiest way to get the most up-to-date toolchain: 
```bash
curl https://sh.rustup.rs -sSf | sh
```
## Hardware-specific dependencies
### SxCeiver-related dependencies
_Skip this section if you're not using the SxCeiver platform._

Clone and install SoapySX:
```bash
cd
git clone "https://github.com/tejeez/sxxcvr.git"
cd sxxcvr/SoapySX
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```
Check that the driver is available:
```bash
SoapySDRUtil --info
```
Check that the device is detected:
```bash
ls -l /proc/device-tree/hat
SoapySDRUtil --probe=driver=sx
```

Don't forget to cd back into your home before continuing.
```bash
cd
```


### LimeSDR-related dependencies
_Skip this section if you're not using the LimeSDR platform._

Install LimeSuite and related tools:
```bash
sudo apt update
sudo apt install -y limesuite liblimesuite-dev limesuite-udev soapysdr-module-lms7
```
Ensure udev rules are installed and reload them if necessary:
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```
Check that the driver is available:
```bash
SoapySDRUtil --info
```
Check that the device is detected:
```bash
LimeUtil --find
SoapySDRUtil --find
```
The device should appear in both outputs.


### USRP-related dependencies

_Skip this section if you're not using the USRP platform._

Install the UHD driver packages provided by Debian:
```bash
sudo apt update
sudo apt install -y uhd-host libuhd-dev soapysdr-module-uhd
```
You will also need to download firmware images, as they're loaded on the SDR platform at each reset:
```bash
sudo uhd_images_downloader
```
If you're using a USRP clone (LibreSDR B210 for instance), you might have to replace the original bitstream/firmware. [More information here.](https://github.com/lmesserStep/LibreSDRB210)

Check that the driver is available:
```bash
SoapySDRUtil --info
```
Check that your USRP device is detected correctly:
```bash
uhd_find_devices
SoapySDRUtil --find
```
You should see your USRP model listed on both commands. If not, verify USB/Ethernet connectivity and power.

### PlutoSDR-related dependencies
_Skip this section if you're not using the PlutoSDR platform (original Adalm Pluto or Pluto+). Other Pluto "clones" are not supported._

> **Note:** Do not use the low-quality USB cable shipped with the Pluto+. Use a good-quality cable to avoid connectivity issues.

#### Flash a timestamping-capable firmware

The stock firmware does not support the hardware timestamping required by TETRA-bluestation. You must flash a custom firmware first:

- For the original Adalm Pluto: https://github.com/pgreenland/plutosdr-fw/releases
- For the Pluto+: https://github.com/wahlm/plutosdr-fw-timestamp/releases

Follow the flashing instructions provided in the respective repository.

#### Install the timestamping-capable SoapySDR driver

The standard SoapyPlutoSDR driver does not support timestamping. Install the patched version:
```bash
git clone https://github.com/pgreenland/SoapyPlutoSDR.git -b sdr_gadget_timestamping
cd SoapyPlutoSDR
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
sudo ldconfig
``` 

Check that the driver is available:
```bash
SoapySDRUtil --info
```

The `plutosdr` module should appear in the list of available modules.

#### Check that the device is detected
```bash
SoapySDRUtil --find
```

The Pluto should appear in the output. If you are connecting a Pluto+ over Ethernet or USB, you can specify the URI explicitly to select the right interface, for example:
```bash
SoapySDRUtil --find="driver=plutosdr,uri=ip:pluto.local"
SoapySDRUtil --find="driver=plutosdr,uri=ip:192.168.42.42"
SoapySDRUtil --find="driver=plutosdr,uri=usb:1.3.5"
```

## Clone and build tetra-bluestation
You can either build the upstream repository (which contains the latest stable features), or build a fork.

Clone the repository (replace by the URL of the fork if you're not using the upstream one):
```bash
git clone https://github.com/MidnightBlueLabs/tetra-bluestation
cd tetra-bluestation
```

If you want to use another branch, replace `main`. 

```bash
git checkout main
```

Build tetra-bluestation:
```bash
. "$HOME/.cargo/env"
cargo build --release
```

## Update tetra-bluestation
Pull latest changes from the repository. Don't forget to cd into the main folder first.
```bash
git pull
```

Re-build tetra-bluestation (all pre-existing crates shouldn't need updating, the process should be much faster).
```bash
cargo build --release
```
