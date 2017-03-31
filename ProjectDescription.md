---
layout: page
mathjax: true
permalink: /Project/
---

## Course Project ##
1. [Introduction](#intro)
3. [Calculations](#calcs)
3. [Analysis](#analysis)
4. [Report](#report)
5. [Research Paper](#paper)
6. [Grading](#grading)
7. [Summary of Requirements](#reqs)


For the Final Project, you will be studying thermo-chemical ammonia synthesis on 2D MXenes (M<sub>2</sub>X) with X = C or N. Each student will be assigned a MXene to work on either a carbide or a nitride (see list of Assigned Projects). The students will work in groups of two on the same MXene to perform calculations individually, however, complementing each other. Each pair of students will present their results in class that will be critiqued by another group of two. Finally, these four students will jointly write a final report on the combined data. The due date for the final written report is <font color="red">5/1 at 5:00 PM (hard deadline)</font>.

Turn in your final report by emailing a PDF file to:

```
alevoj@seas.upenn.edu, liangzha@seas.upenn.edu
```

<a name='intro'></a>

## Introduction ##

The thermo-chemical synthesis of ammonia is accomplished through the [Haber-Bosch process](http://en.wikipedia.org/wiki/Haber_process), where nitrogen gas reacts with hydrogen gas via:

$$
\mathrm{N_2+3H_2\rightarrow 2NH_3}
$$

This process is crucial for the industrial production of fertilizers and chemical feedstocks. Typically, an iron catalyst is used to stabilize the bond-breaking of the N<sub>2</sub> species. The reaction can be separated into elementary reaction steps ([Honkala et. al. (2005)](http://dx.doi.org/10.1126/science.1106435) for more details):

$$
\begin{align}
\mathrm{N_{2\,(g)}} &\rightarrow \mathrm{2N*}\\
\mathrm{H_{2\,(g)}} &\rightarrow \mathrm{2H*}\\
\mathrm{N* + H*} &\rightarrow \mathrm{NH*}\\
\mathrm{NH* + H*} &\rightarrow \mathrm{NH_2*}\\
\mathrm{NH_2* + H*} &\rightarrow \mathrm{NH_3*}\\
\mathrm{NH_3*} &\rightarrow \mathrm{NH_{3\,(g)}}
\end{align}
$$

A free energy diagram is illustrated below:

<center><img src="../Images/N2_path.jpg" alt="N2 path" style="width: 450px;"/>
<br>Ammonia synthesis pathway on a Ru catalyst (<a href="http://dx.doi.org/10.1126/science.1106435">Honkala et. al. (2005)</a>)</center>

Due to the high operating pressures and temperatures required for this reaction, alternative catalysts are still needed for this process. [Medford et. al. (2015)](http://dx.doi.org/10.1016/j.jcat.2014.12.033) have suggested that the linear scaling between the dissociation energy of N<sub>2</sub> and its transition state energy prevents most catalysts from achieving a high rate. Assuming that the bond-breaking of N<sub>2</sub> is rate limiting, then traditional metal catalysts have a transition state that is too high in energy. This is illustrated in the filled contour plot below, where the turnover frequency is plotted as a function of the transition state energy of the first N<sub>2</sub> bond breaking (*E*<sub>N-N</sub>) and the dissociation energy (∆*E*<sub>diss</sub>). A catalyst would need to behave differently from these extended surfaces in order to land in a more active region of the map. 

<center><img src="../Images/N2_volcano.png" alt="N2 volcano" style="width: 400px;"/>
<br>Filled contour plot for the turnover frequencies (Singh et. al. (2016))</center>

We will be exploring 2D MXenes where such configurations might be found. 

Your goals for the project will be to: 
(1) explore this reaction to find unique adsorption configurations and possibly more favorable thermodynamics,
(2) explore if the theormodynamics can be changed with functionalization of the MXenes,
(3) explore relations between the reaction intermediates in the pathway,
(4) compare the chemsitry of the carbide and nitride MXenes.

<a name='calcs'></a>

## Calculations ##

For the Final Project, create a `CBE544Project` folder in your `$SCRATCH` directory by running:

```bash
mkdir $SCRATCH/CHE444Project/
```

You may run the exercises in any directory (as long as it is under `$SCRATCH`), but keep all the final files for the project organized.

To describe the full reaction on your catalytic system, you will need to calculate the adsorption energies of all intermediates, in their most stable configuration (N\*, NH\*, NH<sub>2</sub>\*, NH<sub>3</sub>\*, H\*). A mean field approximation can be used in the analysis (*e.g.* ∆*E*<sub>2NH</sub> = 2∆*E*<sub>NH</sub>). You are not required to calculate any of the transition states for this assignment. Instead use the universal BEP relations for N<sub>2</sub> dissociation.

First, download and unarchive the files you need via:

```bash
wget https://github.com/CBE544/CBE544.github.io/raw/master/Archive.tar
tar -zxvf Archive.tar
```

This will create a directory named `Class`. Within, you will find pre-relaxed .traj files for the project. Your team will need the bare, O-terminated, and H-terminated MXenes. They are labeled as M2X.traj for the bare, M2XO2.traj for the O-terminated, and M2XH2.traj for the H-terminated; e.g. for Mo<sub>2</sub>C, the .traj file is Mo2C.traj for the bare MXene, Mo2CO2.traj for the O-terminated, and Mo2CH2.traj for the H-terminated.

In summary:

1. Structural relaxations on both the bare MXene and the two functionalized MXenes; that is, O-terminated and H-terminated. [Project Part 1](../ASE/Getting_Started)
2. Adsorption energies for the intermediates in the adsorbed state (N\*, NH\*, NH<sub>2</sub>\*, NH<sub>3</sub>\*, H\*). Check all possible sites in order to determine optimal adsorption configurations. [Project Part 2](../ASE/Adsorption)
3. Energy diagrams for the overall reaction.
4. Calculation of the reaction rate and also a free energy diagram with some temperature and pressure dependence. [Project Part 3](../ASE/Transition_States)

Once you have finished the required calculations, you are free to explore other features of the reaction as you see fit.

**IMPORTANT:**

When you have finished all your calculations. Confirm that your results are organized in the following way:

```bash
../CBE544Project/bare/
../CBE544Project/bare/Adsorption/
../CBE544Project/bare/Adsorption/N/
../CBE544Project/bare/Adsorption/N/config
../CBE544Project/bare/Adsorption/NH/
../CBE544Project/bare/Adsorption/NH/config
...
../CBE544Project/O-term/
../CBE544Project/O-term/Adsorption/
../CBE544Project/O-term/Adsorption/N/
../CBE544Project/O-term/Adsorption/N/config
../CBE544Project/O-term/Adsorption/NH/
../CBE544Project/O-term/Adsorption/NH/config
...
../CBE544Project/H-term/
../CBE544Project/H-term/Adsorption/
../CBE544Project/H-term/Adsorption/N/
../CBE544Project/H-term/Adsorption/N/config
../CBE544Project/H-term/Adsorption/NH/
../CBE544Project/H-term/Adsorption/NH/config
...
```

where `CBE544Project` is your **project directory**. You should rename `config` to describe the binding configuration, such as `fcc`, `hcc`, `top`, `bridge` sites. You should have one calculation per directory. Run the following to copy all your files into the shared course directory, so your classmates may access the results.

On Sherlock, from your **project directory**, run:

```bash
/scratch/PI/suncat/chemeng444_2016/submit
```

<a name='analysis'></a>

## Analysis ##

Your analysis should include the following:

* Discussion of the optimal binding configurations on the surface
* Comparison between the (111) surface and the M<sub>13</sub> cluster
* Analysis of rate as a function of the temperature

Beyond these points, you may discuss anything you find interesting. Here are some ideas:

* Do the reaction intermediates (adsorbates) show the same dependence on surface site or surface termination?
* What are other factors that can affect adsorbate-metal interaction?
* How important is the coordination number of the metal? Compare the metal cluster to the metal surface.

You are welcome to share data amongst your peers to discuss broader trends. 

**If you need the energy of the fixed clusters, they are available [here](../Fixed_Lattice_Clusters/energies.txt).**

<a name='report'></a>

## Report ##

Your report should be between 3 to 5 pages long including figures and tables. Please be succinct and organize it in the following way:

* Introduction (brief) - don't write too much
* Calculation details
* Results and discussion
* Conclusion (brief)

<a name='paper'></a>

## Research Paper ##

We expect that your results will form the basis of a manuscript that we will be putting together as a class. For previous class papers:

* [(2011) Finite-Size Effects in O and CO Adsorption for the Late Transition Metals, *Topics in Catalysis*](http://dx.doi.org/10.1007/s11244-012-9908-x)
* [(2015) Direct Water Decomposition on Transition Metal Surfaces: Structural Dependence and Catalytic Screening, *Catalysis Letters*](http://dx.doi.org/10.1007/s10562-016-1708-7).
 
Notice how much the class size has grown!

<a name='grading'></a>

## Grading ##

* 30% exercises
* 20% write-up
* 20% kinetics
* 30% calculations

<a name='reqs'></a>

## Summary of Requirements ##

At a minimum you should accomplish the following:

1. Complete the [three exercises](../ASE/).
2. Setup a M<sub>13</sub> cluster and a (111) surface and calculate adsorption energies for all intermediates.
3. Calculate transition states for the first step N<sub>2</sub> dissociation) using the fixed bond-length method. Extra credit for calculating the hydrogenation barriers.
4. Vibrational frequency and free energy calculations (initial, transition, and final states, and all adsorbed intermediates). 
5. Analysis
    1. Optimal adsorption sites (relation to transition states)
    2. Kinetic rate analysis
6. Report (3~5 pages maximum)

Email your final report as a PDF document to:

`ctsai89@stanford.edu, aayush@stanford.edu, shaama@stanford.edu, ambarish@stanford.edu`
