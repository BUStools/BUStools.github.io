---
layout: page
title: "Source"
description: ""
group: navigation
---
{% include JB/setup %}

The __bustools__ GitHub repository is https://github.com/BUStools/bustools. Currently, __bustools__ can be built on Linux, Mac, Rock64 (ARM) and Windows. If building on Mac, we suggest using a package manager such as [Homebrew](http://brew.sh) to download dependencies. Homebrew is easily installed by copying and pasting the command below at a terminal prompt:

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Other dependencies are included, or can be installed using package managers on the system. Instructions for building bustools from source without a package manager are identical to kallisto, and can be found [here](http://pachterlab.github.io/kallisto/local_build.html).

#### Requirements:

- A 64-bit operating system
- g++ version >= 4.8
- __CMake__ version >= 2.8.12
    - Mac: `brew install cmake`
    - Ubuntu: `sudo apt-get install cmake`
    - CentOS: `sudo yum install cmake`


## Installation from source

Download bustools with

`git clone https://github.com/BUStools/bustools.git`

Navigate to the bustools directory

`cd bustools`

Make a build directory and move there:

`mkdir build`

`cd build`

Run cmake:

`cmake ..`

Build the code:

`make`

The bustools executable is now located in build/src. To install bustools into the cmake install prefix path type:

`make install`



