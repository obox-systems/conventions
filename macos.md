# MacOS testing

This document describes possible ways of testing your software on MacOS.

## Table of Contents

- [MacOS testing](#macos-testing)
  - [Table of Contents](#table-of-contents)
  - [GitHub Action](#github-action)
  - [Docker VM](#docker-vm)
    - [Pulling existing image](#pulling-existing-image)
    - [Manual build](#manual-build)
  - [Other virtualization solutions](#other-virtualization-solutions)
    - [VMWare](#vmware)
    - [VirtualBox](#virtualbox)


## GitHub Action 

This method is best for running compilation checks, unit/integration tests.
For interactive manual testing see [Docker VM](#docker-vm) and [Other virtualization solutions](#other-virtualization-solutions)

```yml
name: MacOS-Rust-testing

on:
  push:
  pull_request:

jobs:
  macos:
    runs-on: macos-latest
    steps:
      # Clone current branch to the action directory
      - uses: actions/checkout@v4

      # Rust utilities (rustc, cargo, etc.)
      - uses: sfackler/actions/rustup@master
      
      # Print rustc version for debugging
      - run: echo "version=$(rustc --version)" >> $GITHUB_OUTPUT
        id: rust-version

      # Fetch dependencies
      - name: Fetch
        run: cargo fetch

      # Build all binaries and libraries
      - name: Build
        run: cargo build --release --verbose

      # Run all tests
      - name: Run tests
        run: cargo test --release --verbose      
```

## Docker VM

It's possible to run a MacOS virtual machine on any hardware or OS with a docker image.

Repository for this project with additional help can be found [here](https://github.com/sickcodes/Docker-OSX).

This method works best on Linux, since for Windows you'd need to run in under WSL which creates nested virtualization.

### Pulling existing image

**! IMPORTANT ! Currently the main docker hub repository for this method is removed due to DMCA takedown.**
There are alternatives from other repositories hosting those images. This guide will show one of them, but in case that these no longer work
you can try find a working one in this [issue](https://github.com/sickcodes/Docker-OSX/issues/799);

```bash
# 40GB disk space required: 20GB original image 20GB your container.
docker pull dickhub/docker-osx:auto

# boot directly into a real OS X shell with a visual display [NOT HEADLESS]
docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e GENERATE_UNIQUE=true \
    dickhub/docker-osx:auto

# username is user
# password is alpine
```

### Manual build

This was taken from the issue on the 

```bash
# Clone the repository and build the docker image
git clone https://github.com/sickcodes/Docker-OSX.git
cd Docker-OSX
docker build -t sickcodes/docker-osx:ventura --build-arg SHORTNAME=ventura .

# Install required dependencies
# For distributions that use other packet manager search for the alternatives.
# Every step going forward should be taken in WSL if on Windows.
sudo apt-get update
sudo apt -y install bridge-utils cpu-checker libvirt-clients libvirt-daemon qemu qemu-kvm
sudo apt install x11-apps -y
sudo apt install -y tasksel
sudo tasksel install xubuntu-desktop
sudo apt install gtk2-engines

# (Optional) Windows only
# Export required vars for the root user.
sudo su
vi ~/.bashrc
# export DISPLAY=$(ip route | grep default | awk '{print $3}'):0.0
# export LIBGL_ALWAYS_INDIRECT=1
# /etc/init.d/dbus start &> /dev/null

# (Optional) Windows only
# Download https://sourceforge.net/projects/vcxsrv/ and installed
# Open XLaunch
# Chose "Multiple Windows"
# First screen. Set window number as 0
# Next screen. Select "Start no client"
# Next screen. Ensure all options are checked including "disable access control". Additional parameters: -ac -listen tcp
# Finish. Allow network access / firewall if prompted.
# Exit wsl
# In non-wsl terminal:
wsl --update
wsl --shutdown
wsl

# Run the container
docker run -it --device /dev/kvm -p 50922:10022 -v /tmp/.X11-unix:/tmp/.X11-unix -e "DISPLAY=${DISPLAY}" sickcodes/docker-osx:ventura
```

## Other virtualization solutions

### VMWare

This method is a bit tricky and will require you to find a working, pre-installed, MacOS virtual disk image.

Short guide for this on VMWare can be found [here](https://github.com/dortania/macOS-VMware-Guide/blob/master/making-the-virtual-machine.md).

### VirtualBox

This method requires you to install MacOS yourself. If you already own a device with MacOS installed, or can access it
you can create an ISO installation file on that machine.

Otherwise those images are available on multiple platforms, especially [The Internet Archive](https://archive.org/search?query=MacOS+iso).

A short guide that does over the steps can be found [here](https://www.xda-developers.com/how-install-macos-virtualbox/).
