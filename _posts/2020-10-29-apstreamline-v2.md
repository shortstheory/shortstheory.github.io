---
title: APStreamline v2!
slug: apstreamline-v2
date_published: 2020-10-29T10:57:20.000Z
tags: Ardupilot, GSoC, Programming
layout: post
---

The [APStreamline project](https://github.com/shortstheory/APStreamline/) aims to make streaming live video from companion computers as painless as possible. It was first released as the product of my ArduPilot [GSoC project](https://discuss.ardupilot.org/t/introducing-apstreamline/31723) back in 2018. Community feedback for APStreamline was very encouraging and that led me to continue development in 2019. However, there was an issue which had been nagging at me from the outset of its v1 release - adding support for new cameras was tricky due to the confusing coupling of some classes in the codebase. In fact, the most requested feature I've received for APStreamline has been to make it easier to add support for different types of cameras.

![](/content/images/2020/IMG_0674.JPG)
*APStreamline has grown to support a wide variety of use cases for network adaptive video streaming*

The Linux V4L2 driver helps in making it easier to add support for different cameras. Several cameras have a standard interface for capturing video using GStreamer elements. So why does support for each camera need to be baked into the code? The reason is that some cameras use different GStreamer elements, some only support non-standard resolutions, and some even have onboard ASICs for H.264 compression. For example, in the NVIDIA Jetson line of single-board computers have GStreamer pipelines built specifically for accessing the Jetson's ISP and hardware encoder for high quality video encoding. To make the most of each camera and companion computer, it is well worth the effort to add specific support for popular camera models.

All this inspired a rewrite of the code, and I am proud to announce that with the release of APStreamline v2, it is now much simpler to add support for your own camera! Let's first take a look at what devices are already supported:

## Cameras Supported

* Logitech C920 webcam
* Raspberry Pi Camera (Raspberry Pi only)
* e-Con AR0521 (requires the Developer Preview version of NVIDIA Jetpack 4.4)
* ZED2 Depth camera (V4L2 mode)
* Any camera which support MJPG encoding (fallback when specific support is not detected. The MJPG stream from the camera is then encoded to H264 using a software encoder.)

## Devices Tested

* Raspberry Pi Zero W/2/3B
* NVIDIA Jetson Xavier NX
* x86 computer

## Adding Your Own Camera

To add specific your own camera, follow these steps!

1) Figure out the optimized GStreamer pipeline used for your camera. You can usually find this in the Linux documentation for your camera or online developer forums. There are differences for each camera but there is a generic template for most GStreamer pipelines: `SRC ! CAPSFILTER ! ENCODER ! SINK`. 
There might be more elements or additional capsfilters depending on each camera. In case you aren't sure what to do, feel free to drop a [GitHub issue](https://github.com/shortstheory/APStreamline/issues) to ask!
2) Create a configuration file for your camera. There are examples in the `config/` folder. The element names must match those in the examples for APStreamline to set references to the GStreamer elements correctly.
3) Subclass the `Camera` class and override the functions for which you want to add specific support for your camera. To give an example, the ways in which the H264 encoding bitrate is set are different for various cameras and encoder configurations. For instance, pipelines which use the `x264enc` encoder can change the bitrate by using `g_object_set`, whereas the Raspberry Pi camera GStreamer pipeline needs an IOCTL to change the bitrate.
There is a fair bit of trial and error to discover what the capabilities of a GStreamer pipeline are. Not all pipelines support the dynamic resolution adaptation of APStreamline, so this feature must be disabled if it causes the pipeline to crash in testing.
4) Create a new type for your camera and add it to the [`CameraType`]((https://github.com/shortstheory/APStreamline/blob/master/src/Camera/CameraType.h)) enum class.
5) Add the enum created in the previous step to the [`CameraFactory`](https://github.com/shortstheory/APStreamline/blob/master/src/Camera/CameraFactory.h) class.
6) Add a way of identifying your camera to [`RTSPStreamServer`](https://github.com/shortstheory/APStreamline/blob/8ba5b548321f46e39c8bc51c4e2e46f6847e5272/src/RTSPStreamServer/RTSPStreamServer.cpp#L77). A good way of adding a way to identify your camera is by using the Card type property from `v4l2-ctl --list-formats-ext --all --device=/dev/videoX`. In case your camera is not specifically detected, the fallback is to encode the MJPG stream from your camera using the H264 software encoder, `x264enc`.
7) File a Pull Request to get your camera added to the master branch!

## To Do
APStreamline could use your help! Some of the tasks which I want to complete are:

- Add support for the `tegra-video` driver back into the project. Currently this is only supported in [APStreamline v1.0](https://github.com/shortstheory/adaptive-streaming/releases/tag/v1.0), available from the Releases section of the repository
- Update documentation and add more detailed steps for adding a new camera
- Update the APWeb interface to list the actual available resolutions of the camera. Currently it just shows 320x240, 640x480, 1280x720 although the actual camera resolutions may be different
- Switch the APWeb and APStreamline IPC mechanism to using ZeroMQ or rpcgen
- Improve the installation flow. Currently the user needs to run APStreamline from the same directory as its config files for them to be loaded properly. Maybe the configuration files should be moved to `~/.config/?`
- Improve the JavaScript in the APWeb interface
- Document the code better!
