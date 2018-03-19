---
layout: page
mathjax: true
permalink: /ASE/
published: true
---

# ASE Tutorials
1. [Introduction to ASE](../ASE/)
2. [Getting Started with DFT Calculations](Getting_Started/)


____

## Introduction to ASE

## Contents
1. [Atomic Simulation Environment (ASE)](#backgroun)
2. [Graphical User Interface (`ase-gui`)](#ase-gui)

<a name='background'></a>

### Atomic Simulation Environment (ASE) ###
In this class, we will use the [Quantum ESPRESSO](http://www.quantum-espresso.org) calculator as implemented in the Atomic Simulation Envrionment (ASE) for density functional theory (DFT) calculations. ASE provides Python modules for manipulating atoms, performing calculations, and analyzing and visualizing the results. ASE scripts are simply regular Python scripts that incorporate the ASE modules. For detailed documentation, refer to the official [ASE website](http://wiki.fysik.dtu.dk/ase/index.html). We will be demonstrating all the features you will need to use, on the project website, but you may want to explore additional functionality on your own.

Here is an example of a few commonly used modules for a calculation and how you would use them in your Python scripts:

```python
from ase import Atoms
from ase import optimize
from espresso import espresso
import numpy as np
```

The modules provide functions that can be used for setting up the system and performing calculations. The `ase.optimize` module is needed for performing geometry optimizations, the `espresso` module is needed to use the Quantum ESPRESSO calculator, and the `numpy` module is needed for using certain mathematical functions.

Typically, the script will first read a trajectory file (`.traj`) that contains the structure, then set up the Quantum ESPRESSO calculator, and then perform the calculation (often a structural optimization). You can use the ASE graphical user interface `ase-gui` to view trajectory files or to setup or modify structures. To visualize a trajectory file, simply type:

```bash
ase-gui <trajectory_file>.traj
```

ASE supports a variety of file formats. More information about ASE can be found in the [official documentation](http://wiki.fysik.dtu.dk/ase/ase/ase.html).

<a name='ase-gui'></a>

### Graphical User Interface (`ase-gui`) ###

As mentioned above, you can use the graphical user interface to view atomic structures. It is also convenient for adding or manipulating atoms inside your system. Read all the details [here](http://wiki.fysik.dtu.dk/ase/ase/gui/gui.html).

Log on to Stampede2, download an example and take a look:

```bash
wget https://cbe544.github.io/CBE544-2018.github.io/ASE/sto.traj
ase-gui sto.traj
```

You should see the following window:

<center><img src="Images/gui-slab.png" alt="window" style="width: 400px;"/><br>
ASE GUI interface
</center>

To modify/replace an atom, select an existing atom and go to `Edit > Modify` or press `Ctrl+Y`. In the resulting dialog box, choose the atom you want to replace the selected atom with. After selecting OK, the new atom selected will be replaced in place of the atom you selected. If you want to move the atom, you can use `Tools > Move atoms` or `Ctrl+M`. The atom should have a green outline and you can use your arrow keys to move its position. You can simply hold down `Ctrl` to select multiple atoms.

<center><img src="Images/gui-mod.png" alt="add" style="width: 400px;"/><br>
Modifying/Replacing an atom
</center>

You can repeat the slab to create a larger supercell. This is convenient for identifying symmetric sites, which might not be obvious at first glance from the unit cell. To do this:

<center><img src="Images/gui-repeat.png" alt="add" style="width: 400px;"/><br>
Repeating a slab
</center>

**Next**: move on to [Getting Started with DFT Calculations](Getting_Started/) when you're ready to start doing some DFT!
