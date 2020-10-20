The APStreamline project aims to make streaming live video from companion computers as painless as possible. It was first released as the product of a GSoC project completed in 2018. Community feedback for APStreamline was very encouraging for APStreamline and that led me to continue development on it in 2019 as well to add support for different types of cameras. It begs the question - why does support for each camera need to be baked in to the code? The reason is that some cameras use different GStreamer elements, support different resolutions, and even have different capabilities when it comes to adjusting encoder settings, resolution, and bitrates. In case of the NVIDIA Jetson line of single-board computers, there are GStreamer pipelines built specifically for accessing the Jetson's ISP and hardware encoder for high quality video encoding. To make the most of each camera and companion computer, it is well worth the effort to add specific support for popular types of cameras.

In fact, the most requested feature for APStreamline has been to make it easier to add support for different types of cameras. However, it quickly became apparent that the codebase was too tightly coupled to do this. That inspired a rewrite of the code, and I am proud to announce that with the current branch of APStreamline, it is now much simpler to add support for your own camera!

## Cameras Supported

* Logitech C920
* Raspberry Pi Camera (Raspberry Pi only)
* e-Con AR0521 (requires the Developer Preview version of NVIDIA Jetpack 4.4)
* ZED2 Depth camera in V4L2 mode
* Any camera which support MJPG encoding (fallback when specific support is not detected)

## Devices Tested

* Raspberry Pi 3B
* Raspberry Pi Zero W
* NVIDIA Xavier NX
* x86_64 laptop
