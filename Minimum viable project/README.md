# Minimum Viable Project

## Getting started
This folder contains the materials necessary for running a toy analysis that simulates a set of temporally sampled Herpes Simplex 1 (HSV-1) sequences, which are then used to test the ability of heterochronous phylogenetic modeling to correctly recover the nucleotide substitution rate used to simulate the data. 

This analysis is modified from a study by Firth et al. (2010) that used time-structured data available in GenBank to estimate evolutionary rates of double-stranded DNA viruses. After using their dsDNA virus datasets to estimate evolutionary rates of seven viruses, they then attempted to validate their estimates. They did this by creating simulation data sets and assessing their ability to recover the nucleotide substitution rates of these sequences with a known evolutionary history across a range of scenarios (ex., differing root heights, substitution rates, nucleotide sequence length, number of samples, strict vs relaxed clock).

For my project, I will only be conducting the simulation portion instead of first estimating substitution rates from real data. This is meant to be an explorative project that helps to inform a sampling potential sampling regime from museum specimens farther down the road.

## Files in this folder
This folder contains the following files:
- a `.xml` file [HSV1_SubstitutionRate_1E-4.xml](https://github.com/lsb53/finalproject/blob/master/Minimum%20viable%20project/HSV1_SubstitutionRate_1E-4.xml) to be used in BEAST software v1.10.4 (Suchard et al. 2018). This XML file runs the entire analysis and contains:

  - a taxa list with 84 taxa names that each are assigned a date (year).
  
  - A chunk of code to create a random tree from a coalescent process using an MCMC process, assuming constant population size, and with root height fixed (100 ybp in this file). The tip dates are clamped based on the time-of-sampling of each sequence.
  
  - A chunk of code to set up the sequence simulation using the Beagle simulator function in BEAST. In this case, it is a HKY model with a strict clock rate (1E-4 subs/site/yr), nucleotide frequences based on the actual data sets used by Firth et al. (2010) (0.18, 0.35, 0.29, 0.18), and kappa (transition/transversion ratios) based on the actual data sets (kappa = 4.91). This creates a 1200 bp nucleotide sequence for each taxa based on the previously generated random tree and saves it as a .fasta file `filename`.
  
  - A chunk of code that sets up posterior inference on the simulated data set assuming a strict molecular clock, HKY model, and constant population size. This generates a `.log` file, a `.ops` file, and a `.trees` file (all located in [this folder](https://github.com/lsb53/finalproject/tree/master/Minimum%20viable%20project/BEAST%20simulation%20outputs)).

## Running the analysis with BEAST
Open `BEAST`. Use the `Choose File...` option to select the `.xml` file. Then press `Run`.

## Analyzing the BEAST output
Use `Tracer` to summarize the parameter estimates from the analysis by uploading the `.log` file. This is where you will find the clock rate (`clock.rate` statistic) that was estimated from the simulation data, as well as other statistics such as root height (`treeModel.rootHeight`). 

You can build the MCC tree from the`.trees` file with `TreeAnnotator`. Set the burn-in as 10% of the total number of iterations used in the analysis (here, 1,000,000), set the Input Tree File as the `.trees` file with the `Choose File...` function, and create a name and save location for the output file (MCC tree) using the `Choose File` function.

You can view the generated MCC tree using `Figtree`.

## How this will be adapted for my project  
I will use the `.xml` file here as the framework for my project. 

First, I will substitute out the HSV-1 taxa with a list of taxa with sampling years relevant to my study sytem (range: 2001 - 2020). I will run the analysis with **two separate taxa sets**. The small taxa set will consist of one taxa from each year from 2001-2020 (n=20). The large taxa set will consist of five taxa from each year from 2001-2020 (n=100). For each taxa set, I will simulate **nucleotide sequence sets of two different sizes**: a small nucleotide sequence set typical of a singe-gene alignment(1,200 bp), and a large nucleotide sequence set typcal of a full-genome alignment (100,000 bp).

For each taxa set, I will generate random phylogenies with a constant population size and **two different root heights**: 100 and 1000 ybp. 

For simplicity, I plan to use the same substitution model that Firth et al. (2010) used to simulate the human dsDNA virus data, `HKY`. I will use base frequencies and transition/transversion ratios for FV3 based on empirical data. I will simulate the data with **three different substitution rates** using a strict molecular clock: 1E-4 subs/site/year, 1E-6 subs/site/year, and 1E-8 subs/site/year. These rates represent what might be considered a fast, intermediate, and slow substitution rate, respectively, for a dsDNA virus (Duffy et al. 2008).

All things considered, this will result in a total of 24 test combinations (2 taxa sets X 2 nucleotide sequence size sets X 2 root heights X 3 substitution rates). I will replicate each combination 5 times, resulting in a **total of 120 MCMC analyses of simulated data**.


### References
Duffy, S., L. A. Shackelton, and E. C. Holmes. 2008. Rates of evolutionary change in viruses: Patterns and determinants. Nature Reviews Genetics 9:267–276.

Firth, C., A. Kitchen, B. Shapiro, M. A. Suchard, E. C. Holmes, and A. Rambaut. 2010. Using time-structured data to estimate evolutionary rates of double-stranded DNA viruses.   Molecular Biology and Evolution 27:2038–2051.

Suchard MA, Lemey P, Baele G, Ayres DL, Drummond AJ & Rambaut A (2018) Bayesian phylogenetic and phylodynamic data integration using BEAST 1.10 Virus Evolution 4, vey016. DOI:10.1093/ve/vey016
