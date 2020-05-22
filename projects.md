---
title: Projects
slug: projects
date_published: 2019-05-10T13:48:23.000Z
date_updated: 2019-12-27T00:37:53.000Z
layout: page
---

This is a list of the projects which I'm proud of making:

### 2020

### F1/10 Autonomous Racing

This was a group project for the ESE615 F1/10 Autonomous Racing course. Using ROS, we had to develop an agent capable of overtaking another agent without collisions. The agents in question are 1:10 scale models of RC cars, equipped with a 2D LIDAR and an NVidia Jetson TX2. This was a deceptively simple task and we ended up using Model Predictive Control with some interesting hacks in our final submission. We also tried using a heavily modified version of RRT* and Gaussian processes for opponent project. This was a fun robotics project, and my only regret is that we couldn't run it on the awesome F1/10 cars. That said, it works pretty well in simulation!

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/lucascheuer/F110-Final/)
<i class="far fa-file-pdf"></i>[  Presentation  ](/static/autonomous-racing-presentation.pdf)
<i class="far fa-file-pdf"></i>[  Report  ](/static/autonomous-racing.pdf)

### RGB-D Object Tracker

This project was our final project for the ESE650 Learning in Robotics course. We wanted to play with RGB-D cameras and apply them to tracking arbitrary objects. As RGB-D cameras can estimate depth, they have the potential to estimate the 6DOF relative position of an object much better than a monocular RGB camera can. However, RGB-D cameras suffer from having a limited range, usually limited to 5m or else. In this project, we tried applying a particle filter to estimate the 3DOF relative position and velocity for the three main cases which can occur when tracking an object 1) object is in range of the depth sensor and is visible in the RGB camera 2) object is out of range of the depth sensor but is still visible in the RGB camera and 3) object is no longer visible. We experimented using a dataset from the [Princeton RGB-D tracking benchmark](http://tracking.cs.princeton.edu/dataset.html). It would be interesting to see how well this approach works in realtime with actual hardware.

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/shortstheory/depthtracking650/)
<i class="far fa-file-pdf"></i>[  Report  ](/static/depth-tracking.pdf)

### 2019

### DtnLink

[![shortstheory/underwater-dtn - GitHub](https://gh-card.dev/repos/shortstheory/underwater-dtn.svg)](https://github.com/shortstheory/underwater-dtn)

This was for my undergraduate thesis at the Acoustic Research Laboratory, located in NUS, Singapore. I was mentored by Prof. Mandar Chitre. I've had an amazing time working at the ARL. The UnetStack simulator I've used in my project is very unique and it has changed the way I think about computer networks. `DtnLink` may not be the most complicated project I've worked on, but it's one of the most polished and well documented projects I've done. Thanks to working with Prof. Mandar, my development process was much more streamlined than what it would've been had I developed it by myself.

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/shortstheory/underwater-dtn)
<i class="far fa-file-pdf"></i>[  Presentation  ](/static/ug-thesis-presentation.pdf)
<i class="far fa-file-pdf"></i>[  Report  ](/static/ug-thesis.pdf)

### 2018

### APStreamline

[![shortstheory/adaptive-streaming - GitHub](https://gh-card.dev/repos/shortstheory/adaptive-streaming.svg)](https://github.com/shortstheory/adaptive-streaming)

This was my second GSoC project. I worked with the [ArduPilot](http://ardupilot.org/) organisation. This was by far the biggest project I had ever done. I had to cover a lot of bases - learning the GStreamer API for constructing a video streaming pipeline, RTSP/RTCP/RTP, the theory of H.264 encoding, the [image capturing pipeline](https://picamera.readthedocs.io/en/release-1.13/fov.html) with CSI cameras on the RPi, local sockets to communicate with a web server, and even a bit of JavaScript to create a GUI to tie the whole thing together. A lot of the development for this project was on the Raspberry Pi. There were many tiny frustrations throughout this project (as there will be with anything involving hardware), but the end product is something I'm very proud of making. It feels great to have made something which some people use on a daily basis! Also, big thanks to ArduPilot for sponsoring the hardware :-)

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/shortstheory/adaptive-streaming)
<i class="fas fa-blog"></i>[  Post 1  ](/2018/06/05/gsoc-2018-new-beginnings/)
<i class="fas fa-blog"></i>[  Post 2  ](/2018/07/22/gsoc-2018-batteries-included/)
<i class="fas fa-blog"></i>[  Post 3  ](/2018/08/17/introducing-apstreamline/)

### 2017

### A 3D Children's Park

[![shortstheory/3d-graphics-scene - GitHub](https://gh-card.dev/repos/shortstheory/3d-graphics-scene.svg)](https://github.com/shortstheory/3d-graphics-scene)

This was one of my favourite course projects ever because of how visual it is. With the help of some very talented friends, I made a 3D scene for rendering a children's park using OpenGL 3.3. It was pretty interesting to learn how the OpenGL hardware pipeline worked. I also liked playing around different values of the lighting effects to see how it would change the appearance of the scene.

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/shortstheory/3d-graphics-scene)

### Quadcopters

The only non-software project in this list, and in some ways not exactly an original project at all? I first got my hands wet with quadcopters through the Automation & Robotics Club in college. It was the perfect intersection of my interests in flying vehicles, electronics, hardware, and software. While we didn't end up making anything significant that time, I bought the parts for making my own CC3D-powered quadcopter that very summer. I had a blast working my way through the assembly and tuning of my machine. Later on, I studied the dynamics in more detail and became fairly confident that robotics was definitely something I would want to work on in the future.

The next year, I made significant upgrades to my quadcopter, giving it a new brain in the form of the Pixhawk. I also added GPS, live radio telemetry, Wi-Fi router, and a Raspberry Pi with a CSI camera for video streaming. Unfortunately, the Indian government has made it much harder to fly unregulated "micro" aerial vehicles so I'm not sure what the future of this hobby looks like.

<i class="fas fa-blog"></i>[  Post 1  ](/2017/05/26/quadcopters-the-beginning/)
<i class="fas fa-blog"></i>[  Post 2  ](/2017/06/13/quadcopters-a-hitchhikers-guide-to-the-sky/)
<i class="fas fa-blog"></i>[  Post 3  ](/2019/06/27/using-the-logitech-f310-with-the-dji-tello/)

### 2016

### kio-stash

[![KDE/kio-stash - GitHub](https://gh-card.dev/repos/KDE/kio-stash.svg)](https://github.com/KDE/kio-stash)

Probably the project which started it all, this was a GSoC project for [KDE](https://kde.org/) which was the realisation of an idea I had in my mind for nearly a year. I created a new `kioslave` for developing my idea of making a virtual directory which could copy files from multiple directories before copying them to the destination. This project was an amazing learning experience before it even began - from the early days of lurking on the KDE mailing lists to finally having a coherent idea of what I was going to write in my proposal. I took help from developers for submitting my first patches and reviewing my idea and I'm profoundly thankful to every single KDE developer I interacted with for taking the time to make me comfortable with the intricacies of KDE's development stack. I learned about version control, unit testing, and writing **good** C++/Qt code. This project also had huge IRL benefits as I got to attend two of KDE's biggest international conferences - QtCon 2016, Berlin and [Akademy 2017](https://arnavdhamija.com/2017/08/17/akademy-2017/), Almeria as well! I've also made some great friends from the KDE community, some of whom I'm still in touch with today.

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/KDE/kio-stash)

### 2014

### Rigel

[![shortstheory/rigel - GitHub](https://gh-card.dev/repos/shortstheory/rigel.svg)](https://github.com/shortstheory/rigel)

I made a Flappy Bird-like computer game for my Grade XII CBSE project. Back at the time, I knew nothing idea about version control, proper documentation, or how to write safe and properly structured C++ code.

Nevertheless, it was a huge personal achievement for me to make something with two-dimensional *graphics* at that point in time. I used the SFML library for all the graphics code that would've otherwise been OpenGL and did all the game logic in C++11 (as opposed to the 1990 flavour of Turbo C++ we did at school). This project gave me a lot of confidence in my abilities and the things I learned from this helped me a lot for my first GSoC project in my first year of college.

<i class="fa fa-code" aria-hidden="true"></i>[  Code  ](https://github.com/shortstheory/rigel)



*GitHub cards generated using [https://gh-card.dev/](https://gh-card.dev/)*

<!-- *Icons made by [Freepik](https://www.flaticon.com/authors/freepik) from [Flaticon.com](www.flaticon.com)* -->