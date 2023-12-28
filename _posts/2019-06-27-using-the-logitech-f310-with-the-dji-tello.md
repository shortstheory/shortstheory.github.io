---
title: Using the Logitech F310 gamepad with the DJI Tello
slug: using-the-logitech-f310-with-the-dji-tello
date_published: 2019-06-27T21:01:36.000Z
date_updated: 2019-06-27T22:06:58.000Z
tags: Quadcopters, Tech, Electronics
layout: post
---

I really like the [DJI/Ryze Tello](https://store.dji.com/product/tello).

It\'s a great nano drone and its feature list is hard to match at its price point. The camera/video quality is very good and for a drone which solely relies on its WiFi hotspot for control and video streaming to a smartphone, the range is pretty decent as well. It\'s impressively stable outdoors and the 10-15 minute battery life is adequate. As a trainer drone for new pilots, there\'s very little to complain about.

However, the main thing which drew to me to buy the Tello on impulse was its marketing with an emphasis on being "programmable". Unfortunately, I bought it without really looking into what this meant. I blindly assumed the Tello was hackable, but it turns out that the "programmable" features of the drone are limited to a few [Scratch commands](https://www.halfchrome.com/program-the-ryze-tello-with-scratch/) for writing basic scripts for movement. I wanted more control over the drone and initial impressions suggest that it has the technology to do a lot more than what the Scratch API exposes. The tiny drone has an infrared sensor for autonomous landing and uses optical flow for maintaining its position, which makes its sensor array unmatched for its price point.

The onboard Intel Myriad 2 SoC is advertised to have "14 Programmable SHAVE cores" but details on how to develop for this SoC are scarce. It\'s a shame, because I would\'ve really loved to see the autonomous capabilities of drone by implementing my own algorithms for it. The Tello uses this SoC for its Visual Processing Unit to keep it stable in flight, so I think it\'s reasonable to speculate that this chip could be used for a larger variety of image processing applications. Moreover, the drone is clearly tuned for stability and this makes it feel sluggish despite the drone having the hardware to be much more nimble.

But I digress. I spent some time playing with different approaches to reverse engineer the Tello, and the main Python projects I found were:

[Tello-Python](https://github.com/dji-sdk/Tello-Python): DJI\'s official Python API for interacting with the Tello. Promising, but I found their sample apps glitchy.

[TelloPy](https://github.com/hanyazou/TelloPy): This library is amazing. It appears to be a port of some of the reverse engineering work on the Tello conducted by the Gobot folks. The API supports everything offered in Tello-Python and much more. To top it off, I found the sample TelloPy apps far more reliable than their Tello-Python counterparts

I also suggest checking [tellopilots.com](http://tellopilots.com/) for more mods and discussion about hacking the Tello.

The nice thing about TelloPy is its controller support using `pygame`. However, this is limited to the PS4, PS3, XBOne, and Taranis controllers in gamepad mode. My Logitech F310 wired controller wasn\'t supported, so I figured out the button mappings in `pygame` and added it to the source of the `joystick_and_video.py` sample app. I\'ve created a pull request for the same too. To download my fork of TelloPy, clone it from GitHub here:

    
    git clone https://github.com/shortstheory/TelloPy/tree/F310 
    
    

After this, build and install `tellopy`. The one currently available on `pip` seems to be outdated .

    
    cd TelloPy 
    
    python setup.py bdist_wheel 
    
    pip install dist/tellopy-*.dev*.whl --upgrade 
    
    

Now switch on the Tello and connect your computer to its WiFi hotspot. The Tello can be controlled using the F310 joystick using:

    
    python tellopy.examples.joystick_and_video.py 
    
    

Press RB to takeoff and LB to land! The left stick is used for yaw and altitude control and the right stick handles roll and pitch.

## Raspberry Pi Controller

![Tello](/content/images/2019/06/Tello.png)

It would be nice if this could work without the need of a laptop. I tested this by following the same installation steps on a Pi Zero W running Raspbian Lite and the setup works well. I added the script to the Pi\'s `crontab` so it would start on every boot of the Pi:

    
    sudo crontab -e 
    
    

Followed by adding:

    
    @reboot python /home/pi/TelloPy/tellopy/examples/joystick_and_video.py 
    
    

To make the Pi Zero W automatically connect to the Tello when switched on, add the following lines to your `/etc/wpa_supplicant.conf` on the Pi:

    network={
       ssid="TELLO-XXXXXX"
       key_mgmt=NONE
    }
    

The Pi Zero is small enough to be tucked in behind the controller, even with a small Li-ion battery for power. However, I imagine the range of the Pi Zero W\'s Wi-Fi connection would only make it suitable for indoor flight. Fortunately, this can always be fixed by using a USB Wi-Fi dongle.

## Things to be done

While the TelloPy API supports the directional flip capability of the Tello, it doesn\'t map these controls to the joystick. I plan on dedicating the D-Pad for directional flips. I would also want the video streamed to the Tello to be saved directly to the Pi\'s internal storage. It would also be interesting to see if I could get the Pi to handle the image processing tasks and control the Tello remotely.
