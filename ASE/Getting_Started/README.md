---
layout: page
mathjax: false 
permalink: /ASE/Getting_Started/
---

# ASE Tutorials
1. [Introduction to ASE](../)
2. [Getting Started](../Getting_Started/)
3. [Adsorption](../Adsorption/)

____

## Getting Started ##

In the first exercise, we will be looking at MXenes and how to determine lattice constants, then we will be setting up adsorbates. For homework 5, you will all be doing the same system (Ti<sub>2</sub>C). For the final project, you will use the same structure but with different atoms (e.g., Mo<sub>2</sub>N instead of Ti<sub>2</sub>C).

## Contents ##

1. [A Typical ASE Script](#a-typical-ase-script)
2. [MXenes](#mxene)
  1. [Lattice Constant Determination](#lattice-constant-determination)
  2. [Convergence with k-Points](#convergence-with-k-points)
3. [Next Steps](#next)

### Required Files ###

Obtain the required files by running:

on Stampede:

```bash
cd $SCRATCH
wget http://CBE544.github.io/ASE/Getting_Started/exercise_1_sherlock.tar
tar -xvf exercise_1_sherlock.tar
```

This should create a folder called `Exercise_1_Getting_Started/` containing subfolders with all the starter scripts you will need. By default, The output for your calculations will be written into the folder from where you submitted the script. To perform new calculations, you will generally be copying the scripts from these tutorials into new folders, modifying them, and submitting them.

<a name='a-typical-ase-script'></a>

### A Typical ASE Script ###

ASE scripts can be run directly in the terminal (in the login node) or submitting to external nodes. Generally, you will be submitting jobs to external nodes and only small scripts will be run on the login node. By default, all output from any submitted script will be written *from the directory where the submission command was executed*, so make sure you are inside the calculation folder before running the submission command.

There are two files that are necessary to run jobs on the Stampede cluster. The first is `spede_esp.sub`; this is the file that tells the scheduler how much time the job is allowed, how many processors it requires, and other pertinent information. First, notice the comments in the beginning. These include information such as how much time to allocate, the number of nodes required, what the names of the output and error files are, what the name of the job should be, and what your email is. 

```bash
#!/bin/bash

#SBATCH -J test        # Job Name
#SBATCH -A TG-CHE160084 # Allocation number: Do not change this
#SBATCH -o ll_out    # Output file name
#SBATCH -e ll_err    # Error file name
#SBATCH -n 16          # Total number of cores requested
#SBATCH -p normal # which queue to run in
#SBATCH -t 03:00:00     # Run time (hh:mm:ss)
#SBATCH --mail-user=youremail@whatever.com # Make sure to change this!!!!
#SBATCH --mail-type=end # Emails you at the end of the job

```
The next five lines tell the job exactly what to execute; do not change these lines.

Finally, the last line 

```bash
python Energy.py

```
picks the script you want to run. Therefore, you need to change the name of the file depending on which script you are running.


Let's look at how a typical ASE script is written. Open the [`Energy.py`](energy.py) script. We import all the relevant ASE modules in for this calculation

```python
from espresso import espresso
from ase.io import read, write
```

`from espresso import espresso` imports the Quantum ESPRESSO calculator for the ASE interface, and `from ase.io import read, write` imports the read and write commands for trajectory files.

An existing trajectory can be read in:

```python
# read in the slab
slab = read('Ti2C.traj')
```

Then, the Quantum ESPRESSO calculator is set up. All parameters related to the electronic structure calculation are included here. The following example shows typical parameters that we use in the group for MXene calculations.

```python
calc = espresso(pw=700,             #plane-wave cutoff
                dw=7000,                    #density cutoff
                xc='BEEF-vdW',          #exchange-correlation functional
                kpts=(8,8,1),   #k-point sampling;
                nbands=-20,             #20 extra bands besides the bands needed to hold
                #the valence electrons
                sigma=0.1,
                convergence= {'energy':1e-6,
                'mixing':0.1,
                'nmix':10,
                'mix':4,
                'maxsteps':500,
                'diag':'david'
                },	#convergence parameters
                output={'removesave':True},
                dipole={'status':False}, #dipole correction to account for periodicity in z
                spinpol=False,
                outdir='calcdir')	#output directory for Quantum Espresso files

```

Finally, the Quantum ESPRESSO calculator is attached to the `slab` Atoms object, the energy calculation is ran, and the total energy of the system is output in the log file (defined in the `spede_esp.sub` file above). 

To submit the job, use:

```bash
sbatch -J $PWD spede_esp.sub

```
The `-J $PWD` gives the name of the job as the current directory. Make sure this calculations runs correctly before proceeding. You should get a total energy of -3353.960 eV.

<a name='mxenes'></a>

<a name='lattice-constant-determination'></a>

#### Lattice Constant Determination ####

Find the [`Lattice_Relax.py`](Lattice_Relax.py) script in the `lattice` folder. This script calculates the different energies of the system as a dunction of the lattice constant. Before you run this job, make sure you read the comments within to understand what it does. You will later need to modify this file for the final project.

Remeber to change the script name to Lattice_Relax.py in the `spede_esp.sub` file! Submit the script by running:

```bash
sbatch --job-name=$PWD spede_esp.sub
```
Here, `--job-name=$PWD` sets the current working directory as the job name. 

The output states the energy with respect to the given lattice constant. Take this data and plot it however you choose. Fit ~5 points near the minimum of this function with a quadratic function. Then, use calculus to find the minimum energy, and thus the true lattice constant.

**HW 5:** Plot the energies as listed above, and report the true lattice constant.

To set the lattice constant of your MXene sheet (to the true one calculated above), open your .traj file in the `ase-gui`. Press `Ctrl + E` to open up the 'Expert Mode'. In the text box, type `A` and press `Enter`. You should now see this:

```bash
>>>A
array([[ 3.1, 0., 0.],
[-1.55, 2.6846, 0.],
0., 0., 18.687]])
```
The two-dimensional bulk modulus B describes the compressibility of a two-dimensional sheet (how difficult it is to stretch or compress). Take the lattice script given before, change the given value to the true lattice constant, and change the strain value to run from 0.98 to 1.02, with five steps of 0.1. Fit the energies with a quadratic function. Then, calculuate B via:

$B=S_{0}\frac{d^{2}E}{dS^{2}}$

where S is the surface area of the sheet (a variable) and S<sub>0</sub> is the true surface area. Note that in this case we are fitting the surface area S, _not_ the lattice constant! The surface area of the MXenes is given by:

$S=\frac{\sqrt{3}}{2}a^{2}$

**HW 5:** Report the two-dimensional bulk modulus of Ti<sub>2</sub>C.

<a name='convergence-with-k-points'></a>

#### Convergence with k-Points ####
Next, we will determine how well-converged the energy is with respect to the number of k-points in each direction. Modify the [`Lattice_Constant.py`](Lattice_Constant.py) script using the lattice parameter obtained from the previous section. Instead of looping over strain values as above, modify the script to keep the same lattice constant and loop over kpoints instead. Try using k = (2,2,1), (4,4,1), (6,6,1), (10,10,1), and plot the energy as a function of k-points. Pick one and try to justify why it would be a reasonable choice. The relevant k-points will usually be known, since we have consistent settings that we use throughout the group. In principle, one should always check for convergence when working with a new system. (Side note: Think about why the last kpoint is always 1).

**HW 5:** Show the k-point convergence plot, your pick for the k-points, and your rationale.

For the final project, we will be using (8x8x1) kpoints for the (1x1) MXene surfaces, and (4x4x1) kpoints for the (2x2) MXene surfaces.

**Next**: move on to [Adsorption](../Adsorption/) to learn about how to calculate adsorbates on your surface.
