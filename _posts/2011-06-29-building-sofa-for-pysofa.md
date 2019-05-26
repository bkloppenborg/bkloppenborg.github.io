---
title: "Building the Standards of Fundamental Astronomy (SOFA) library for pysofa"
date: 2011-06-29T00:34:06-00:00
draft: false
author: "Brian Kloppenborg"
tags : ["SOFA"]
categories : ["astronomy"]
---
{% include JB/setup %}

Standards of Fundamental Astronomy (SOFA) is a Fortran/C library from the IAU
that provides a variety of useful functions for astronomy that deal with time
and astrometric calculations. Unfortunately, the library ships with a very ugly
license that prohibits its modification and/or redistribution. As such, it isn't
widely used, despite being packed with very useful functions that provide:

* Calendars
* Time Scales
* Earth rotation and sidereal time
* Ephemerides (medium precision)
* Geocentric/geodetic transformations
* Precession, nutation, polar motion
* Star space motion
* Star catalogue conversion

If you wish to use SOFA from Python, you are in luck: there is a 
[Python interface on pypi](http://pypi.python.org/pypi/pysofa) 
however, you will need to build SOFA as a shared library and install it on your
system in order to get it to work.

## System Requirements

In order to proceed you will need several things:

* Python (and potentially the python development packages)
* A C compiler and linker
* CMake

On Ubuntu it's quite easy to install all of these, just do

    sudo apt-get install python python-all-dev build-essential cmake

## Getting SOFA

Download and extract SOFA from the
[official download page](http://www.iausofa.org/current.html)

## Creating a Shared Library

Here is the tricky part. You can either modify the existing makefile to
produce a shared library, or you can use the CMake build script and instructions
I provide below. The CMake file will automatically build the library and
run SWIG to create a Python interface.

After extracting SOFA, `cd` into the main directory and create a `CMakeLists.txt`
file with the following content:

    cmake_minimum_required(VERSION 2.6)
    
    project(sofa_c C)
      
    # Set a few variables:
    set(LIBS ${LIBS} m)
    
    # Extract all of the source files.
    file(GLOB_RECURSE C_SOURCE . src/*.c)
    
      
    # Build a shared library
    add_library(sofa_c SHARED ${C_SOURCE})
    
      
    # Now define the installation options
    install(TARGETS sofa_c LIBRARY DESTINATION lib)

Now you can easily build and install SOFA as follows:

    cmake .
    make
    make install

## Installing pysofa

Here you need to install [pysofa](http://pypi.python.org/pypi/pysofa). It has
three requirments (1) Python 2.5 or higher, (2) numpy, and (3) a shared library
version of sofa (which we just installed). You can install all of this on the
python terminal by

    sudo aptitude install numpy
    easy_install pysofa

And that's it! If it all worked you can fire up the python interpreter and
`import pysofa` and you'll have access to all of the SOFA library functions.
