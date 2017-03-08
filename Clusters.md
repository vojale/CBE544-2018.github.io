---
layout: page
mathjax: false 
permalink: /Clusters/
---

# Getting Started
1. [Logging Into the Computing Clusters](../Clusters/)
2. [Basic UNIX](../UNIX/)
3. [Python](../Python/)

____

## Logging Into the Computing Clusters

Half of the class have been assigned computing accounts on Sherlock and the other half have been assigned accounts on CEES. Follow the instructions for using the one you have been assigned to and follow the tests to make sure everything is set up properly and functional.

## Contents
1. [Installation](#installation)
2. [Logging On](#logging)
3. [First Time Logging On](#first-time)
4. [Making Sure Everything Works](#testing)

<a name='installation'></a>

## Installation

### Mac OSX
Download and install:

* [XQuartz](http://www.xquartz.org/)

To prevent X11 from timing out, open the terminal and type:

```bash
mkdir -p ~/.ssh
echo $'\nHost *\n ForwardX11Timeout 1000000\n' >>~/.ssh/config
```


### Windows

Download and install:

* [PuTTY](http://www.putty.org/)
* [Xming](http://sourceforge.net/projects/xming/) (Note: disable automatic installation of PuTTY with Xming. The above installer is a newer version)


### Linux (Debian-based, e.g. Ubuntu)
From the terminal
____

<a name='logging'></a>

## Logging onto the Clusters

For the [**Stampede**](http://stampede.tacc.utexas.edu) cluster, make sure to read through the User Guide [here](https://portal.tacc.utexas.edu/user-guides/stampede).

login with your XSEDE username and password.

Follow the instructions below for your system:

### Mac OSX

Then,

```bash
ssh -X username@stampede.tacc.utexas.edu
```

### Windows
Launch Xming. You will always need to have this open in order to forward graphical windows from the external clusters.

Start PuTTY, and:

* “Session” → “Host Name” `username@stampede.tacc.utexas.edu` for **Stampede**
* “Connection” → “SSH” → “X11” check “Enable X11 forwarding”
* Back in “Session”, you can **save these settings for next time**.

You can start putty several times, if you need several terminal windows; only one instance of Xming needed.


### Linux ###

In a terminal (Sherlock only):

Then for **Stampede**:

```bash
ssh -X username@stampede.tacc.utexas.edu
```
____

<a name='first-time'></a>

### First Time Logging in ###

<a name='first-time-sherlock'></a>

**Sherlock only**:

For the **first login** only, run the following command:

```bash
cp /home1/03672/tg829713/vojgroup/bash_script/bashrc_copy ~/.bashrc 
source ~/.bashrc
```

This will enable you to run specific software on the Stampede cluster, including the ASE interface to Quantum ESPRESSO.

There are two file partitions, the `home` and the `work` partition. Go ahead and make a symbolic link to the `work` partition using:

```bash
ln -s $WORK work
```

**Perform all your calculations from the `work` partition.**

<a name='first-time-cees'></a>
____

<a name='testing'></a>

## Making Sure Everything Works ##

Once you are logged into the terminal, run:

```bash
ase-gui
```

and make sure a graphical interface appears. Next, run Python in interactive mode by typing:

```bash
python
```

and make sure the following commands work:

```python
import ase
import numpy
```


