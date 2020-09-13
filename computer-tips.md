---
title: // Computer Tips
slug: computer-tips
date_published: 2019-12-26T22:54:17.000Z
date_updated: 2020-02-06T00:38:31.000Z
layout: page
---

I frequently find myself needing to lookup how to do simple things with Linux and programming all the time. This page serves as a continuously updated repository of useful commands which I need to use frequently:

## General

### Grab all files of a given type from a webpage

    wget -r -np -nd -l 0 -A.pdf http://security.cs.rpi.edu/courses/binexp-spring2015/lectures/


### Edit all images in-place

    # wxh+left+top
    # Cropping
    mogrify -crop 640x330+0+0 *.jpg
    # Resizing
    mogrify -resize 640x330+0+0 *.jpg

### Photo Metadata

    # Read metadata
    exiftool /tmp/my_photo.jpg
    # Delete metadata
    exiftool -overwrite_original_in_place -all= /tmp/my_photo.jpg

### Create a softlink

    ln -s <src> <dest>

### Change Git Remote URL for Forks

    git remote set-url origin https://github.com/USERNAME/REPOSITORY.git

### Get all Git submodules

    git submodule update --init --recursive

### Change Video Bitrate

    # To get a 4mbps video
    ffmpeg -i input.mp4 -b 4000k output.mp4


### Make LaTeX Machine Readable

    % Add these two lines in your preamble
    \input{glyphtounicode}
    \pdfgentounicode=1


### Get Windows BIOS Keys

    sudo hexdump -s 56 -e '"MSDM key: " /29 "%s\n"' /sys/firmware/acpi/tables/MSDM


### Watch a Directory

    # Updates every 0.1s
    watch -n 0.1 ls -lt .


### Fix Ubuntu and Windows Time Sync issues

    timedatectl set-local-rtc 1 --adjust-system-clock

### SSHFS Example

    sshfs USER@IP:/$PATH $LOCAL_PATH

### Copy SSH keys to server

    ssh-copy-id id@server

### Screen Tips

    # Create a named screen
    screen -S myscreen
    # Reconnect
    screen -r myscreen
    # List screens
    screen -ls
    # Detach
    Ctrl+A+D

### Jekyll Setup

    sudo apt-get install ruby-full build-essential zlib1g-dev
    gem install jekyll bundler
    bundle install
    bundle exec jekyll serve --watch --trace

### Boot Repair Setup

    sudo add-apt-repository ppa:yannubuntu/boot-repair
    sudo apt-get update
    sudo apt-get install -y boot-repair && boot-repair

## Python and Jupyter

### Show all Pandas columns

    pd.set_option('display.max_colwidth', -1)


### Increase Width of Jupyter Notebook

    # Add this to the first cell of your notebook
    from IPython.core.display import display, HTML
    display(HTML("<style>.container { width:90% !important; }</style>"))


### Install Dependencies

    pip install -r requirements.txt


### Display a full numpy array

    np.set_printoptions(threshold=np.inf)


### Automatically Reload Modules for Jupyter

    # add to beginning of notebook
    %load_ext autoreload
    %autoreload 2

### Creating a new Jupyter Notebook

    import numpy as np
    from matplotlib import pyplot as plt
    from scipy import interpolate
    import matplotlib.image as mpimg
    import os
    import PIL.Image as Image
    %load_ext autoreload
    %autoreload 2

### Main Function
    # wtf is this so hard to remember?
    def main(): 
        print("hey there") 
    if __name__=="__main__": 
        main() 

## Raspberry Pi

### SSH

`touch ssh` to enable ssh in `/boot`

### WiFi

In `/boot/wpa_supplicant.conf`

    country=US
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
    ssid="NAME"
    scan_ssid=1
    psk="PASSWORD"
    key_mgmt=WPA-PSK
    }

    network={
       ssid="TELLO-D8A5A9"
       key_mgmt=NONE
    }


### GPIO Cheatsheet

    pinout


### NMap to find devices

    sudo nmap -sn 192.168.1.0/24


## ROS

### Build with Debug Flags

    catkin_make -DCMAKE_BUILD_TYPE=Debug

## Miscellaneous

### Get GTK Thumbnails back

    sudo apt install gnome-icon-theme

### Video Thumbnail Previews in Dolphin

    sudo apt install ffmpegthumbs mplayerthumbs kffmpegthumbnailer kio-extras
    # and then enable Previews by going to Dolphin Preferences


### Setup a Samba Share in Dolphin

    sudo apt install kdenetwork-filesharing libsmbclient samba smbclient
    # right click to Share folder after this


### Change to libinput for touchpad

Uninstall synaptics.

## Fresh Install

    zoom code slack
    geany mendeleydesktop