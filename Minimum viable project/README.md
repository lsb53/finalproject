# Minimum Viable Project

## Getting started
This folder contains the materials necessary for running a toy analysis that simulates a set of temporally sampled Herpes Simplex 1 (HSV-1) sequences, which are then used to test the ability of heterochronous phylogenetic modeling to correctly recover the nucleotide substitution rate used to simulate the data. 

This analysis is modified from a study by Firth et al. (2010) that used time-structured data available in GenBank to estimate evolutionary rates of double-stranded DNA viruses. After using their dsDNA virus datasets to estimate evolutionary rates of seven viruses, they then attempted to validate their estimates. They did this by creating simulation data sets and assessing their ability to recover the nucleotide substitution rates of these sequences with a known evolutionary history across a range of scenarios (ex., differing root heights, substitution rates, nucleotide sequence length, number of samples, strict vs relaxed clock).

## Files in this folder
This folder contains the following files:
- a .xml file `filename` to be used in BEAST software (citation). This XML file runs the entire analysis and contains:

  - a taxa list with 84 taxa names that each are assigned a date (year).
  
  - A chunk of code to create a random tree from a coalescent process using an MCMC process, assuming constant population size, and with root height fixed (100 ybp in this file).    The tip dates are clamped based on the time-of-sampling of each sequence.
  
  -A chunk of code to set up the sequence simulation using the Beagle simulator function in BEAST. In this case, it is a HKY model with a strict clock rate (1E-4 subs/site/yr),      nucleotide frequences based on the actual data sets used by Firth et al. (2010) (0.18, 0.35, 0.29, 0.18), and kappa (transition/transversion ratios) based on the actual data     sets (kappa = 4.91). This creates a 1200 bp nucleotide sequence for each taxa based on the previously generated random tree and saves it as a .fasta file `filename`.
  
  -A chunk of code that sets up posterior inference on the simulated data set assuming a strict molecular clock, HKY model, and constant population size. This generates a .log       file `filename`, a .ops file `filename`, and a .trees file `filename`.

For my project, I will only be conducting the simulation 

Because my focus virus, Frog Virus 3 (FV3) is also a dsDNA virus.
