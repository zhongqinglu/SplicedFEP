# SplicedFEP
## Overview
`SplicedFEP` is a versatile Free Energy Perturbation (FEP) strategy designed to calculate free energy changes resulting from residue modifications, including substitutions (even self-substitutions), insertions, and deletions. This approach rationally constructs hybrid structures and topologies for FEP calculations by emulating the biological splicing mechanism.

## Dependencies
- Python (some common Python libraries)
- [MODELLER](https://salilab.org/modeller/)
- [MDTraj](https://mdtraj.org/1.9.7/index.html)
- [GROMACS](http://www.gromacs.org/) (required MPI version)
- [alchemical_analysis](https://github.com/MobleyLab/alchemical-analysis)

## Installation
```bash
git clone https://github.com/zhongqinglu/SplicedFEP.git
```

## Usage
#### Prepare for FEP
Add the source path of SplicedFEP to the environment, i.e. the root path of this file.
```bash
SRCPATH=`pwd`
export PATH=$SRCPATH:$PATH
```
Check necessities for FEP.
```bash
splicedfep.prepare -h
```
Make sure that GROMACS has been installed, and the command should be "gmx".
```bash
which gmx
```
Change current path to the workplace.
```bash
cd $WORKPATH
```
Make a new directory "SEQUENCE.notes" for the original sequence to be modified. (e.g. KQWLVWLFL.Q2A)
```bash
mkdir KQWLVWLFL.Q2A
cd KQWLVWLFL.Q2A
```
Place the PDB file of the original structure in this directory, name it as "KQWLVWLFL.pdb".
```bash
touch KQWLVWLFL.pdb
```
Or, test using an example file.
```bash
cp $SRCPATH/example/1_hhat/KQWLVWLFL.pdb .
```
Try a simple example for FEP preparations.
```bash
splicedfep.prepare -l Q2A -q
```
More advanced examples in [$SRCPATH/example/](), or contact with developers.

#### Run FEP
Make sure that MPI version of GROMACS has been installed, and the command should be "gmx_mpi".
```bash
which gmx_mpi
```
Check parameters for FEP runs.
```bash
splicedfep.mpirun -h
```
Run in the background.
```bash
nohup splicedfep.mpirun -g 4 -p 16 -t 4 > splicedfep.log &
```

#### Analyze free energy changes
INSTALL alchemical_analysis.
```bash
conda create -n pymbar python=2.7 matplotlib
conda activate pymbar
conda install -c conda-forge pymbar
pip install pathlib2
git clone https://github.com/MobleyLab/alchemical-analysis.git
cd alchemical-analysis
pip install -e .
vim alchemical_analysis/alchemical_analysis.py
  #line 334: O = MBAR.computeOverlap()['matrix']
```
Calculate free energy changes.
```bash
cd $WORKPATH
conda activate pymbar
calcmbar
conda deactivate
```
Report dG results.
```bash
cd $WORKPATH
calcddg -s KQWLVWLFL
```
or
```bash
cd KQWLVWLFL.Q2A
calcddg
```

## Contact
Temporarily, these scripts are specifically designed for HLA-I-restricted epitopes involving residue modifications within HLA-epitope-TCR systems. However, `SplicedFEP` has the potential to be extended to any peptide modifications. If you have any peptide modification needs or further questions, please contact us at [zhongqinglu@foxmail.com]().

