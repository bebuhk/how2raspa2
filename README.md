# RASPA2 tutorial: how to calculate interaction energy of solid at gridpoint

This is an explainatory example of how to calculate the LJ and coulombic interaction of a single molecule at a certain position(/gridpoint in the cDFT framework). To illustrate RASPAs calculation procedure, we define a potential molecule XX consisting of a single atom XXX that has a charge of + 0.42 and Lennard Jones parameters sigma=3.21 and epsilon/k_b=77.77 and demonstrate how RASPA calculates the LJ and the Coulombic interaction with an adsorbent defined in a .cif file. For the adsorbent structure we take a single Carbon (C) atom at the origin of a large orthogonal unit cell as a simple, illustrative example. 

## 1. start from scratch

if you use this for the first time you can delete all subfolders, just keep the following 10 files:
- run
- simulation.input

- XX.def # defines the molecule. RASPA will look for the string MoleculeName from simulation.input (here 'XX') with the file ending '.def'
- pseudo_atoms.def # here RASPA will look for the strings of the atoms defined in the MoleculeName.def file, which are listed with an index under "# atomic positions" (here the molecule XX consists of only one atom 'XXX') in the #type collumn and consider the charge (here 0.42) for the calculation of the Coulombic intercation. make sure the #number of pseudo atoms is equal the # of lines/pseudoatoms defined. to define real molecules consider also RASPA_DIR/share/raspa/forcefield/ExampleMoleculeForceField/pseudo_atoms.def as a reference.
- force_field_mixing_rules.def # gives the LJ parameter for all atoms in the structure (.cif file) and the components/adsorbents (here atom XXX of molecule XX). this file takes UFF parameters for the 103 atoms of the periodic table + one parameter set defined for out atom XXX as sigma=3.21, eps=77.77 for the illustrative purpose of this tutorial. this file also defines the mixing rules for the LJ-parameters, if the LJ-potential is shifted or truncated and if tail correction should be calculated or not. 
- example_structure_1_atom.cif # this defines the structure of the calculation. RASPA will look for the string of FrameworkName in the simulation.input file (here 'example_structure_1_atom') with the file ending '.cif'. from this file RASPA reads the unic cell and the atoms with their positions and partial charges (here one C atom at origin with partial charge= 0.42)

- force_field.def # to surpress warning "'force_field.def' file not found and therefore not used" (optional)
- README.me # this file, not relevant for RASPA
- verification.ipynb # not relevant for RASPA, intended to verify if results are plausible
- how2raspa2.code-workspace # not relevant for RASPA, intended to convieniently open this folder in VSC (optional)
- run_auto # not needed for the manual start from scratch procedure, but here to au

- (maybe dont delete the .git folder ;)
1. once you have only these files in a folder, you can start the calculation with executing "bash run" from a terminal (in this folder). [RASPA has to be installed in the RASPA_DIR defined in the bash script "run"]. make sure that in simulation.input the "CreateNumberOfMolecules  1" line is not commented out to make sure a single XX molecule is created and the "RestartFile yes" is commented out.

2. after executing this code for the first time, RASPA will create the Folders CrashRestart, Movies, VTK, Output, and Restart. For now we only need the the output file under Restart/System0/ where we can see the (random) position of our single molecule (at the bottom). To start the simulation again from a chosen position, we copy that file to a new folder RestartInitial/System0/ (which we create manually or alternatively run "cp -r Restart RestartInitial/") and modify the position of the molecule to our needs (here we put it to the xyz-position [4.56, 0, 0], i.e. 4.56 Å distance to the single framework atom (which is located at the origin as defined in the example_structure_1_atom.cif file)). To do so we change the line 48 to "Adsorbate-atom-position: 0 0    4.56 0.0 0.0". Now we can reverse the 2 steps taken under 1. in the simulation.input file (include the "RestartFile yes" line to start the simulation with the molecule at the defined position and comment out/delete the line "CreateNumberOfMolecules  1" to not create a second molecule). then, the calculation can be (re-)started with "bash run" as before.

3. now we can check the results for the LJ-interaction in the simulations output file under Output/System0/. the "Host/Adsorbate energy" can be seen in line 689 and is splitted into LJ (i.e. VDW energy) and Coulomb (which again is split into the real and the fourier part). To check if these results make sense we can check what values we expect with the jupyter notebook verification.ipynb. For the distance of 4.56 Å we get a LJ interaction of -32.53 K and a Coulombic interaction of 11589.97. The LJ energy can be verified to whole precision. The coulombic interaction of the single pair can be calculated with the CoulombTruncated method to be 5694.63 K. 

The coulombic energy calculated with the ewald sum in RASPA2 is higher than the single pair interaction value as the Ewald method takes into accounth the interaction with the periodic repetition of the unitcell too (its therefore higher as all repetitions are also positive charges, therefore adding positively)


## 2. run auto

just run
'''
bash run_auto
'''

