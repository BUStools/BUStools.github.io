---
layout: page
title: "Source"
description: ""
group: navigation
---
{% include JB/setup %}

The __bustools__ GitHub repository is https://github.com/BUStools/bustools. Currently, __bustools__ can be built on Linux and Mac. If building on Mac, we suggest using a package manager such as [Homebrew](http://brew.sh) to download dependencies. Homebrew is easily installed by copying and pasting the command below at a terminal prompt:

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Other dependencies are included, or can be installed using package managers on the system. Instructions for building bustools from source without a package manager are identical to kallisto, and can be found [here](http://pachterlab.github.io/kallisto/local_build.html).

#### Requirements:

- A 64-bit operating system
- g++ version >= 4.8
- __CMake__ version >= 2.8.12
    - Mac: `brew install cmake`
    - Ubuntu: `sudo apt-get install cmake`
    - CentOS: `sudo yum install cmake`
- __zlib__ (should be installed on OSX >= 10.9)
    - Mac: Should be installed by default
    - Ubuntu: `sudo apt-get install zlib1g-dev`
    - CentOS: `sudo yum install zlib-devel`
- __autoconf__ 
    - Mac: `brew install autoconf`
    - Ubuntu: `sudo apt-get install autoconf`
    - CentOS: `sudo yum install autoconf`
- __HDF5__ C library version >= 1.8.12
    - Mac: `brew install hdf5`
    - Ubuntu: `sudo apt-get install libhdf5-dev`
    - CentOS: `sudo yum install hdf5-devel`


## Compiling from source

`git clone https://github.com/BUStools/bustools.git` # Download bustools 

`cd bustools` # Navigate to the bustools directory

`mkdir build` # Make a build directory and move there:
`cd build`
`cmake ..` # Run cmake
`make` # Build the code

The bustools executable is now located in `build/src`. To install bustools into the cmake install prefix path type:
`make install`


