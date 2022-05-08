# Logs for Spring 2020
## vmd cheat sheet
### view pbc box
```
pbc set {x y z} -all
pbc box
````
### showing priyank a demo

### Center pbc box to origin
```
pbc box -center bb
```

## 18.02.2022
### charmm custom tip3p file
```
/tip3p-charges-only-mayank.itp
```

### Generating gromacs files from pdb
```
gmx pdb2gmx -f start.pdb -o water_1.gro -ignh -p topol.top
gmx grompp -f water.mdp -c water_1.gro -p topol.top -o water.tpr -maxwarn 2
gmx mdrun -s water.tpr -o traj.trr
gmx traj -f water_1.gro -s water.tpr -of force.xvg -ox coords.xvg
```
[Answer on stack exchange](https://mattermodeling.stackexchange.com/questions/8681/how-to-generate-a-tpr-file-from-a-pdb-or-xyz-file-in-gromacs)

## 07.10.2021
### How to install [n2p2](https://github.com/CompPhysVienna/n2p2)
The instructions provided on the documentation may not be enough to install n2p2. Several other libraries need to be installed to be able to build the modules. The following help build `nnp-norm` on ey pc:
```bash
apt install libopenmpi-dev
apt install libeigen3-dev
apt install libgsl-dev
apt install libblas-dev
cd src
make nnp-norm
```

## 27.03.2021
<blockquote class="imgur-embed-pub" lang="en" data-id="unSt1nf"><a href="https://imgur.com/unSt1nf">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

## 07.03.2021
[N2p2 configuration (input.data) format example](https://github.com/CompPhysVienna/n2p2/blob/master/examples/nnp-predict/H2O_RPBE-D3/input.data)

## 28.02.2021
[N2p2 configuration (input.data) format documentation](https://compphysvienna.github.io/n2p2/topics/cfg_file.html?highlight=input%20data)

## 07.11.2020
> As a conceptual rule-of-thumb, any estimate for the average of an observable which is found to be based on fewer than ~20 statistically independent configurations (or trajectory segments) should be considered unreliable. There are two related reasons for this. First, any estimate of the uncertainty in the average based on a small number of observations will be unreliable because this uncertainty is based on the variance, which converges more slowly than the observable (i.e., average) itself. Secondly, any time the estimated number of statistically independent observations (i.e., effective sample size) is ~20 or less, both the overall sampling quality and the sample-size estimate itself must be considered suspect. This is again because sample-size estimation is based on statistical fluctuations that are, by definition, poorly sampled with so few independent observations. [reference paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2865156/#APP1)

## 29.08.2020

This is a composed log of everything done since last log:
- [lammps lost atoms - use neighbour skin as 2x dist a fast atom would travel](https://lammps.sandia.gov/threads/msg42509.html)

## 11.07.2020
- [paper](https://arxiv.org/pdf/1811.08630.pdf)
- [repo](https://github.com/BingqingCheng/ab-initio-thermodynamics-of-water)
<blockquote class="imgur-embed-pub" lang="en" data-id="LhkCjpm"><a href="//imgur.com/LhkCjpm">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>
FIG. 2. Oxygen-oxygen, oxygen-hydrogen, and hydrogenhydrogen radial distribution functions (RDF) at 300 K and
experimental density computed via (PI)MD simulations in
the NVT ensemble using the NN potential. The experimental
O-O RDF was obtained from Ref 37, and the experimental
O-H and H-H RDFs were taken from Ref. 38, 39.
## 07.07.2020
### Discussion with sir
- make sure the weights we are using are correct by replicating a result from the paper.
- run an unbiased simulation with n2p2 and compare energy
- use henderson hasselbach equation to calculate pH frmo free energy
- can we use order parameter l from the [giesler paper](https://science.sciencemag.org/content/291/5511/2121)


## 06.07.2020
### Discussion with sir
Finally picked up call after I called from a new number. Says will talk tomo

## 01.07.2020
### Discussion with sir
Didn't message

## 30.06.2020
### Discussion with sir
Presented literature survey for pH calculation - [presentation](https://docs.google.com/presentation/d/1mHU_hdjhVIe42KqFXsTb06gI6uNDVtC5umCl9J2UMKw/edit?usp=sharing). He said he would tell an approach tomo

## 13.06.2020
### Discussion with sir 
Asked me to write a report with lit survey on pH calculations.

#### List of articles
0. [Development of Methods for the Determination of pKa Values](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3747999/)
1. [Prediction of the pKa of Carboxylic Acids Using the ab Initio Continuum-Solvation Model PCM-UAHF](https://pubs.acs.org/doi/abs/10.1021/jp981922f)
2. [Estimation of pKa Using Semiempirical Molecular Orbital Methods. Part 1: Application to Phenols and Carboxylic Acids](https://onlinelibrary.wiley.com/doi/abs/10.1002/1521-3838(200211)21:5%3C457::AID-QSAR457%3E3.0.CO;2-5)
3. [Application of the multiple computer automated structure evaluation methodology to a quantitative structure–activity relationship study of acidity](https://onlinelibrary.wiley.com/doi/abs/10.1002/jcc.540150911)
4. [MULTICASE 1. A Hierarchical Computer Automated Structure Evaluation Program](https://onlinelibrary.wiley.com/doi/abs/10.1002/qsar.19920110208)
5. [pKa prediction from ab initio calculations](https://researchoutreach.org/articles/physical-sciences/pka-prediction-from-ab-initio-calculations/)
6. [Experiment stands corrected: accurate prediction of the aqueous pKa values of sulfonamide drugs using equilibrium bond lengths](https://pubs.rsc.org/en/content/articlelanding/2019/sc/c9sc01818b#!divAbstract)
7. [pKa Prediction from an ab initio bond length: part 2—phenols](https://pubs.rsc.org/en/content/articlelanding/2011/cp/c1cp20379g/unauth#!divAbstract)
8. [The AIBLHiCoS Method: Predicting Aqueous pKa Values from Gas-Phase Equilibrium Bond Lengths](https://pubs.acs.org/doi/abs/10.1021/acs.jcim.5b00580)

## 07.06.2020
### Discussion with sir
Sent [REPORT](https://docs.google.com/document/d/1ThTg9skSKa51SCDdLjbyzpVzqySepQRZNHuidlvy8mY/edit?usp=sharing)

### Discussion with Suresh
There is a significant difference in the values of energy from N2P2 and from CP2K calculations. (see y axis) 
<blockquote class="imgur-embed-pub" lang="en" data-id="ooievwN"><a href="//imgur.com/ooievwN">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

<blockquote class="imgur-embed-pub" lang="en" data-id="eViyZW5"><a href="//imgur.com/eViyZW5">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

However, since the energy profiles are similar(similar shape of graph), therefore the two calculations are equivalent. The same is shown by the follwoing graph which is roughly a straight line.
<blockquote class="imgur-embed-pub" lang="en" data-id="NEiyMwm"><a href="//imgur.com/NEiyMwm">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

Also, sent this resource to read about the concept: [https://pubs.acs.org/doi/abs/10.1021/cr1000173](https://pubs.acs.org/doi/abs/10.1021/cr1000173) - sent a copy on mail

## 01.06.2020
### Discussion with sir
The cell size used for cp2k calculations of trainng data is different from reported lattice size. therefore use the reported energy data. Also what is energy calculated from N2P2 during the trajectory run. 

### Results
<blockquote class="imgur-embed-pub" lang="en" data-id="MrGhXFa"><a href="//imgur.com/MrGhXFa">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

<blockquote class="imgur-embed-pub" lang="en" data-id="ooievwN"><a href="//imgur.com/ooievwN">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

<blockquote class="imgur-embed-pub" lang="en" data-id="1cAAGFS"><a href="//imgur.com/1cAAGFS">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

## 24.05.2020
To split a trajectory file into coordinates, that can be used for generating the input files for a cp2k simulation :
```bash
split -d -l 194 -a 4 --additional-suffix=.xyz dataset_200.xyz coordinates_training/training
```
- `-d` : makes the suffix numeric
- `-l <number>` : size of each chunk
- `-a` : the length of the suffix (default is 2)


## 20.03.2020
<blockquote class="imgur-embed-pub" lang="en" data-id="jJZ9QPc"><a href="//imgur.com/jJZ9QPc">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

<blockquote class="imgur-embed-pub" lang="en" data-id="huFbI7x"><a href="//imgur.com/huFbI7x">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

## 03.03.2020
### Discussion with sir
Reported that we are observing O[2-] species in the counting of protons, Oh- and water. Asked me to plot free energy histograms and figure out if the trajectory is correct. Need to see if any outliers in the free energy histogram.

According to Navya: the negative of restraint bias will be the free energy 

## 20.02.2020
### Discussion with sir
Instead of calculating concentration of OH- by counting the OH- using number of protons, use a cutoff method. 

## 18.02.2020
<blockquote class="imgur-embed-pub" lang="en" data-id="D1zE9QM"><a href="//imgur.com/D1zE9QM">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

### Discussion with sir
- Need to find disassociaiton constant of water. 
- Posed the following questions for this calculation : 
> To calculate the disassociation constant:
> - Kw = [H3O+][OH-]
> - Is it okay to use the concentration of [H+] instead of [H3O+]

## 22.01.2020
### Edited lammps input file 
Edited LAMMPS input file to dump output in `.xyz` format so that it can be analyzed in VMD and rdf be plotted to find if initial configuration is in `atomic units` or `angstrom`. 

```
dump            traj_xyz all custom 5000 xyz out.lammpstrj element x y z
```

### Reading paper
- Started reading [Ab Initio Molecular Dynamics Simulations of the Influence of Lithium Bromide Salt on the Deprotonation of Formic Acid in Aqueous Solution](https://pubs.acs.org/doi/abs/10.1021/acs.jpcb.9b04618)

## 21.01.2020
### Counting protons on 
<blockquote class="imgur-embed-pub" lang="en" data-id="kfokLRI"><a href="https://imgur.com/kfokLRI">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

<blockquote class="imgur-embed-pub" lang="en" data-id="FhmuHkm"><a href="https://imgur.com/FhmuHkm">View post on imgur.com</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

For the plot, need to make sure that the initial configuration was in angstrom. 



## 13.01.2020
### Discussion with sir
Showed the following RDF - the peaks are at roughly 2 a.u. and hence the config sent before was actually protons.

<blockquote class="imgur-embed-pub" lang="en" data-id="a/4U3x2Ei"  ><a href="//imgur.com/a/4U3x2Ei">RDF of O-H </a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

- Need to find number of protons in all of traiing data 
- Write parser for simulation files of umbrella sampling 
- Find protons in simulation files 

## 09.01.2020
### Discussion with sir
Again asked him to see the configuration. Wants me to do the follwing:
- plot radial distribution of O-O, O-H and H-H. (the peaks will tell if we have fallen upon some random config)
- which energy range is the first frame from 

## 06.01.2020

<blockquote class="imgur-embed-pub" lang="en" data-id="a/blm7mrA"><a href="//imgur.com/a/blm7mrA">Isolated protom in water</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

## 04.01.2020
### Discussion with sir
- Showed [report](https://docs.google.com/document/d/13d_2XpAIFdocU_h2dNsHb-7R-ck55aYQDembbHz1Pcc/edit#)
- Asked following questions:
  - Did I see a single protonation event
  - Show him a configuration with hydronium ion and hydroxyl ions 
  
## 03.01.2020
### Automation for running simulation 
- [LINK](https://github.com/fireballpoint1/umbrellaSamplingOfWater/blob/master/lammps_attempt1/interface-pinning/README.md) Created 2 bash scripts to create `plumed.dat` and `in.lmp` files. 
- Need to automate process in `z-ada.sh` for multiple files.
- Need to automate process of creating meta files.

## 02.01.2020
### TO-DO
- Plot using xmgrace
- Simulate for more windows

### Shuru se
- Got all simulation data need to figure out input format for wham
- May have made a mistake in restraint bias by doing sort.127 and printing sort.128, changed it and run another set of simulation in `batch2` in [commit](https://github.com/fireballpoint1/umbrellaSamplingOfWater/commit/a5f4e7bea8ab3ca31d0a1bcaca79c9a7e5530275)
- Got wham files from hemanth - write script to get the same format from my files 
- Make sem report for sir 

### Preparing files for WHAM
Got the following files from hemanth: <br>
1. [plumed file](https://gist.github.com/fireballpoint1/947d78e6a4c2d686ac2104b7372ba8cf)
2. meta file
<script src="https://gist.github.com/fireballpoint1/76b2047899660b244b4c0ba46f56a077.js"></script>


- use `sed -r 's/\S+//4' COLVARnvt_3 | sed -r 's/\S+//2' > COLVARnvt_3_cleaned` to remove the second column of file. Use flag -i to edit the file in place instead of std output.

### Running WHAM
I am running the wham by alan grossfield using the following command for the given output: 
```bash
[mayank.modi@gnode32:batch1]$ /home/mayank.modi/umbrellaSamplingOfWater/lammps_attempt1/interface-pinning/wham/wham/wham 0 1 5 10E-9 300 0 ./meta ./result.dat
# /home/mayank.modi/umbrellaSamplingOfWater/lammps_attempt1/interface-pinning/wham/wham/wham 0 1 5 10E-9 300 0 ./meta ./result.dat
#Number of windows = 5
# Dumping simulation biases, in the metadata file order 
# Window  F (free energy units)
# 0	0.000000
# 1	12.300000
# 2	40.300000
# 3	180.300000
# 4	17.800000
# No MC error analysis requested
```

### References 

The follwoing were used for reference to determine the command:
1. [http://membrane.urmc.rochester.edu/sites/default/files/wham/doc.pdf](http://membrane.urmc.rochester.edu/sites/default/files/wham/doc.pdf)
> `wham [P|Ppi|Pval] hist_min hist_max num_bins tol temperature numpad metadatafile freefile [num_MC_trials randSeed]` 
The first (optional) argument specifies the periodicity of the reaction coordinate. For a nonperiodic reaction coordinate (a distance, for example), it
should be left out. “P” means that the reaction coordinate has a periodicity
of 360, appropriate for angles. “Ppi” specifies a periodicity of 2*pi, appropriate for angles measured in radians. “Pval” specifies periodicty of some
6
arbitrary amount, val, which should be an integer or floating point number. For example, “P180.0” would be appropriate for an angle with twofold
symmetry.
hist min and hist max specify the boundaries of the histogram. As a rule,
all data points outside the range (hist min, hist max) are silently ignored.
The only exception is that if an entire trajectory is outside the range, the
program halts with an error message. The solution is to remove that file from
the metadata file. hist min and hist max should be floating point numbers.
num bins specifies the number of bins in the histogram, and as a result
the number of points in the final PMF. It should be an integer.
tol is the convergence tolerance for the WHAM calculations. Specifically,
the WHAM iteration is considered to be converged when no Fi value for
any simulation window changes by more than tol on consecutive iterations.
As the program runs, it prints the average change in the F values for the
most recent iteration. Obviously, this number will be smaller than tol before
the computation converges, because convergence is triggered by the largest
change as opposed to the average.
temperature is a floating point number representing the temperature in
Kelvin at which the weighted histogram calculation is performed. This does
not have to be the temperature at which the simulations were performed (see
below for discussion).
numpad specifies the number of “padding” values that should be printed
for periodic PMFs. This number should be set to 0 for aperiodic reaction
coordinates. It doesn’t actually affect the calculation in any way. Rather, it
just alters the final printout of the free energy, to make plotting of periodic
reaction coordinates simpler. This is more important for wham-2d than
wham.
metadatafile specifies the name of the metadata file. The format of this
file is described below.
freefile is the name used for the file containing the final PMF and probability distribution.
num MC trials and randSeed are both related to the performance of
Monte Carlo bootstrap error analysis. If these values are not supplied, error
analysis is not performed. num MC trials should be an integer specifying
the number of fake data sets which should be generated. randSeed is an
integer which controls the random number seed – the value you pick should
be irrelevant, but I let the user set it primarily for debugging purposes

2. [https://www.r-ccs.riken.jp/labs/cbrt/samples/wham_analysis/](https://www.r-ccs.riken.jp/labs/cbrt/samples/wham_analysis/)
