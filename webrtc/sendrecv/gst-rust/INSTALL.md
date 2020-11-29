# Setup on PC
Download, install and start rpi-imager
```bash
wget -q https://downloads.raspberrypi.org/imager/imager_1.4_amd64.deb
sudo apt install -y ./imager_1.4_amd64.deb
./rpi-imager
```

Follow the instructions and select: Ubuntu > Ubuntu Desktop 20.10 (64-bit)
Install Ubnutu on the SD-card and boot the Pi

# Setup on Pi & PC
## Install packages
Update all the things
```bash
sudo apt update && sudo apt upgrade -y
```

Install all the things
```bash
sudo apt install -y \
  vim \
  git \
  libssl-dev \
  gstreamer1.0-tools gstreamer1.0-nice gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-plugins-good libgstreamer1.0-dev git libglib2.0-dev libgstreamer-plugins-bad1.0-dev libsoup2.4-dev libjson-glib-dev \
  curl \
  build-essential \
  openssh-server \
  ninja-build \
  python3-pip
```

## Install meson
Install meson
```bash
pip3 install --user meson
echo -e 'PATH="$PATH:$HOME/.local/bin\n' >> ~/.bashrc
source ~/.bashrc
```

## Install rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

echo -e 'source $HOME/.cargo/env\n' >> ~/.bashrc
source ~/.bashrc
```

## Clone and build libnice

```bash
git clone https://gitlab.freedesktop.org/libnice/libnice.git

meson --prefix "$(pwd)/install" builddir
ninja -C builddir
ninja -C builddir install
cd install/lib/aarch64-linux-gnu/
echo "$(pwd)" > ~/.libnice-library-path.txt
```

## Clone the repo and setup audio
Clone the repo and check out branch "test1"
```bash
git clone https://github.com/totalorder/gst-examples.git
cd gst-examples/
git checkout test1
```

Figure out how to access your microphone and speakers from gstreamer
```bash
gst-device-monitor-1.0
```

Put the command for the microphone in "input.txt"
```bash
# NOTE: This is just an example
echo "pulsesrc device=alsa_input.usb-046d_09a1_2560E220-02.mono-fallback" > input.txt
```

Put the command for the headphones/speakers in "output.txt"
```bash
# NOTE: This is just an example
echo "autoaudiosink" > output.txt
```

(pc only) Add the IP of the pi to pi-ip.txt
```bash
# NOTE: This is just an example
echo "192.168.2.203" > pi-pi.txt
``` 

(pc only) Add yourself to authorized keys on the pi
```bash
scp ~/.ssh/id_rsa.pub "$(cat pi-ip.txt):.ssh/authorized_keys"
```
 
(pc only) Start the "server"
```bash
# NOTE: There is a race-condition if the compile takes >3 second. 
# Just run ./run-remote again and it will be fast enough the second time

./run-remote
```

(pi only) Start the "client"
```bash
./run-local
```
