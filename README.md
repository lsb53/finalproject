# Phylogenetic Biology - Final Project

## Guidelines - you can delete this section before submission

This repository is a stub for your final project. Fork it, develop your project, and submit it as a pull request. Edit/ delete the text in this readme as needed.

Some guidelines and tips:

- Use the stubs below to write up your final project. Alternatively, if you would like the writeup to be an executable document (with [knitr](http://yihui.name/knitr/), [jupytr](http://jupyter.org/), or other tools), you can create it as a separate file and put a link to it here in the readme.

- For information on formatting text files with markdown, see https://guides.github.com/features/mastering-markdown/ . You can use markdown to include images in this document by linking to files in the repository, eg `![GitHub Logo](/images/logo.png)`.

- The project must be entirely reproducible. In addition to the results, the repository must include all the data (or links to data) and code needed to reproduce the results.

- If you are working with unpublished data that you would prefer not to publicly share at this time, please contact me to discuss options. In most cases, the data can be anonymized in a way that putting them in a public repo does not compromise your other goals.

- Paste references (including urls) into the reference section, and cite them with the general format (Smith at al. 2003).

- Commit and push often as you work.

OK, here we go.

# Using time-structured data to estimate the evolutionary rate of Frog Virus 3 – a simulation study

## Background and Goals
  Ranaviruses (genus = *Ranavirus*) are a group of globally distributed double-stranded DNA viruses capable of infecting many species of ectothermic vertebrates such as amphibians, reptiles, and fishes (Gray and Chinchar 2015). Ranviruses have been implicated in mass die-off events, and indeed, much of our understanding of ranaviruses come from studies of these die-off events. Because of this, the bulk of reserach on ranavirus has been reactionary and near-sighted, and as a result, we lack a longitudinal perspective of ranavirus dynamics. In my reviews of the literature, the longest studies of ranavirus dynamics I have identified have lasted no more than 5 years and are focused on a limited number of field sites (Olori et al. 2018; O'Connor et al. 2016). This limits our ability to parameterize epidemiological models and determine whether a ranavirus-induced die-off event is a call for concern or a natural part of a system’s ecology. To address this gap in the literature, I am interested in analyzing long-term trends in ranavirus infection dynamics in wood frog larvae at the Yale-Myers Forest (YMF). Wood frog larva are among the most susceptible species to ranavirus, and in previous field studies, ranavirus has been detected at high prevalence at YMF. The Skelly lab has 20 years of demographic data from ~60 wood frog breeding sites, ethanol-preserved larvae samples collected opportunistically from these sites over the past 20 years. These samples can assessed for ranavirus infection using standard molecular methods. **While my initial interests focused on the effects of ranavirus on host population dynamics, this also presents a potential opportunity to study pathogen evolution through time.**

  Because I have no data of my own to address this question and no data exist to directly address this question, my project will be an exploratory simulation. Specifically, my goal is to explore the viability and limits of using archived samples to estimate Frog Virus 3 (FV3) nucleotide substitution rate. This analysis is modified from a study by Firth et al. (2010) that used time-structured data available in GenBank to estimate evolutionary rates of double-stranded DNA viruses. After using their dsDNA virus datasets to estimate evolutionary rates of seven viruses, they then attempted to validate their estimates. They did this by creating simulation data sets and assessing their ability to recover the nucleotide substitution rates of these sequences with a known evolutionary history across a range of scenarios (ex., differing root heights, substitution rates, nucleotide sequence length, number of samples, strict vs relaxed clock).

  For my project, I will only be conducting the simulation portion instead of first estimating substitution rates from real data. This is meant to be an explorative project that helps to inform a sampling potential sampling regime from museum specimens farther down the road. Ultimately, I hope that this will give me a clearer idea of if this will be a worthwhile research avenue once I begin processing preserved specimens collected over a 20-year span.

## Methods and data
I used BEAST software v1.10.4 (Suchard et al. 2018) for all analyses.

First, I downloaded a `.xml` file provided in the supplement to Firth et al. (2010) to use as template for my project files. The basic components of this file are:
  1) a taxa list with taxa names that each are assigned a collection date (year)
  2) A chunk of code to create a random tree from a coalescent process using an MCMC process, assuming constant population size, and with root height fixed. The tip dates are clamped based on the time-of-sampling of each sequence.
  3) A chunk of code to set up the sequence simulation using the Beagle simulator function in BEAST. This includes a substitution model, a clock rate, nucleotide frequencies, and transition/transversion ratios, and a nucleotide sequence length.
  4) A chunk of code that sets up posterior inference on the simulated data set assuming a strict molecular clock, HKY model, and constant population size. This generates a .fasta file, .tre file, .log file, a .ops file, and a .trees file (all located in this folder).
 
## Methods

The tools I used were... See analysis files at (links to analysis files).

## Results

The tree in Figure 1...

## Discussion

These results indicate...

The biggest difficulty in implementing these analyses was...

If I did these analyses again, I would...

## References

Gray, M.J., Chinchar, G. V., 2015. Ranaviruses: lethal pathogens of ectothermic vertebrates. Choice Rev. Online 53, 53-1272-53–1272. https://doi.org/10.5860/choice.192841

Olori, J., Netzband, R., McKean, N., Lowery, J., Parsons, K., Windstam, S., 2018. Multi-year dynamics of ranavirus, chytridiomycosis, and co-infections in a temperate host assemblage of amphibians. Dis. Aquat. Organ. 130, 187–197. https://doi.org/10.3354/dao03260

O’Connor, K.M., Rittenhouse, T.A.G., Brunner, J.L., 2016. Ranavirus is Common in Wood Frog (Lithobates sylvaticus) Tadpoles throughout Connecticut , USA. Herpetol. Rev. 47, 394–397.

Jariani, A., Warth, C., Deforche, K., Libin, P., Drummond, A.J., Rambaut, A., Matsen, F.A., Theys, K., 2019. SANTA-SIM: Simulating viral sequence evolution dynamics under selection and recombination. Virus Evol. 5. https://doi.org/10.1093/ve/vez003
