# Lysozyme in water
Let us start with the simplest system (also very well documented by Justin Lemkul)
**Lysozyme in water**
To avoid the hassle of downloading the files and clean those I have included all the necessary ones in the folder Lysozyme.
So, let's retrieve the folder in our laptop and start playing with it, to do that we can just run wget in the terminal:
1. If you don't have already installed we can use the same procedure as for installing GROMACS:
- **Linux**
```sudo apt-get install wget```
- **MacOS**
```brew install wget```
2. Use **wget** to download the files:
```wget https://github.com/jadgt/GROMACS-in-your-laptop/blob/main/Tutorial%20Example/Lysozyme.zip```

## Preparing the files

The first thing to do will be to get a topology for our protein. Why? 
1. The PDB file is just a geometry file.
2. We need a [Force Field](https://en.wikipedia.org/wiki/Force_field_(chemistry)) that contains the necessary information to compute the forces, potentials, energies and trajectories. In other words, whithout the topology there is no molecular dynamics, just a beautiful representation.

### How do I generate a topology?
1. Automatically 
GROMACS includes an extensive database for aminoacids, making the process of buiding a topology file for a protein (an ensemble of aminoacids) a piece of cake.
2. Manually 
Worth another entire tutorial (or two...)

**Let's go for it**
Once you have your geometry file ready (PDB) it's time to make GROMACS do its job.
To call GROMACS from terminal is as simple as typing `gmx`. You can give it a try so we check that the program is correctly installed.

Once we master calling GROMACS we can call it for something worth, like building the topology. Just type this:
```gmx pdb2gmx -f 1aki -o 1aki_proc -water spce ```

Brief explanation to the syntax:
- `gmx`: calling the program 
- `pdb2gmx` : function of the program 
- `-f` : input file
- `-o`: output file
- `-water` : which water model will you be using? `spce` indicates that we will be using [SPC/E](https://pubs.acs.org/doi/10.1021/j100308a038)
Do you wanna know more? Then you, curious mind, can always type `gmx -h` for the global help or `gmx pdb2gmx -h` for help about the specific function.

Now we are ready to pick a Force Field:
```
Select the Force Field:

From '/opt/homebrew/bin/../Cellar/gromacs/2022/share/gromacs/top':

 1: AMBER03 protein, nucleic AMBER94 (Duan et al., J. Comp. Chem. 24, 1999-2012, 2003)
 2: AMBER94 force field (Cornell et al., JACS 117, 5179-5197, 1995)
 3: AMBER96 protein, nucleic AMBER94 (Kollman et al., Acc. Chem. Res. 29, 461-469, 1996)
 4: AMBER99 protein, nucleic AMBER94 (Wang et al., J. Comp. Chem. 21, 1049-1074, 2000)
 5: AMBER99SB protein, nucleic AMBER94 (Hornak et al., Proteins 65, 712-725, 2006)
 6: AMBER99SB-ILDN protein, nucleic AMBER94 (Lindorff-Larsen et al., Proteins 78, 1950-58, 2010)
 7: AMBERGS force field (Garcia & Sanbonmatsu, PNAS 99, 2782-2787, 2002)
 8: CHARMM27 all-atom force field (CHARM22 plus CMAP for proteins)
 9: GROMOS96 43a1 force field
10: GROMOS96 43a2 force field (improved alkane dihedrals)
11: GROMOS96 45a3 force field (Schuler JCC 2001 22 1205)
12: GROMOS96 53a5 force field (JCC 2004 vol 25 pag 1656)
13: GROMOS96 53a6 force field (JCC 2004 vol 25 pag 1656)
14: GROMOS96 54a7 force field (Eur. Biophys. J. (2011), 40,, 843-856, DOI: 10.1007/s00249-011-0700-9)
15: OPLS-AA/L all-atom force field (2001 aminoacid dihedrals)
```
We will pick OPLS-AA, therefore we type 15, and done.

We will receive back:
1. A new geometry file: `1aki_proc.gro`
2. A topology file: `topol.top`
3. A position restrain file: `posre.itp` .This file is useful if for some reason you want to keep your protein static.

## Building a periodic box
Molecular dynamics simulations uses periodic boundary conditions to conserve the mass without creating a preassure cooker.
So, the next command to type is: `gmx editconf -f 1aki_proc -o box -c -d 0.5 -bt triclinic`

And, so we have a nice box there 

## Solvate the box
Like plants, proteins tend to need an aqueous environment if you want to have some realistic cell-like simulation
Type: `gmx solvate -cp box -cs spc216 -p topol -o solv`
Now you'll see a new residue in the topology called SOL, that's the water

## Adding ions
The treatment of the non bonded interactions with the Particle Mesh Ewald (PME) algorithm does not like net charges in the system.
Type: ```gmx grompp -f ions -c solv -p topol -o ions 
         gmx genion -s ions -p topol -neutral -pname NA -nname CL -o solv_ions```

## Energy minimization
Let's find a local minimum in the conformational space:
Type: ```gmx grompp -f em -c solv_ions -p topol -o em
         gmx mdrun -v -deffnm em```

## Equilibrations
To have an stable simulation we need to get an equilibrated system:
- **NVT Equilibration**
Type: ```gmx grompp -f nvt -c em -p topol -o nvt
         gmx mdrun -v -deffnm nvt```
- **NPT Equilibration**
Type: ```gmx grompp -f npt -c nvt -p topol -o npt
         gmx mdrun -v -deffnm npt```

## Actual MD Simulation
Type: ```gmx grompp -f md -c npt -p topol -o md
         gmx mdrun -v -deffnm md``` 



