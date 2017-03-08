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
    1. [Sherlock](#first-time-sherlock)
    2. [CEES](#first-time-cees)
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
echo $'\nexport PATH=/home/vossj/suncat/bin:$PATH' >>~/.bashrc
echo 'export LD_LIBRARY_PATH=/home/vossj/suncat/lib:/home/vossj/suncat/lib64:$LD_LIBRARY_PATH' >>~/.bashrc
source ~/.bashrc
```

This will enable you to run SUNCAT specific software on the Sherlock cluster, including the ASE interface to Quantum ESPRESSO.

There are two file partitions, the `home` and the `scratch` partition. Go ahead and make a symbolic link to the `scratch` partition using:

```bash
ln -s $SCRATCH scratch
```

**Perform all your calculations from the scratch partition.**

<a name='first-time-cees'></a>

**CEES only**:

If you access the CEES cluster from off-campus or wireless connection at Stanford Residences, you need to connect to Stanford’s VPN service before login to the cluster. The information regarding to Stanford’s VPN can be found [here](http://uit.stanford.edu/service/vpn).

For the **first login** only, run the following command:

```bash
echo "setenv PATH /home/vossj/suncat/bin:${PATH}" >> ~/.tcshrc
source ~/.tcshrc
```

Create a folder in `/data/cees/`, from where you will create additional folders for performing your calculations. Type the following to create a directory and a symbolic link from the home directory:

```bash
mkdir /data/cees/$USER
ln -s /data/cees/$USER
```

**Make sure to perform all your work in this directory.**


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


