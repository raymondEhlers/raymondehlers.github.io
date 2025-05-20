---
layout: post
title: "Setting up your software environment: 2025 edition"
date: 2025-05-19 09:05:00
description: "Minimal notes and resources to help get started"
tags: new-students software
categories: physics software
featured: false
---

Software is a key element of the physics toolbox. Although the code itself is not the final product -- the physics result is --, following best practices for software can help move the physics forward more quickly and reliably. Things change quickly in the software world, so I'm updating the [2024 version](2024-02-27-minimal-software-setup.md) of my software notes.

# Table of Contents

1. [Python](#python)
2. [Compiled code (c++)](#compiled-code)
3. [Normal workflow](#normal-workflow)

# Python

Since last year, `uv` has emerged as _the_ tool to manage python. It's fast and convenient, covering many of the pain points associated with managing python installations. Many excellent resources on using uv are a quick search away, so I'll just present a quick summary.

The below steps only need to be done once (except step 3, which is done once per project).

1. **Install uv**: [Official instructions are here](https://docs.astral.sh/uv/getting-started/installation/). Pick your favorite method - I personally use `pipx`, but I also have a fairly complicated setup. For first time users, the standalone script is probably a good starting point.

2. **Install python**: For many reasons, it's best to install your own python. As a rule of thumb, it's good to avoid the newest version for a few minor releases to allow time for packages to update and small bugs to be worked out. As of May 2025, I'd suggest using 3.13. This can be installed by uv with:

   ```bash
   # List available python versions (example from my system - macOS. Yours will look different)
   $ uv python list --managed-python
   cpython-3.14.0a6-macos-aarch64-none                 <download available>
   cpython-3.14.0a6+freethreaded-macos-aarch64-none    <download available>
   cpython-3.13.3-macos-aarch64-none                   <download available>
   cpython-3.13.3+freethreaded-macos-aarch64-none      <download available>
   cpython-3.12.10-macos-aarch64-none                  <download available>
   cpython-3.11.12-macos-aarch64-none                  <download available>
   cpython-3.10.17-macos-aarch64-none                  <download available>
   cpython-3.9.22-macos-aarch64-none                   <download available>
   cpython-3.8.20-macos-aarch64-none                   <download available>
   pypy-3.11.11-macos-aarch64-none                     <download available>
   pypy-3.10.16-macos-aarch64-none                     <download available>
   pypy-3.9.19-macos-aarch64-none                      <download available>
   pypy-3.8.16-macos-aarch64-none                      <download available>
   graalpy-3.11.0-macos-aarch64-none                   <download available>
   graalpy-3.10.0-macos-aarch64-none                   <download available>
   graalpy-3.8.5-macos-aarch64-none                    <download available>
   # Install python 3.13
   $ uv python install --managed-python cpython-3.13.3-macos-aarch64-none
   # Check where the new version was installed
   $ uv python list --managed-python
   cpython-3.14.0a6-macos-aarch64-none                 <download available>
   cpython-3.14.0a6+freethreaded-macos-aarch64-none    <download available>
   cpython-3.13.3-macos-aarch64-none                   /Users/<username>/.local/share/uv/python/cpython-3.13.3-macos-aarch64-none/bin/python3.13
   cpython-3.13.3+freethreaded-macos-aarch64-none      <download available>
   cpython-3.12.10-macos-aarch64-none                  <download available>
   cpython-3.11.12-macos-aarch64-none                  <download available>
   cpython-3.10.17-macos-aarch64-none                  <download available>
   cpython-3.9.22-macos-aarch64-none                   <download available>
   cpython-3.8.20-macos-aarch64-none                   <download available>
   pypy-3.11.11-macos-aarch64-none                     <download available>
   pypy-3.10.16-macos-aarch64-none                     <download available>
   pypy-3.9.19-macos-aarch64-none                      <download available>
   pypy-3.8.16-macos-aarch64-none                      <download available>
   graalpy-3.11.0-macos-aarch64-none                   <download available>
   graalpy-3.10.0-macos-aarch64-none                   <download available>
   graalpy-3.8.5-macos-aarch64-none                    <download available>
   ```

3. **Install pdm**: Although uv is extremely useful, there are some workflows that we'll also sometimes use a similar package management tool called `pdm`. We can use uv to install it:

   ```bash
   $ uv tool install pdm
   ```

   By installing with `uv tool`, `pdm` will be available in your shell.

4. **Create a virtual environment**: We want to ensure that dependencies don't interfere between projects. To quote from [last year](2024-02-27-minimal-software-setup.md):

   > Python can be [very useful](https://xkcd.com/353), but it can be tricky to get it [setup well](https://xkcd.com/1987). A quick summary of my usual habits is:
   >
   > - Always use a virtual environment! (often called a "venv" or "virtualenv"). These create walled off spaces to hold python packages.
   > - Use a separate virtual environment per project.

   `uv` can help with virtual environments too! To create one, go to the directory where you project is located, and then run:

   ```bash
   # Adapt the path to the one that you found above.
   $ uv venv --python /Users/<username>/.local/share/uv/python/cpython-3.13.3-macos-aarch64-none/bin/python3.13 .venv-3.13
   ```

   Be sure to adapt the path to the one you found above when you listed the python that was installed. The last argument is the name of the virtual environment. By convention it's `.venv`, but I often find it useful to append the major version to help keep track - i.e. `.venv-3.13`. You only need to create it once.

# Compiled code

As I noted in [last year's version](<(2024-02-27-minimal-software-setup.md)>), c++ is prevalent in physics, especially for expensive calculations. In practice, python is often used as a "glue language" to connect different packages which are implemented predominately in c, c++, or rust. For example, numpy mainly consistent of high performance functions written in c and fortran, with python wrapped around those functions to make them convenient to work with. There are some mechanisms to make this (somewhat) seamless, but we'll occasionally have to work at a level that breaks that abstraction. Which is to say, we also need a c++ compiler.

Setup will depend on your system. I'll cover a few common systems below. I'll rely on you to configure your shell appropriately so that the compiler can be found (should be automatic in most cases).

## macOS

Install basic development tools with:

```bash
$ xcode-select --install
```

This will provide you with a c++ compiler (clang), git, etc.

## Ubuntu / Debian

This is based on the [ALICE software stack installation](https://alice-doc.github.io/alice-analysis-tutorial/building/prereq-ubuntu.html). This is certainly more than you need, but also shouldn't cause too many problems to install everything.

```bash
$ sudo apt update -y
$ sudo apt install -y curl libcurl4-gnutls-dev build-essential gfortran libmysqlclient-dev xorg-dev libglu1-mesa-dev libfftw3-dev libxml2-dev git unzip autoconf automake autopoint texinfo gettext libtool libtool-bin pkg-config bison flex libperl-dev libbz2-dev swig liblzma-dev libnanomsg-dev rsync lsb-release environment-modules libglfw3-dev libtbb-dev graphviz libncurses-dev software-properties-common gtk-doc-tools cmake ninja
```

This will provide you with a c++ compiler (g++), git, etc.

## Windows

I don't use Windows anymore, so I'm not going to be a reliable source of information and help here. I'd suggest setting up a virtual machine and running linux (e.g. Ubuntu). However, if you want to try with Windows, you could try out the Windows Subsystem for Linux (WSL). I understand you can install the Ubuntu version with:

```bash
$ wsl --install
```

As a next step, I think you have to install Ubuntu packages as described above. However, you'll have to figure this out yourself.

# Normal workflow

After completing the setup steps above, the c++ compiler should be available in your shell by default. For python, you need to a bit of configuration to get into your normal workflow. It will look something like:

```bash
$ cd to/my/project/directory
# Load the virtual environment
$ source .venv-3.13/bin/activate
# The `(.venv-3.13)` in the prompt tells you that you're loaded the virtual environment.
# Now you can work as normal!
(.venv-3.13) $ python run_my_script.py
```

If you decide to switch projects, be certain to run `deactivate`. Then, when you get to your new project directory, load the virtualenv that corresponds to that project. Remember to make a separate virtualenv for each project!!
