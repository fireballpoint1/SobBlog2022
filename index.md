# Digital notebook
- [Link to formatting guide page](./index1.md).
- [Links-References-Commands](./linksReferencesCommands.md)
- [Spring 2020 logs](./spring2020.md)

## 02.01.2020
- Got all simulation data need to figure out input format for wham
- May have made a mistake in restraint bias by doing sort.127 and printing sort.128, changed it and run another set of simulation in `batch2` in [commit](https://github.com/fireballpoint1/umbrellaSamplingOfWater/commit/a5f4e7bea8ab3ca31d0a1bcaca79c9a7e5530275)
- Got wham files from hemanth - write script to get the same format from my files 
- Make sem report for sir 

### Preparing files for WHAM
- use `sed -r 's/\S+//2' COLVARnvt_1` to remove the second column of file. Use flag -i to edit the file in place instead of std output.

## 10.12.2019
### Run NVE with plumed fix [ sent the files for this to sir ] 
- file `in2.lmp` 
- getting different bias now 
- nve not unfixed before the plumed fix 
- not sure about the units of the distance yet

### Discussion with dir 
- need only the restraint bias for umbrella sampling 

## 09.12.2019
### Don't think the CV defined in plumed file is being used
- Read something [here](https://arxiv.org/pdf/1812.08213.pdf)

## 05.12.2019
### SUCCESS : LAMMPS + plumed + n2p2
Semprathi sir installed a working version of this. Use the following to access it :
```
$ sinteractive -c 8

$ module load openmpi/4.0.0

$ export LD_LIBRARY_PATH=/opt/lammps-7Aug19_nnp_plumed/lib/nnp/lib

$ mpirun -np 8 /opt/lammps-7Aug19_nnp_plumed/src/lmp_mpi

```
## 29.11.2019
- Reading about pH calculation methods in gromacs [here](https://www.nature.com/articles/srep22523)

## 28.11.2019
### LAMMPS + plumed + n2p2
Instead of running `Install.py` myself in the lib directory. Do the following: 
```
cd lammps/src
make yes-user-plumed
make lib-plumed -b -v 2.5.1
make mpi
```
However, still running into an error regarding C++11 compliant compiler.

## 27.11.2019
### LAMMPS + plumed + n2p2
- [mailing list option](https://lammps.sandia.gov/threads/msg77839.html)
> I suggest you try the latest patch release (Aug16) which has a CMake build option.
It can auto-build lots of packages and can auto-download libs that LAMMPS
doesn't supply.  Traditional make can do many of the same things
if you use the make lib-foo kind of commands, where foo = package name.
But CMake is much more automated.

- [useful git repo](https://github.com/lammps/lammps/tree/master/cmake#quick-start-for-the-impatient)
```
git clone https://github.com/lammps/lammps.git
cd lammps/src 
make yes-user-plumed
cd ../lib/plumed
python Install.py -b -v 2.5.1
cd ../../src/
make mpi 
```


## 19.11.2019
### Trying out [pre-patched LAMMPS](https://github.com/plumed/conda#install-lammps)
- Cloned the repo on ada in `/home/mayank.modi/Software/lammps_pre_patched`
- Run conda environment `conda activate prepatched`
- Install plumed as per repo `conda install -c plumed/label/lugano -c conda-forge plumed`
- Install prepatched lammps as per repo `conda install -c plumed/label/lugano -c conda-forge lammps`

- **TO DO** Check what can be done with `lammps/build.sh` and `.travis.yml` files to fix n2p2 on this version. 
- **Check reply to the issue [here](https://github.com/plumed/conda/issues/1)**

## 14.11.2019
### Build lammps with plumed 
- Trying to fix the error [here](https://github.com/fireballpoint1/summerResearch/blob/gh-pages/index.md#error-making-lammps-with-plumed) by doing this:
```
cd ~/mylammps/src
ln -s ~/Software/for_lammps/include/plumed ./plumed
```
But failed because couldn't find `plumed_finalize` and `plumed_cmd_nothrow`. **Possible fix** [here](https://groups.google.com/forum/#!msg/plumed-users/f-bnMk_HQSw/Hf5DqKDTetsJ)

### Running plumed on ada
Since plumed was built while `openmpi/3.1.0` was loaded, it runs on mpi by default but running plumed gives the following error:
```
[gnode12:14911] OPAL ERROR: Not initialized in file pmix2x_client.c at line 109
--------------------------------------------------------------------------
The application appears to have been direct launched using "srun",
but OMPI was not built with SLURM's PMI support and therefore cannot
execute. There are several options for building PMI support under
SLURM, depending upon the SLURM version you are using:

  version 16.05 or later: you can use SLURM's PMIx support. This
  requires that you configure and build SLURM --with-pmix.

  Versions earlier than 16.05: you must use either SLURM's PMI-1 or
  PMI-2 support. SLURM builds PMI-1 by default, or you can manually
  install PMI-2. You must then build Open MPI using --with-pmi pointing
  to the SLURM PMI library location.

Please configure as appropriate and try again.
--------------------------------------------------------------------------
*** An error occurred in MPI_Init
*** on a NULL communicator
*** MPI_ERRORS_ARE_FATAL (processes in this communicator will now abort,
***    and potentially your MPI job)
[gnode12:14911] Local abort before MPI_INIT completed completed successfully, but am not able to aggregate error messages, and not able to guarantee that all other processes were killed!
```
 **FIX** :   Use plumed commands with `--no-mpi` flag.

## 13.11.2019

### Error making lammps with plumed
```
mpicxx -g -O3   -DLAMMPS_GZIP -DLAMMPS_MEMALIGN=64 -I../../lib/plumed/includelink -I../../lib/nnp/include  -DMPICH_SKIP_MPICXX -DOMPI_SKIP_MPICXX=1   -D__PLUMED_HAS_DLOPEN=1 -D__PLUMED_DEFAULT_KERNEL=/libplumedKernel.so   -c ../npair_full_nsq_ghost.cpp
mpicxx -g -O3   -DLAMMPS_GZIP -DLAMMPS_MEMALIGN=64 -I../../lib/plumed/includelink -I../../lib/nnp/include  -DMPICH_SKIP_MPICXX -DOMPI_SKIP_MPICXX=1   -D__PLUMED_HAS_DLOPEN=1 -D__PLUMED_DEFAULT_KERNEL=/libplumedKernel.so   -c ../fix_box_relax.cpp
mpicxx -g -O3   -DLAMMPS_GZIP -DLAMMPS_MEMALIGN=64 -I../../lib/plumed/includelink -I../../lib/nnp/include  -DMPICH_SKIP_MPICXX -DOMPI_SKIP_MPICXX=1   -D__PLUMED_HAS_DLOPEN=1 -D__PLUMED_DEFAULT_KERNEL=/libplumedKernel.so   -c ../nstencil_half_bin_2d_newton.cpp
mpicxx -g -O3   -DLAMMPS_GZIP -DLAMMPS_MEMALIGN=64 -I../../lib/plumed/includelink -I../../lib/nnp/include  -DMPICH_SKIP_MPICXX -DOMPI_SKIP_MPICXX=1   -D__PLUMED_HAS_DLOPEN=1 -D__PLUMED_DEFAULT_KERNEL=/libplumedKernel.so   -c ../fix_plumed.cpp
../fix_plumed.cpp:39:35: fatal error: plumed/wrapper/Plumed.h: No such file or directory
compilation terminated.
Makefile:104: recipe for target 'fix_plumed.o' failed
make[1]: *** [fix_plumed.o] Error 1
make[1]: Leaving directory '/home/mayank.modi/mylammps/src/Obj_mpi'
Makefile:201: recipe for target 'mpi' failed
make: *** [mpi] Error 2
```

- Need to figure out the error when doing a `make mpi` with lammps. Possible description of error on [github of plumed]
(https://github.com/peastman/openmm-plumed/issues/10)

### Plumed Install 
Bashrc looks like this now (on ada): 
```
## Prompted by plumed config to do this 
_plumed() { eval "$(plumed --no-mpi completion 2>/dev/null)";}
complete -F _plumed -o default plumed

## Prompted by plumed install to do this 
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/home/mayank.modi/Software/for_lammps/lib/pkgconfig
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/mayank.modi/Software/for_lammps/lib
export PLUMED_KERNEL=/home/mayank.modi/Software/for_lammps/lib/libplumedKernel.so
alias plumed=/home/mayank.modi/Software/for_lammps/bin/plumed
export PLUMED_USE_LEPTON=yes
```

Installed plumed on ada with the following logs after `make install`
```
*** PLUMED has been installed ***

Install prefix : /home/mayank.modi/Software/for_lammps
Full name      : plumed

Setup your environment
- Ensure this is in your execution path         : /home/mayank.modi/Software/for_lammps/bin
- Ensure this is in your include path           : /home/mayank.modi/Software/for_lammps/include
- Ensure this is in your library path           : /home/mayank.modi/Software/for_lammps/lib
- Ensure this is in your PKG_CONFIG_PATH path   : /home/mayank.modi/Software/for_lammps/lib/pkgconfig
For runtime binding:
- Set this environment variable                 : PLUMED_KERNEL=/home/mayank.modi/Software/for_lammps/lib/libplumedKernel.so

To create a tcl module that sets all the variables above, use this one as a starting point:
/home/mayank.modi/Software/for_lammps/lib/plumed/modulefile

To uninstall, remove the following files and directories:
/home/mayank.modi/Software/for_lammps/lib/plumed
/home/mayank.modi/Software/for_lammps/share/doc/plumed
/home/mayank.modi/Software/for_lammps/include/plumed
/home/mayank.modi/Software/for_lammps/bin/plumed
/home/mayank.modi/Software/for_lammps/bin/plumed-patch
/home/mayank.modi/Software/for_lammps/bin/plumed-config
/home/mayank.modi/Software/for_lammps/lib/pkgconfig/plumed.pc
/home/mayank.modi/Software/for_lammps/lib/libplumed.so
/home/mayank.modi/Software/for_lammps/lib/libplumedKernel.so
A vim plugin can be found here: /home/mayank.modi/Software/for_lammps/lib/plumed/vim/
Copy it to /home/mayank.modi/.vim/ directory
Alternatively:
- Set this environment variable         : PLUMED_VIMPATH=/home/mayank.modi/Software/for_lammps/lib/plumed/vim
- Add the command 'let &runtimepath.=','.$PLUMED_VIMPATH' to your .vimrc file
From vim, you can use :set syntax=plumed to enable it
WARNING: plumed executable will not run on this machine
WARNING: unless you invoke it as 'plumed --no-mpi'
WARNING: This is normal if this is the login node of a cluster.
WARNING: - to patch an MD code now use 'plumed --no-mpi patch'
WARNING:   (notice that MPI will be available anyway in the patched code)
WARNING: - all command line tools are available as 'plumed --no-mpi name-of-the-tool'
WARNING:   e.g. 'plumed --no-mpi driver'
WARNING:   (MPI will be disabled in this case)
make[2]: Leaving directory '/home/mayank.modi/plumed-2.5.3/src/lib'
make[1]: Leaving directory '/home/mayank.modi/plumed-2.5.3/src'
```

## 12.11.2019
The make on lab machine was failing due to improper install with plumed :
```
user@user-Vostro-3669:~/mylammps/src$ make serial
make[1]: Entering directory '/home/user/mylammps/src/STUBS'
make[1]: 'libmpi_stubs.a' is up to date.
make[1]: Leaving directory '/home/user/mylammps/src/STUBS'
Gathering installed package information (may take a little while)
make[1]: Entering directory '/home/user/mylammps/src'
make[1]: 'lmpinstalledpkgs.h' is up to date.
Gathering git version information
make[1]: Leaving directory '/home/user/mylammps/src'
Compiling LAMMPS for machine serial
make[1]: Entering directory '/home/user/mylammps/src/Obj_serial'
Makefile.package.settings:4: ../../lib/plumed/Makefile.lammps: No such file or directory
Makefile.package.settings:5: ../../lib/nnp/src/libnnpif/Makefile.lammps.shared: No such file or directory
make[1]: *** No rule to make target '../../lib/nnp/src/libnnpif/Makefile.lammps.shared'.  Stop.
make[1]: Leaving directory '/home/user/mylammps/src/Obj_serial'
make[1]: Entering directory '/home/user/mylammps/src/Obj_serial'
Makefile.package.settings:4: ../../lib/plumed/Makefile.lammps: No such file or directory
Makefile.package.settings:5: ../../lib/nnp/src/libnnpif/Makefile.lammps.shared: No such file or directory
make[1]: *** No rule to make target '../../lib/nnp/src/libnnpif/Makefile.lammps.shared'.  Stop.
make[1]: Leaving directory '/home/user/mylammps/src/Obj_serial'
Makefile:201: recipe for target 'serial' failed
make: *** [serial] Error 2
```

The issue was an improper installation process with `make yes-user-plumed`. To remedy:

```
user@user-Vostro-3669:~/mylammps/src$ vim Makefile.package.settings
```
and change the file from:
```
# Makefile settings generated by libraries used by specific LAMMPS packages
# this file is auto-edited when those packages are included/excluded

include ../../lib/plumed/Makefile.lammps
include ../../lib/nnp/src/libnnpif/Makefile.lammps
```
to :
```
# Makefile settings generated by libraries used by specific LAMMPS packages
# this file is auto-edited when those packages are included/excluded

include ../../lib/plumed/Makefile.lammps.shared
include ../../lib/nnp/src/libnnpif/Makefile.lammps
```

## 10.11.2019
Ran the following lammps file
```
###############################################################################
# MD simulation for revPBE0-D3 H2O with NN potential
###############################################################################

###############################################################################
# VARIABLES
###############################################################################
clear
variable dt              equal  0.0005                                                   # timestep (ps)
variable intThermo       equal  1                                                       # screen output interval (timesteps)
variable numSteps        equal  4000000                                                    # total number of simulation timesteps (timesteps)
variable runnerCutoff    equal  6.3501269880                                            # largest symmetry function cutoff (Angstrom)
variable mass2           equal  15.9994                                                 # mass for element 1 (O)  (g/mol)
variable mass1           equal  1.0080                                                  # mass for element 2 (H) (g/mol)
variable nameStartCfg    string "data.iceih-big"                				# name of the starting configuration file
variable runnerDir       string "./"	                        # directory containing RuNNer files
# set initial velocity distribution
variable initTemp        equal  300                                                   # initial temperature (K)
variable velSeed         equal  4983009                                                 # seed for random number generator
# NVT integrator (Nose-Hoover)
variable startTemp       equal  300                                                   # starting temperature for thermostat (K)
variable stopTemp        equal  300                                                   # final temperature for thermostat (K)
variable tDamp           equal  0.02                                                    # thermostat damping factor (ps)
# dump thermodynamic properties (temperature, pressure, potential energy, kinetic energy, integrator quantity)
variable intTD           equal  10                                                      # thermodynamics dump file interval (timesteps)
variable nameTD          string "traj/td"                                               # thermodynamics file name
variable varVolume       equal  vol                                                     # variable required to print volume
variable varKE           equal  ke                                                      # variable required to print kinetic energy
###############################################################################
# SETUP
###############################################################################
units metal                                                                             # define unit system (metal = Angstrom, eV, ps, g/mol)
boundary p p p                                                                          # set periodic boundary conditions
atom_style atomic                                                                       # set atomic style for particles
read_data ${nameStartCfg}                                                               # read start configuration
mass 1 ${mass1}                                                                         # set mass for element 1
mass 2 ${mass2}          

#
change_box all x final 0.0 46.937 y final 0.0 44.121 remap
### SIMULATION STARTS HERE ###
change_box all z scale 1.00 remap

#replicate 1 1 2

neighbor       7.0 bin          # neighbor list skin width
neigh_modify   every 1 delay 0 check yes # frequency to update neighor list
# Write initial data file
#write_data ./init.data

# Regions and group
#region zcenter block 0 INF 0 INF $(lz/3) $(2*lz/3) side in units box
region zcenter block 0 INF 0 INF $(lz/3) $(2*lz/3) side in units box
group ice region zcenter
group water subtract all ice

pair_style nnp showew no showewsum 1000 resetew yes maxew 200 cflength 1.8897261328 cfenergy 0.0367493254
pair_coeff * * ${runnerCutoff}                                                          # set up pair style coefficients
timestep ${dt}                                                                          # set timestep
#velocity all create ${initTemp} ${velSeed}                                              # create initial velocities
#fix AVE_TD all ave/time ${intTD} 1 ${intTD} c_thermo_temp c_thermo_press v_varVolume c_thermo_pe v_varKE f_INT file ${nameTD} mode scalar       # set up thermodynamic properties output
thermo ${intThermo}                                                                     # set screen output
###############################################################################
# SIMULATION
###############################################################################

#dump            traj_xyz all custom 50 out.lammpstrj element x y z
#dump_modify     traj_xyz element H O sort id

# melt the ice first
#velocity        water create 600 2148459 rot yes mom yes dist gaussian # assign initial velocities to the particles
#fix             1 water npt temp 500.0 400.0 ${tDamp} z 1.0 1.0 10.0
#run             5000
#unfix           1

velocity        all create 200 2148459 rot yes mom yes dist gaussian
# equilibrate
fix             2 all nvt temp ${startTemp} ${stopTemp} ${tDamp}
run             100
unfix           2

#fix             2 water nvt temp ${startTemp} ${stopTemp} ${tDamp}
#run             100
#unfix           2

fix 1 all nvt temp ${startTemp} ${stopTemp} ${tDamp}
run            1000
unfix          1

#write_restart   restart.sl

fix             1 all npt temp ${startTemp} ${stopTemp} ${tDamp} z 1.0 1.0 10.0

fix             3 all plumed   plumedfile  plumed_bigdat_64.dat     outfile p.log

restart         10000  restart
dump            traj_xyz all custom 5000 out.lammpstrj element x y z
dump_modify     traj_xyz element H O sort id

run ${numSteps}
#fix 1 all ipi driver.01 32346 unix
#run 10000000
#
write_restart ${nameRestartEnd}                                                         # write final configuration (binary)
###############################################################################
# CLEAN UP
###############################################################################
#undump XYZ
#unfix AVE_TD
#unfix INT
###############################################################################
```
on lab PC and got the following [error log](https://github.com/fireballpoint1/summerResearch/blob/gh-pages/attachments/log1.md)

## 06.10.2019
- Found a way to fix plumed with lammps [here](https://lammps.sandia.gov/doc/fix_plumed.html).
- Facing error while installing lammps with nnp for some reason. 

## 22.10.2019
- Using the pre-patched lammps [here](https://github.com/plumed/conda#install-lammps)
- setup a conda env in `/home/mayank.modi/umbrellaSamplingOfWater/lammps_attempt1` on ada. 
- need to run the lammps code and tell sir about diwali plan.

## 21.10.2019
tried to patch plumed with lammps on ada - faced an issue with OMPI config with slurm on ada. 

## 18.10.2019
Trying to find a tutorial to run plumed with LAMMPS. Found the two following links :
- [plumed website](https://www.plumed.org/doc-v2.5/user-doc/html/lugano-6a.html)
- [from the lammps mailinglist](https://github.com/giacomofiorin)

## 17.10.2019

Finished seminar presentation yesterday. presented Ab-Initio thermodynamics of water and ice. 

### Code CVs as suggested by sir 
- for every hydrogen, find the closest H 
- find the longest such O-H bond (in the statement above ) using [plumed sort](https://www.plumed.org/doc-v2.5/user-doc/html/_s_o_r_t.html)

Written plumed conf file for the above conditions in `umbrellaSamplingOfWater/gromacs_files_2/plumed_more_logical.dat` and pushed to ada.

- Need to run everything with `gmx_mpi` on ada but currently getting the follwoing error : 

```
Command line:
  gmx_mpi grompp -f mdp/prd.mdp -o prd -pp prd -po prd -c eql2 -t eql2


Back Off! I just backed up prd.mdp to ./#prd.mdp.1#
Setting the LD random seed to 856336328

Back Off! I just backed up prd.top to ./#prd.top.3#
Generated 330891 of the 330891 non-bonded parameter combinations
Generating 1-4 interactions: fudge = 0.5
Generated 330891 of the 330891 1-4 parameter combinations
Excluding 2 bonded neighbours molecule type 'SOL'
turning H bonds into constraints...
Cleaning up constraints and constant bonded interactions with virtual sites
Removing all charge groups because cutoff-scheme=Verlet
Analysing residue names:
There are:   395      Water residues
Number of degrees of freedom in T-Coupling group System is 2367.00
Determining Verlet buffer for a tolerance of 0.005 kJ/mol/ps at 298.15 K
Calculated rlist for 1x1 atom pair-list as 1.038 nm, buffer size 0.038 nm
Set rlist, assuming 4x4 atom pair-list, to 1.001 nm, buffer size 0.001 nm
Note that mdrun will redetermine rlist based on the actual pair-list setup
Reading Coordinates, Velocities and Box size from old trajectory
Will read whole trajectory

-------------------------------------------------------
Program:     gmx grompp, version 2016.3
Source file: src/gromacs/fileio/checkpoint.cpp (line 800)

Fatal error:
Attempting to read a checkpoint file of version 17 with code of version 16

For more information and tips for troubleshooting, please check the GROMACS
website at http://www.gromacs.org/Documentation/Errors
-------------------------------------------------------
--------------------------------------------------------------------------
MPI_ABORT was invoked on rank 0 in communicator MPI_COMM_WORLD
with errorcode 1.

NOTE: invoking MPI_ABORT causes Open MPI to kill all MPI processes.
You may or may not see output from other processes, depending on
exactly when Open MPI kills them.
--------------------------------------------------------------------------
```


## 08.10.2019

- Trying to run a sampling on the files found [here](http://www.sklogwiki.org/SklogWiki/index.php/GROMACS_files_for_the_TIP4P/2005_model) - FAILED - Didn't know how to handle or convert `.g96` files.
-  **SUCCESS** Trying a run from [here](https://www.svedruziclab.com/tutorials/gromacs/1-tip4pew-water/)
    - Write the following `topol.top` file :
    ```
    #include "oplsaa.ff/forcefield.itp"
    #include "oplsaa.ff/tip4pew.itp"
    
    [ System ]
    TIP4PEW
    
    [ Molecules ]
    ```
    - `gmx solvate -cs tip4p -o conf.gro -box 2.3 2.3 2.3 -p topol.top`
    - When we ran gmx solvate, GROMACS added enough water molecules to fill a box 2.3 nm in each direction.
    - Downloaded mdp files for 2 minimizations, 2 equilibrations and 1 production run.
    - Ran minimizations and equilibrations 
    - Used the following `plumed.dat` file for umbrella sampling 
    ```
    d1: DISTANCES  GROUPA=1,5,9,13,17,21,25,29,33,37,41,45,49,53,57,61,65,69,73,77,81,85,89,93,97,101,105,109,113,117,121,125,129,133,137,141,145,149,153,157,161,165,169,173,177,181,185,189,193,197,201,205,209,213,217,221,225,229,233,237,241,245,249,253,257,261,265,269,273,277,281,285,289,293,297,301,305,309,313,317,321,325,329,333,337,341,345,349,353,357,361,365,369,373,377,381,385,389,393,397,401,405,409,413,417,421,425,429,433,437,441,445,449,453,457,461,465,469,473,477,481,485,489,493,497,501,505,509,513,517,521,525,529,533,537,541,545,549,553,557,561,565,569,573,577,581,585,589,593,597,601,605,609,613,617,621,625,629,633,637,641,645,649,653,657,661,665,669,673,677,681,685,689,693,697,701,705,709,713,717,721,725,729,733,737,741,745,749,753,757,761,765,769,773,777,781,785,789,793,797,801,805,809,813,817,821,825,829,833,837,841,845,849,853,857,861,865,869,873,877,881,885,889,893,897,901,905,909,913,917,921,925,929,933,937,941,945,949,953,957,961,965,969,973,977,981,985,989,993,997,1001,1005,1009,1013,1017,1021,1025,1029,1033,1037,1041,1045,1049,1053,1057,1061,1065,1069,1073,1077,1081,1085,1089,1093,1097,1101,1105,1109,1113,1117,1121,1125,1129,1133,1137,1141,1145,1149,1153,1157,1161,1165,1169,1173,1177,1181,1185,1189,1193,1197,1201,1205,1209,1213,1217,1221,1225,1229,1233,1237,1241,1245,1249,1253,1257,1261,1265,1269,1273,1277,1281,1285,1289,1293,1297,1301,1305,1309,1313,1317,1321,1325,1329,1333,1337,1341,1345,1349,1353,1357,1361,1365,1369,1373,1377,1381,1385,1389,1393,1397,1401,1405,1409,1413,1417,1421,1425,1429,1433,1437,1441,1445,1449,1453,1457,1461,1465,1469,1473,1477,1481,1485,1489,1493,1497,1501,1505,1509,1513,1517,1521,1525,1529,1533,1537,1541,1545,1549,1553,1557,1561,1565,1569,1573,1577  GROUPB=2,3,6,7,10,11,14,15,18,19,22,23,26,27,30,31,34,35,38,39,42,43,46,47,50,51,54,55,58,59,62,63,66,67,70,71,74,75,78,79,82,83,86,87,90,91,94,95,98,99,102,103,106,107,110,111,114,115,118,119,122,123,126,127,130,131,134,135,138,139,142,143,146,147,150,151,154,155,158,159,162,163,166,167,170,171,174,175,178,179,182,183,186,187,190,191,194,195,198,199,202,203,206,207,210,211,214,215,218,219,222,223,226,227,230,231,234,235,238,239,242,243,246,247,250,251,254,255,258,259,262,263,266,267,270,271,274,275,278,279,282,283,286,287,290,291,294,295,298,299,302,303,306,307,310,311,314,315,318,319,322,323,326,327,330,331,334,335,338,339,342,343,346,347,350,351,354,355,358,359,362,363,366,367,370,371,374,375,378,379,382,383,386,387,390,391,394,395,398,399,402,403,406,407,410,411,414,415,418,419,422,423,426,427,430,431,434,435,438,439,442,443,446,447,450,451,454,455,458,459,462,463,466,467,470,471,474,475,478,479,482,483,486,487,490,491,494,495,498,499,502,503,506,507,510,511,514,515,518,519,522,523,526,527,530,531,534,535,538,539,542,543,546,547,550,551,554,555,558,559,562,563,566,567,570,571,574,575,578,579,582,583,586,587,590,591,594,595,598,599,602,603,606,607,610,611,614,615,618,619,622,623,626,627,630,631,634,635,638,639,642,643,646,647,650,651,654,655,658,659,662,663,666,667,670,671,674,675,678,679,682,683,686,687,690,691,694,695,698,699,702,703,706,707,710,711,714,715,718,719,722,723,726,727,730,731,734,735,738,739,742,743,746,747,750,751,754,755,758,759,762,763,766,767,770,771,774,775,778,779,782,783,786,787,790,791,794,795,798,799,802,803,806,807,810,811,814,815,818,819,822,823,826,827,830,831,834,835,838,839,842,843,846,847,850,851,854,855,858,859,862,863,866,867,870,871,874,875,878,879,882,883,886,887,890,891,894,895,898,899,902,903,906,907,910,911,914,915,918,919,922,923,926,927,930,931,934,935,938,939,942,943,946,947,950,951,954,955,958,959,962,963,966,967,970,971,974,975,978,979,982,983,986,987,990,991,994,995,998,999,1002,1003,1006,1007,1010,1011,1014,1015,1018,1019,1022,1023,1026,1027,1030,1031,1034,1035,1038,1039,1042,1043,1046,1047,1050,1051,1054,1055,1058,1059,1062,1063,1066,1067,1070,1071,1074,1075,1078,1079,1082,1083,1086,1087,1090,1091,1094,1095,1098,1099,1102,1103,1106,1107,1110,1111,1114,1115,1118,1119,1122,1123,1126,1127,1130,1131,1134,1135,1138,1139,1142,1143,1146,1147,1150,1151,1154,1155,1158,1159,1162,1163,1166,1167,1170,1171,1174,1175,1178,1179,1182,1183,1186,1187,1190,1191,1194,1195,1198,1199,1202,1203,1206,1207,1210,1211,1214,1215,1218,1219,1222,1223,1226,1227,1230,1231,1234,1235,1238,1239,1242,1243,1246,1247,1250,1251,1254,1255,1258,1259,1262,1263,1266,1267,1270,1271,1274,1275,1278,1279,1282,1283,1286,1287,1290,1291,1294,1295,1298,1299,1302,1303,1306,1307,1310,1311,1314,1315,1318,1319,1322,1323,1326,1327,1330,1331,1334,1335,1338,1339,1342,1343,1346,1347,1350,1351,1354,1355,1358,1359,1362,1363,1366,1367,1370,1371,1374,1375,1378,1379,1382,1383,1386,1387,1390,1391,1394,1395,1398,1399,1402,1403,1406,1407,1410,1411,1414,1415,1418,1419,1422,1423,1426,1427,1430,1431,1434,1435,1438,1439,1442,1443,1446,1447,1450,1451,1454,1455,1458,1459,1462,1463,1466,1467,1470,1471,1474,1475,1478,1479,1482,1483,1486,1487,1490,1491,1494,1495,1498,1499,1502,1503,1506,1507,1510,1511,1514,1515,1518,1519,1522,1523,1526,1527,1530,1531,1534,1535,1538,1539,1542,1543,1546,1547,1550,1551,1554,1555,1558,1559,1562,1563,1566,1567,1570,1571,1574,1575,1578,1579 MAX={BETA=0.1}
    PRINT ARG=d1.max
    restraint-d1: RESTRAINT ARG=d1.max KAPPA=500 AT=-0.3

    PRINT STRIDE=10 ARG=d1.max,restraint-d1.bias FILE=COLVAR
    ```
    **NOTE** Need to name the groups as GROUPA and GROUPB for this file to run validly  
    
    - Ran 
    ```
    gmx grompp -f mdp/prd.mdp -o prd -pp prd -po prd -c eql2 -t eql2
    gmx mdrun -deffnm prd -plumed plumed.dat
    ``` 
    - Need to run wham and then calculate the free energies for the outputs 
    

## 05.10.2019

```
gmx pdb2gmx -f protein.pdb -ff charmm27 -water tip3p -ignh -o conf.pdb
```

## 03.10.2019
Haven't made notes in a while. Need to get back to doing it.

### Umbrella sampling progress
- Made a `.gro` file with 32 water molecules using the water file [here](link-somewhere-in-my documentation).
- Working on laptop in directoy `attempt-1` which contains an `mdp` file, `liquid_box.gro` file - need to generate topology file
- Found nothing with navya 
- Can I run plumed with lammps to perform Umbrella sampling so that I can use the NN potential for the calculations

## 23.09.2019

### Discussion with sir
Bahaut jhada. pH was supposed to be calulated as a thermodynamic quantity, but he confirmed that there is no known relationship of pH with potential energy. The approach to calcualte pH by cutoff distance is reasonable though. 

## 17.09.2019

### Discussion with Sir
Run Umbrella sampling for auto-ionization of water suing just one order parameter lambdaa

## 16.09.2019

### Fresh install patch LAMMPS with n2p2

```bash
cd ~
# Download lammps 
git clone -b stable https://github.com/lammps/lammps.git mylammps
# Download n2p2
git clone https://github.com/CompPhysVienna/n2p2.git

# Build n2p2
module load openmpi/3.0.0
cd ~/n2p2/src
make libnnpif-shared
export LD_LIBRARY_PATH=~/n2p2/lib:${LD_LIBRARY_PATH}

# Build LAMMPS
cd ~/mylammps/src
make mpi

# Get NNP data files
https://github.com/BingqingCheng/ab-initio-thermodynamics-of-water

# Setup to run interface-pinning file 
cd ~/ab-initio-thermodynamics-of-water/input-files/interface-pinning
mkdir nnp
cd ~/ab-initio-thermodynamics-of-water/NN-potential/
cp -r ./* ../input-files/interface-pinning/nnp/
cd ~/ab-initio-thermodynamics-of-water/input-files/interface-pinning/
vim in.lmp
#change CELLX and CELLY to xhi and yhi
# IMPORTANT STEP
# change variable runnerDir string "." to variable runnerDir string "nnp"
# This defines the directory which has all the parameters for nnp

# Patch LAMMPS with nnp
cd ~/mylammps/
ln -s ~/n2p2/ lib/nnp
cd ~/n2p2/
cp -r src/interface/LAMMPS/src/USER-NNP ~/mylammps/src/
cd ~/mylammps/src/
ls
make yes-user-nnp
make mpi

# Run interface-pinning script
cd ~/ab-initio-thermodynamics-of-water/input-files/interface-pinning/
~/mylammps/src/lmp_mpi < in.lmp

```

## 15.09.2019

### Implementing [Local initiation conditions for water autoionization](https://www.pnas.org/content/115/20/E4569)

Look [here](https://www.researchgate.net/figure/Scheme-of-the-RETIS-method-for-generating-trajectories-A-contour-plot-of-a-hypothetical_fig1_318736829) for brief explanation of RETIS

> Simulation Methods.
The MD simulations required by the RETIS algorithm (14) were performed with the Born–Oppenheimer MD capabilities of the CP2K program package (40). We used the Becke–Lee–Yang–Parr (BLYP) functional with a DZVP-MOLOPT (41) basis set and a plane-wave cutoff of 280 Ry. The BLYP functional gives a reasonable description of the structure and dynamics of liquid water (42, 43) and the absence of dispersion corrections (44) is likely of minor importance for ion–water interactions where the dominant interactions are mainly electrostatic. However, we note that the BLYP functional is known to give an overstructured description of liquid water with a low diffusion coefficient (45). Previous studies on the recombination mechanism for water (17, 46) and for weak bases in water (18) have, however, found that the collective compression of the hydrogen bond wire and the motion of the protons are reproduced with different choices of the functional and basis set.

> The initial system consisted of 32 water molecules placed in a cubic simulation box of 9.85×9.85×9.85 Å3. All MD simulations were carried out under constant energy (microcanonical) dynamics, with a time step of 0.5 fs and periodic boundaries.

> The transition region was divided into 20 path ensembles by positioning RETIS interfaces at λ= {1.07, 1.10, 1.13, 1.16, 1.19, 1.22, 1.25, 1.28, 1.31, 1.34, 1.39 1.43, 1.48, 1.52, 1.56, 1.80, 2.00, 2.50, 2.90, 3.29} Å. In addition, a final interface was placed at λ=∞ such that all trajectories were propagated until they reached the pure water state again. After generating an initial path for each path ensemble (this was done by repeatedly modifying the momenta of the particles and evolving the system forward in time until valid paths were obtained) the RETIS algorithm attempts to either swap paths between different path ensembles or generate new trajectories by the so-called shooting or the time-reversal move. In our simulations the probability of performing a swapping move was set to 50% while the probabilities of the two other moves were both set to 25%. New velocities for the shooting move were drawn from a Maxwell–Boltzmann distribution corresponding to an average temperature of 300 K.

> We performed 24,000 MC moves for each path ensemble, using the RETIS algorithm. This generated between 8,000 and 18,000 distinct trajectories in each path ensemble. The length of the trajectories ranged from 13.5 fs to 1,365 fs and we disregarded the first 400 trajectories in our analysis.

- [Umbrella sampling in plumed](https://www.plumed.org/doc-v2.5/user-doc/html/belfast-4.html)
- useless - [Understanding how to write a collective varibale in plumed](https://www.plumed.org/doc-v2.5/user-doc/html/julich-devel.html)

## 06.09.2019

### Discussion with sir
Use the same order parameter as [here ](https://www.ncbi.nlm.nih.gov/pubmed/29712836) and use the NNP to get the free energy - something umbrella sampling 


## 04.09.2019

Successfully run the LAMMPS interface for n2p2 fo rinterface-pinning at `~/ab-initio-thermodynamics-of-water/input-files/interface-pinning` 
### Steps Followed

```bash
# download lammps
cd ~
git clone -b stable https://github.com/lammps/lammps.git mylammps

# setup n2p2
cd ~/n2p2
cd src
make libnnpif-shared
export LD_LIBRARY_PATH=~/n2p2/lib:${LD_LIBRARY_PATH}  #check path to n2p2 here

# running lammps sim
cd ~/ab-initio-thermodynamics-of-water/input-files/interface-pinning
mkdir nnp
vim in.lmp # change CELLX and CELLY to the xhi and yhi in data file #save
cd ~/ab-initio-thermodynamics-of-water/NN-potential/
cp -r ./* ../input-files/interface-pinning/nnp/

~/mylammps/src/lmp_serial < in.lmp 
```
### Discussion with sir

- run a 1fs time step simulation of water at 300K for a system with 256 water molecules to observe protonation of water. 
- (my) need to remove repeat statement in `~/ab-initio-thermodynamics-of-water/input-files/mayank-water-sim` to keep it to the number of molecules in my data file
- downloaded a water pdb file- need to check what is it and use it in coupling with the commands to make argon system to get a data file for 256 water molecules
- set up the whole thing on ada and time it

## 03.09.2019

### Running LAMMPS scripts in ab-initio repo 
There are two input files in the repo at 
```
~/ab-initio-thermodynamics-of-water/input-files/interface-pinning
```
where `in.lmp` is the initial file which generates required files for `continue.lmp`. All these accomplish the task of **interface pinning** 

There is something wrong with installation of nepe repo itself. following are the modules to be installed in the static library:
```
nnp-comp2    nnp-cutoff   nnp-dist  nnp-predict  nnp-scaling  nnp-symfunc
nnp-convert  nnp-dataset  nnp-norm  nnp-prune    nnp-select   nnp-train
```

#### Interface Pinning 

Interface Pinning is a method for finding melting points from an MD simulation of a system where the liquid and the solid phase are in contact. To prevent melting or freezing at constant pressure and constant temperature, a bias potential applies a penalty energy for deviations from the desired two phase system. <sup>[ref](https://cms.mpi.univie.ac.at/vasp/vasp/Interface_pinning.html)</sup>

## 31.08.2019

### Running LAMMPS 
Found LAMMPS input file in 
```
~/ab-initio-thermodynamics-of-water/input-files/interface-pinning
```
- Follow the link [here]() for running the file. 
- Need to figure out what is `ttttt` in `initTemp` in file `in.lmp`. 

## 28.08.2019

### Difference in MD and ab-initio MD
Read [here](https://www.researchgate.net/post/What_is_the_difference_between_ab-initio_molecular_dynamics_and_simple_molecular_dynamics)
> In the standard MD approach, the instantaneous force on each atom is calculated as a gradient of a prescribed interatomic potential function

> ab-initio : From a quantum-mechanical perspective, the system at a fixed time can be parametrized in terms of the coordinates of the nuclei and the relevant electrons. By invoking the Born-Oppenheimer approximation, one can regard the nuclei fixed at the instantaneous positions of the atoms. This way, one can write a time-independent Schrödinger equation for the many-body wave function of the electrons. This Schrödinger equation is then solved using (time-independent) DFT to obtain the energy. The energy is then considered to be a function of the nuclear coordinates that were fixed earlier, and it can thus act as the interatomic potential that is needed to compute the forces in Newton's equation of motion for the nuclei. So, by computing the gradients of the DFT energy at this fixed point with respect to the nuclear coordinates, the forces are obtained and the nuclei are moved accordingly to get to the next time step. The DFT process is then repeated with these new nuclear coordinates.

## 27.08.2019

### Discussion with sir
- We need to apply newton's second law of motion on the ab-initio system. 
- Find pH for a water configuration 
- Wants me to read Leach to find definition of H-Bond
- Messaged at 8:45PM to read [Auto-Ionization of water](https://iopscience.iop.org/article/10.1088/0953-8984/14/50/202/meta)

### Find pH for a given water configuration 
- First approach in mind is to count number of H atoms farther than O-H bond length from any O
- Second approach in mind is to use some formula of potential . Some reading material on it [here](http://www.icsm.fr/Local/icsm/files/286/JFD_Chemical-potential.pdf) as pH = (µ<sup>0</sup><sub>H+</sub> − µ<sub>H+</sub> ) / RT ln10

Can't understand why no charge on atoms in the training configuration of ab-inition repository. 
- Reading Molecular Modelling by Leach 
> The most significant drawback of Hartree-Fock theory is that it fails to adequately represent electron correlation. In the self-consistent field method the electrons are assumed to be moving in an average potential of the other electrons, and so the instantaneous position of an electron is not influenced by the presence of a neighbouring electron. In fact, the motions of electrons are correlated and they tend to 'avoid' each other more than Hartree-Fock theory would suggest, giving rise to a lower energy. The correlation energy is defined as the difference between the Hartree-Fock energy and the exact
energy. Neglecting electron correlation can lead to some clearly anomalous results,especially as the dissociation limit is approached. For example, an uncorrelated calculation would predict that the electrons in H2 spend equal time on both nuclei, even when they areadvanced ab initio Methods 111 infinitely separated. Hartree-Fock geometries and relative energies for equilibrium structures are often in good agreement with experiment and as many molecular modelling
applications are concerned with species at equilibrium it might be considered that correlation effects are not so important. Nevertheless, there is increasing evidence that the inclusion of correlation effects is warranted, especially when quantitative information is required. Moreover, electron correlation is crucial in the study of dispersive effects
(which we shall consider in Section 4.10.1), which play a major role in intermolecular interactions. Electron correlation is most frequently discussed in the context of ab initio calculations, but it should be noted that the effects of electron correlation are implicitly included in the semi-empirical methods because of the way in which they are parametrised. However, specific electron correlation methods have also been developed for use with the various levels of semi-empirical calculation; this in turn necessitates the modification of some parameters.

- Siddhartha says that QM calculation don't have charges because we can't specify electron positions. 
- Sir wants me to read Leach to find definition of H-Bond.

## 26.08.2019
Using [n2p2](https://github.com/CompPhysVienna/n2p2) to replicate the results of [ab-initio thermodynamics of water](https://github.com/fireballpoint1/ab-initio-thermodynamics-of-water). Currently, onyl using the `nnp-predict` library of `n2p2` in shared bin

## 22.08.2019
- There seems to be a difference in how the `TrainingAndTestingEnergyPlot.ipynb` is reading output and slurm files. Look into it. 

### Discussion with sir
The box size for 100 atom argon simulation should be `1.6 nm` not `11.28 nm` as I have been using, since the bigger box size would mean the system is acting as a gas and there are weaker interactions in place. 

### Running simulation with smaller box size
`rlist` which is the short range cutoff for simulation is 2.5 * sigma usually and less than half the box size. This means that I need to change my `rlist, rcoulomb, rvdw` in `md.mdp` such that `rlist >= rcoulomb, rvdw`. Read about it [here](https://www.researchgate.net/post/How_to_decide_the_cut-off_distance_for_the_Lennard-Jones_potential_in_Molecular_simulation_dynamics)

## 21.08.2019

### Train on faster symmetry function calculation

Got a varying testing output : 
![graph of testing energy for output66.txt from memoized branch](https://github.com/fireballpoint1/summerResearch/blob/memoized-symmetry-functions/simulation/argon/python_scripts/mayank/images/TestingEnergyComparison66-1.png)

### Make symmetry function calculation faster

`memoized-symmetry-functions` branch : 

| head        | file          | performace |
|:-------------|:------------------|:------|
| Fastest:contains code written by prabhakar sir| `z_symmetryFucntions.py` | `(100atoms-10frames-68s)` | 
| Original: | `z_symmetryFucntions0.py`  | `(100atoms-10frames-295s)`   |
| Incomplete memoization:contains code with incomplete memoization        | `z_symmetryFucntions1.py`       | -   |



**Comparing `symmetry63.pkl` which is from `z_symmetryFucntions.py` against `symmetry64.pkl` which is from `z_symmetryFucntions0.py`**

```
>>> prab['frame_energy'] == maya['frame_energy']
array([ True,  True,  True,  True,  True,  True,  True,  True,  True,
        True])
```

```
>>> print(prab['G'] == maya['G'])
[[[ True False]
  [ True False]
  [ True False]
  ...

  [ True False]
  [ True False]
  [ True False]]

 ...

 [[ True False]
  [ True False]
  [ True False]
  ...
  
  [ True False]
  [ True False]
  [ True False]]
```

### Reading [ANI](https://pubs.rsc.org/en/content/articlelanding/2017/sc/c6sc05720a#!divAbstract)

> In an ANI potential, only a single η is used to produce thin Gaussian peaks and multiple Rs are used to probe outward from the atomic center. The reasoning behind this specific use of parameters is two-fold: firstly, when probing with many small η parameters, vector elements can grow to very large values, which are detrimental to the training of NNPs. Secondly, using Rs in this manner allows the probing of very specific regions of the radial environment, which helps with transferability. 

They have made some changes to the angular symmetry function 
![ANI-1 ](https://pubs.rsc.org/image/article/2017/sc/c6sc05720a/c6sc05720a-t5.gif)

Does this statement apply to the original symmetry function too which doesn't has a θs
> Also, ζ changes the width of the peaks in the angular environment

The paper gives no explicit numbers for η, Rc, Rs, lambdaa or zeta but all we have is the numbers in this graph,which doesn't specify the molecule for which the graph is made. 
![ani-1 graph](https://pubs.rsc.org/image/article/2017/sc/c6sc05720a/c6sc05720a-f2.gif)


 
### Discussion with sir
- met at 9:30 am to discuss this in his office 

The neural network structure in `parameter-structure` was wrong and doesn't make much sense since all the other values are constant. Hence, what I really need to do is have a vector of length x, which contains 
```
[g11,g12,g12.... , g21,g22,g23...]
```
where `g11` and `g21` are the symmetry functions corresponding to a particular set of parameters. This maintains more information in the input. 


## 20.08.2019
Use this command to get a node while ada is clogged with jobs 
```
sinteractive -c 10 -p short -A sub -g 1 -w gnode01
```


## 19.08.2019
`parameter-input` branch has Rc,Rs,eeta1,eeta2,lambdaa,zeta as an input to the neural network. The parameters are added to each array as 
```
[g1,g2,Rc,Rs,eeta1,eeta2,lambdaa,zeta].
```
This is to provide more info to the NN.
### Job 1246 is running with this for 3*(300+20) frames with 10 epochs. 
- **SCALE IS SET** TRUE for this job
- Look what scale does to this 
- This changes how the symmetry pickles are stored
- The output files have an edited name to look out for above while plotting graphs 
- Can we somehow send these inputs directly into the NN while training and testing and not have to save and transfer it with every symmetry function array ?
#### Results and output files
![Results](https://github.com/fireballpoint1/summerResearch/blob/parameter-input/simulation/argon/python_scripts/mayank/images/TestingPredictionVsOriginal-6-2.png)  
- [Link to output file](https://github.com/fireballpoint1/summerResearch/blob/parameter-input/simulation/argon/python_scripts/mayank/output-parameter-input6.txt).
- [Link to symmetry file](https://github.com/fireballpoint1/summerResearch/blob/parameter-input/simulation/argon/python_scripts/mayank/symmtery-parameter-input6.pkl).
- [Link to saved model](https://github.com/fireballpoint1/summerResearch/blob/parameter-input/simulation/argon/python_scripts/mayank/model-parameter-input6.pth).
- [Link to slurm file](https://github.com/fireballpoint1/summerResearch/blob/parameter-input/simulation/argon/python_scripts/mayank/slurm-1246.out).

### Job 1250 is running with 3*(600+20) points in a 2x40x20x2x1 network. 
- **Scale is set false** for this job 
#### Results and output files
![Results](https://github.githubassets.com/images/icons/emoji/octocat.png)
- [Link to output file](./add).
- [Link to symmetry file](./add).
- [Link to saved model](./add).
- [Link to slurm file](./add).

# 16.08.2019
- G2 has double calculation of all triplets and it's ok but can be optimized later to save computation 
- memoized-symmetry-function branch used to create a 5 atom simulation and sniff out all the bugs. 
- try the suggested change in model here to improve speed: <a href="https://stackoverflow.com/a/57495421/6224662"> here </a>

#### Running models
- slurm-285089 - model shape (2x10x2x1) - Standard Scalar - 3x(1000+100) frames 
- slurm-285088 - model shape (2x10x2x1) - No Scale - 3x(500+50) frames 
- slurm-285087 - model shape (2x10x2x1) - Standard Scalar - 3x(500+50) frames 
- slurm-285077 - model shape (2x10x2x1) - Manual Scale - 3x(500+50) frames 
- slurm-285154 - model shape (2x40x2x1) - Standard Scalar - 3x(1000+100) frames 


