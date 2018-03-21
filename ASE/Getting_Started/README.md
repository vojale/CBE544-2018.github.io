---
layout: page
mathjax: true
permalink: /ASE/Getting_Started/
---

# ASE Tutorials
1. [Introduction to ASE](../)
2. [Getting Started with DFT Calculations](../Getting_Started/)


____

## Getting Started with DFT Calculations ##

In the first exercise, we will be studying perovskite oxides and how to determine their lattice constants, followed by surface relaxation of the (001) surface of the perovskite. For Homework 5, everyone will be studying the same system (001) SrTiO<sub>3</sub>. For the Final Project, you will use the same perovskite but with different facets (e.g.,(110),(111) etc.).

## Contents ##

1. [A Typical ASE Script](#a-typical-ase-script)
2. [Lattice Constant Determination](#lattice-constant-determination)
3. [Convergence with k-points](#convergence-with-k-points)
4. [Relaxation](#relaxation)


<a name='a-typical-ase-script'></a>

### A Typical ASE Script ###

ASE scripts can be run directly in the terminal (in the login node) or submitting to external nodes. Generally, you will be submitting jobs to external nodes and only small scripts will be run on the login node. By default, all output from any submitted script will be written *from the directory where the submission command was executed*, so make sure you are inside the calculation folder before running the submission command.

There are two files that are necessary to run jobs on the Stampede2 cluster. The first is `spede_esp.sub`; this is the file that tells the scheduler how much time the job is allowed, how many processors it requires, and other pertinent information. First, notice the comments in the beginning. These include information such as how much time to allocate, the number of nodes required, what the names of the output and error files are, what the name of the job should be, and what your email is. 

```bash
#!/bin/bash

#SBATCH -J test        # Job Name
#SBATCH -A TG-CHE160084 # Allocation number: Do not change this
#SBATCH -o ll_out    # Output file name
#SBATCH -e ll_err    # Error file name
#SBATCH -n 16          # Total number of cores requested
#SBATCH -p development # which queue to run in
#SBATCH -t 00:30:00     # Run time (hh:mm:ss)
#SBATCH --mail-user=youremail@whatever.com # Make sure to change this!!!!
#SBATCH --mail-type=end # Emails you at the end of the job

which pw.x
cd $SLURM_SUBMIT_DIR
export TMPDIR=$SLURM_SUBMIT_DIR
echo $EPS_PSP_PATH
export OMP_NUM_THREADS=1

python Energy.py
```

Finally, the last line ```python relax.py``` picks the script you want to run. Therefore, you need to change the name of the file depending on which script you are running. We will be using this script later in this section for performing surface relaxation calculations on the (001) SrTiO<sub>3</sub> perovskite.


Let's look at how a typical ASE script is written. Open the [`relax.py`](relax.py) script. We import all the relevant ASE modules in for this calculation

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

Finally, the Quantum ESPRESSO calculator is attached to the `slab` Atoms object, the relaxation calculation is run, and the total energy of the system is output in the log file. 

To submit any job on Stampede2, use:

```bash
sbatch -J $PWD spede_esp.sub

```

<a name='lattice-constant-determination'></a>

#### Lattice Constant Determination ####

Find the [`Lattice_Constant.py`](Lattice_Constant.py) script in the `lattice` folder. This script calculates the different energies of the system as a function of the lattice constant. Before you run this job, make sure you read the comments within to understand what it does.

Remember to change the script name to Lattice_Constant.py in the `spede_esp.sub` file! Submit the script by running:

```bash
sbatch --job-name=$PWD spede_esp.sub
```
Here, `--job-name=$PWD` sets the current working directory as the job name. 

The output states the energy with respect to the given lattice constant.

**HW 5:** Plot the energies as listed above, and report the DFT lattice constant.

<a name='convergence-with-k-points'></a>

#### Convergence with k-Points ####
Next, we will determine how well-converged the total energy is with respect to the number of k-points in each direction. Modify the [`Lattice_Constant.py`](Lattice_Constant.py) script using the lattice parameter obtained from the previous section. Instead of looping over strain values as above, modify the script to keep the same lattice constant and loop over k-points instead. Try using k = (2,2,1), (4,4,1), (6,6,1), (8,8,1), and (10,10,1), and plot the energy as a function of k-points. Pick one and try to justify why it would be a reasonable choice. The relevant k-points will usually be known, since we have consistent settings that we use throughout the group. In principle, one should always check for convergence when working with a new system. (Side note: Think about why the last k-point is always 1).

**HW 5:** Show the k-point convergence plot, your pick for the k-points, and your rationale.

For the Final Project, we will be using (8x8x1) k-points for the (1x1) MXene surfaces, and (4x4x1) k-points for the (2x2) MXene surfaces.

We have also provided the `Lattice_Resize.py` script that reads in a .traj file, and changes the lattice to the correct version resized with the lattice constant provided. In the script, you need to change the lattice constant you want manually. Unlike the rest of the scripts given to you, this script can be run directly from the command line, without using the submission system, by using:

```bash
python Lattice_Resize.py
```


