The following instructions assume a Debian-based system. All commands are written and tested with Debian in mind.

The setup follows this process:
1. **General dependencies**  
   Install the Rust toolchain and common system libraries required to build and run the project.

2. **Hardware-specific dependencies**  
   Install additional drivers or libraries depending on the selected SDR hardware (for example, SxCeiver-specific requirements).

3. **Clone and build tetra-bluestation**  
   Fetch the source code from the Git repository and compile the software.

Each step is described in detail in the sections below.


## General Dependencies
First, update your package list and install the required build and runtime dependencies:

```bash
sudo apt update
sudo apt install -y --no-install-recommends \
  git make g++ cmake \
  libsoapysdr-dev \
  soapysdr-tools \
  libasound2-dev \
  clang llvm-dev libclang-dev \
  libgpiod-dev \
```

Then, install Rust. Rustup is the easiest way to get the most up-to-date toolchain: 
```bash
curl https://sh.rustup.rs -sSf | sh
```
## Hardware-specific dependencies
### SxCeiver-related dependencies
_Skip this section if you're not using the SxCeiver platform._

You might need to downgrade libgpiod. Dedicated guide coming soon.

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
Check that the device is detected:
```bash
SoapySDRUtil --find
```


### LimeSDR-related dependencies
_Skip this section if you're not using the LimeSDR platform._

Install LimeSuite and related tools:
```bash
sudo apt update
sudo apt install -y limesuite liblimesuite-dev limesuite-udev
```
Ensure udev rules are installed and reload them if necessary:
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
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
sudo apt install -y uhd-host libuhd-dev
```
You will also need to download firmware images, as they're loaded on the SDR platform at each reset:
```bash
sudo uhd_images_downloader
```
Check that your USRP device is detected correctly:
```bash
uhd_find_devices
SoapySDRUtil --find
```
You should see your USRP model listed on both commands. If not, verify USB/Ethernet connectivity and power.


## Clone and build tetra-bluestation
_If you want to try another repository with WIP features, replace the official repo URL with the one you want to test._

Clone the repository:
```bash
git clone https://github.com/MidnightBlueLabs/tetra-bluestation
```
If you want to use another branch, replace `master`. 

```bash
git checkout master
```

Build tetra-bluestation:
```bash
. "$HOME/.cargo/env"
cargo build --release
```

## Update tetra-bluestation
Pull latest changes from the repository. Don't forget to cd into the main folder first, and replace `master` if you're using another branch.
```bash
git pull origin master
```

Re-build tetra-bluestation (all pre-existing crates shouldn't need updating)
```bash
cargo build --release
```