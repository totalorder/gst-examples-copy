# PC
Download, install and start rpi-imager
```bash
wget -q https://downloads.raspberrypi.org/imager/imager_1.4_amd64.deb
sudo apt install -y ./imager_1.4_amd64.deb
./rpi-imager
```

Follow the instructions and select: Ubuntu > Ubuntu Desktop 20.10 (64-bit)
Install Ubnutu on the SD-card and boot the Pi 

# Pi
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

Install rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

echo -e 'source $HOME/.cargo/env\n' >> ~/.bashrc
source ~/.bashrc
```
