---
layout: post
title: "Quick notes on setting up your software environment"
date: 2024-02-27 09:05:00
description: "Minimal notes and resources to help get started"
tags: new-students software
categories: physics software
featured: false
---

Physics is very much about investigations of the physics world, utilizing existing theory as well as our best new ideas to try to make sense of what we observe. In order to do any of that, we spend significant time writing code and working with software development tools. Below is an quick overview and summary of what we use, and how you can set it up for yourself.

Analyzing physics data and simulations requires writing code, integrating existing libraries with new algorithms and ideas. In heavy-ion physics, this is usually done with a mix of c++ and python (and sometimes even a bit of fortran :smile:). c++ has been our preferred language, especially when performance is critical, and python has risen in recent years since it allows for rapid development and testing of ideas.

My preferred mix is to use python as a higher level language to configure analyses, quickly implement new observables, and glue everything together. When improved performance or c++ libraries are required, we can drop down to c++, passing information back and forth using. eg [pybind11](https://github.com/pybind/pybind11) or others.

## HEP software

This is substantial overlap between the tools used by high-energy physics (HEP) and heavy-ion physics. There is excellent training on many topics from the [HEP software foundation](https://hepsoftwarefoundation.org/training/center.html), from using a shell through physics analysis tools. This is a great place to start.

Note that a lot of existing codes rely heavily on the [ROOT](https://root.cern/) framework, which is developed in c++, but also has python bindings (ie. you can use it in python, if setup appropriately). I have found other tools to be more effective for my work, especially based around python and packages developed by the [scikit-hep project](https://github.com/scikit-hep/). Nonetheless, I still use it from time-to-time, and it's worth understanding at at least a basic level since it's impact is so expansive - eg. it's file format is how we store petabytes of data from experiments!

## Python

Python can be [very useful](https://xkcd.com/353), but it can be tricky to get it [setup well](https://xkcd.com/1987). A quick summary of my usual habits is:

- Always use a virtual environment! (often called a "venv" or "virtualenv")
- Use a separate virtual environment per project.

For actually setting up python, others have covered this better than I could, so some resources are below. They include both instructions and recommendations. Note that these articles are quite opinionated and I haven't used them independently to set things up [^1], so I don't agree with every single thing stated, but it's a great starting point!

- [Installing python](https://www.bitecode.dev/p/installing-python-the-bare-minimum)
- [Recommendations on how to use virtual environments](https://www.bitecode.dev/p/back-to-basics-with-pip-and-venv)
- [A higher level overview + recommendations](https://www.bitecode.dev/p/relieving-your-python-packaging-pain). This is a bit more detailed, but provides intuition for a consistent workflow.

[^1]: Personally, I use a tool called [`pyenv`](https://github.com/pyenv/pyenv) to install and manage my python installations on macOS and linux. However, it can be quick tricky to use, so I wouldn't recommend it if you don't already have experience with it.

## c++

Setting up a c++ compiler often is a bit more uniform since we can usually be less picky about the precise compiler version. For macOS, install the XCode command line tools, which will provide `cling`. On linux or WSL, install a recent version of `g++`.

We use at least c++17 (or newer).

---
