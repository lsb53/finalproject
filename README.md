# Phylogenetic Biology - Final Project

# Using time-structured data to estimate the evolutionary rate of Frog Virus 3 – a simulation study

## Background and Goals
  Ranaviruses (genus = *Ranavirus*) are a group of globally distributed double-stranded DNA viruses capable of infecting many species of ectothermic vertebrates such as amphibians, reptiles, and fishes (Gray and Chinchar 2015). Ranviruses have been implicated in mass die-off events, and indeed, much of our understanding of ranaviruses come from studies of these die-off events. Because of this, the bulk of reserach on ranavirus has been reactionary and near-sighted, and as a result, we lack a longitudinal perspective of ranavirus dynamics. In my reviews of the literature, the longest studies of ranavirus dynamics I have identified have lasted no more than 5 years and are focused on a limited number of field sites (Olori et al. 2018; O'Connor et al. 2016). This limits our ability to parameterize epidemiological models and determine whether a ranavirus-induced die-off event is a call for concern or a natural part of a system’s ecology. To address this gap in the literature, I am interested in analyzing long-term trends in ranavirus infection dynamics in wood frog larvae at the Yale-Myers Forest (YMF). Wood frog larva are among the most susceptible species to ranavirus, and in previous field studies, ranavirus has been detected at high prevalence at YMF. The Skelly lab has 20 years of demographic data from ~60 wood frog breeding sites, ethanol-preserved larvae samples collected opportunistically from these sites over the past 20 years. These samples can assessed for ranavirus infection using standard molecular methods. **While my initial interests focused on the effects of ranavirus on host population dynamics, this also presents a potential opportunity to study pathogen evolution through time.**

  Because I have no data of my own to address this question and no data exist to directly address this question, my project will be an exploratory simulation. Specifically, my goal is to explore the viability and limits of using archived samples to estimate Frog Virus 3 (FV3) nucleotide substitution rate. This analysis is modified from a study by Firth et al. (2010) that used time-structured data available in GenBank to estimate evolutionary rates of double-stranded DNA viruses. After using their dsDNA virus datasets to estimate evolutionary rates of seven viruses, they then attempted to validate their estimates. They did this by creating simulation data sets and assessing their ability to recover the nucleotide substitution rates of these sequences with a known evolutionary history across a range of scenarios (ex., differing root heights, substitution rates, nucleotide sequence length, number of samples, strict vs relaxed clock).

  For my project, I will only be conducting the simulation portion instead of first estimating substitution rates from real data. This is meant to be an explorative project that helps to inform a sampling potential sampling regime from museum specimens farther down the road. Ultimately, I hope that this will give me a clearer idea of if this will be a worthwhile research avenue once I begin processing preserved specimens collected over a 20-year span.

## Methods and data
I used BEAST software v1.10.4 (Suchard et al. 2018) for all analyses.

First, I downloaded a `.xml` file for use with BEAST provided in the supplement to Firth et al. (2010) to use as template for my project files. The basic components of this file are:
  1) a taxa list with taxa names that each are assigned a collection date (year)
  2) A chunk of code to create a random tree from a coalescent process using an MCMC process, assuming constant population size, and with root height fixed. The tip dates are clamped based on the time-of-sampling of each sequence.
  3) A chunk of code to set up the sequence simulation using the Beagle simulator function in BEAST. This includes a substitution model, a clock rate, nucleotide frequencies, and transition/transversion ratios, and a nucleotide sequence length.
  4) A chunk of code that sets up posterior inference on the simulated data set assuming a strict molecular clock, HKY model, and constant population size. This generates a `.fasta` file, `.tre` file, `.log` file, a `.ops` file, and a `.trees` file.
  
I set up my `.xml` files to explore a range of scenarios across a fixed sampling time range: 2001 - 2020 (20 yr). I ran the analysis with two separate texa sets to explore the sensitivity of analysis to sample size. The small taxa set consisted of one sample from each year from 2001 - 2020 (n = 20). The large taxaset consisted of five samples frome each year from 2001 - 2020 (n = 100). For each taxa set, I simulated nucleotide sequence sets of two different sizes: a small nucleotide sequence set typical of a singe-gene alignment (1,200 bp), and a large nucleotide sequence set typcal of a full-genome alignment (100,000 bp). For each taxa set, I generated random starting phylogenies with a constant population size and two different root heights: 100 and 1000 ybp.

For simplicity, I used the same substitution model that Firth et al. (2010) used to simulate the human dsDNA virus data, HKY. I used base frequencies and transition/transversion ratios for FV3 based on empirical data (Candido et al. 2019). I simulated the data with three different substitution rates using a strict molecular clock: 1E-4 subs/site/year, 1E-6 subs/site/year, and 1E-8 subs/site/year. These rates represent what might be considered a fast, intermediate, and slow substitution rate, respectively, for a dsDNA virus (Duffy et al. 2008).

All things considered, this resulted in a total of 24 test combinations (2 taxa sets X 2 nucleotide sequence size sets X 2 root heights X 3 substitution rates). I replicated each combination 5 times, resulting in a total of 120 MCMC analyses of simulated data. Simulations were run for 10 million generations, with subsampling every 1,000 generations. These files can be found in this [folder (20 taxa)](https://github.com/lsb53/finalproject/tree/master/Ranavirus%20simulations/20%20taxa) and this [folder (100 taxa)](https://github.com/lsb53/finalproject/tree/master/Ranavirus%20simulations/100%20taxa).

After running the simulations, I used Tracer software v1.7.1 (Rambaut et al. 2018) to summarize the parameter estimates from the analysis by uploading the `.log` files. There was a 10% burn in for each file. I then extracted estimated clock rates ± 95% HPD and evaluated mixing and convergence. This was one of the more labor-intensive parts of the project, as I could not figure out a way to efficiently extract the clock rates from each file and organize them into a `.xlsx` file for analysis. Because of this, I ended up opening the five replicate files for each test combination and copy/pasting the estimated clock rates ± 95% HPD into a `.xlsx` file.

After entering the estimated clock rates ± 95% HPD into a `.xlsx` [file (linked here)](https://github.com/lsb53/finalproject/blob/master/Ranavirus%20simulations/BEAST_SimulationOutputs.xlsx), I used R software v4.0.2 (R Core Team 2020) to visualize the data (*ggplot* function, *ggplot2* package; Wickham 2016). The `.rmd` file can be found [here](https://github.com/lsb53/finalproject/blob/master/Ranavirus%20simulations/BEAST_SimulationOutputs_Plots.Rmd). I made four plots, each with *Simulated substitution rate (subs/site/year)* on the x-axis and *Estimated substitution rate (log(subs/site/year))* on the y-axis. The four plots correspond to the four possible `taxa set X nucleotide sequence length` combinations:
  1) 20 taxa + 1,200 bp nucleotide sequences
  2) 20 taxa + 100,000 bp nucleotide sequences
  3) 100 taxa + 1,200 bp nucleotide sequences
  4) 100 taxa + 100,000 bp nucleotide sequences

Each plot contains two panels, one for each root height (100 and 1000 ybp). These plots were used to qualitatively assess the ability of posterior inference to accurately estimate the simulated substitution rate.

## Results
When sequences were generated under a strict molecular clock with 20 taxa and 1200bp nucleotide sequences, posterior inference was able to recover the correct nucleotide substitution rates with narrow 95% HPDs for all five replicates when the simulated substitution rate was 1.0E-4 subs/site/year and the root height was set at 100 ybp. However, when the root height was 1000 ybp, the mean substitution rates were less accurate and two of the five estimates had long-tailed posterior distributions tending toward zero (Fig. 1). For both root heights at 1.0E-6 and 1.0E-8 subs/site/year, the mean estimated substitution rates were much higher than the true value with long-tailed posterior distributions tending toward zero.

<p>
  <img src="/Ranavirus%20simulations/Plots/20taxa_1200bp.png" width="660" height="480" title="20 taxa + 1200bp plot">
    <br>
    <em>Fig. 1: Substitution rates (posterior mean and 95% HPD) estimated from synthetic data sets based on a data set of 20 taxa with 1200bp nucleotide sequences. Five replicates were generated for each simulated substitution rate undedr a strict molecular clock evolving at 1.0E-4, 1.0E-6, or 1.0E-8 substitutions/site/year. The colored dashed lines show correspond to the three simulated substitution rates and can be used as a point of reference for the estimated rates of the same color.</em>
</p>


When the number of nucleotides per taxa is increased from 1,200 to 100,000bp, posterior inference is able to more accurate recover the nucleotide substitution rate for simulations using a rate of 1.0E-6 subs/site/year. However, the estimates in the 1000ybp root height scenario are less precise, with four of five estimates having long-tailed posterior distributions tending toward zero (Fig. 2).


<p>
  <img src="/Ranavirus%20simulations/Plots/20taxa_100000bp.png" width="660" height="480" title="20 taxa + 100000bp plot">
    <br>
    <em>Fig. 2: Substitution rates (posterior mean and 95% HPD) estimated from synthetic data sets based on a data set of 20 taxa with 100000bp nucleotide sequences.</em>
</p>

When sequences are generated under a strict molecular clock with 100 taxa and 1200bp nucleotide sequences, the posterior estimates are qualitatively similar to those from the simulation with 20 taxa and 1200bp sequences (Fig. 3). The main difference is that the 95% HDPs for the simulated substitution rate of 1.0E-4 subs/site/year for the 100 taxa simulation are tighter than those from the 20 taxa simulation. As before, for both root heights at 1.0E-6 and 1.0E-8 subs/site/year, the mean estimated substitution rates were much higher than the true value with long-tailed posterior distributions tending toward zero.

<p>
  <img src="/Ranavirus%20simulations/Plots/100taxa_1200bp.png" width="660" height="480" title="100 taxa + 1200bp plot">
    <br>
    <em>Fig. 3: Substitution rates (posterior mean and 95% HPD) estimated from synthetic data sets based on a data set of 100 taxa with 1200bp nucleotide sequences.</em>
</p>

When sequences are generated under a strict molecular clock with 100 taxa and 100,000bp nucleotide sequences, posterior inference was able to recover accurate nucleotide substitution rates with narrow 95% HPDs for all five replicates when the simulated substitution rate was 1.0E-4 or 1.0E-4 subs/site/year, regardless of root height (Fig. 4). Although closer to the true value than in any of the other simulations, for the simulations with 1.0E-8 subs/site/year the mean estimated substitution rates were higher than the true value with long-tailed posterior distributions tending toward zero.

<p>
  <img src="/Ranavirus%20simulations/Plots/100taxa_100000bp.png" width="660" height="480" title="100 taxa + 100000bp plot">
    <br>
    <em>Fig. 4: Substitution rates (posterior mean and 95% HPD) estimated from synthetic data sets based on a data set of 100 taxa with 100000bp nucleotide sequences.</em>
</p>

## Discussion

The results of this study highlight several important takeaways. First, increasing the number of taxa (20 vs 100) and nucleotide sites (1200 vs 100,000) both enhance the ability to accurately estimate substitution rates. However, it seems that the analysis is more strongly limited by the length of the nucleotide sequences. Even after increasing the number of taxa from 20 to 100, the analysis was unable to accurately estimate rates slower than 1.0E-4 subs/site/year with only 1200bp per taxa. Second, based on the results from this study, the lower range of dsDNA substitution rates reported in Firth et al. (2010) for human dsDNA viruses (1.0E-6) may be accurately estimated from approximately 20 time-structured taxa with whole-genome sequences. While this is likely an underestimation, as simulation data is much cleaner than real data, it does suggest that estimating substitution rates may be feasible with the available samples. Third, tree root height (100 ybp vs 1000 ybp) doesn’t significantly impact estimate accuracy. Although there were slight qualitative differences between the 100 and 100 ybp data sets, the effect was relatively small. Firth et al. (2010) reported that it be more difficult to accurately estimate substitution rates when the sampling interval (20 yrs) is small relative to time frame of sequence evolution (100 vs 1000 ybp). However, the way in which the authors of that study used tree root height differed in that they adjusted substitution rate and root heights in unison so that the genetic diversity of each dataset was held constant but the changes occurred over a different time span (e.g., 1.0E-4 subs/site/year when the root height was 100 ybp, 1.0E-5 subs/site/year when the root height was 1000 ybp, etc.). Fourth, even in best simulation situation (100 taxa, 100,000 bp), temporal signal breaks down at 1.0E-8 subs/site/year. This is likely because at a certain rate, the sampling interval is too small relative to the simulated rate of evolution. Thus, there are likely limits to this method, even with a large number of samples with large nucleotide sequences.

The biggest difficulty in implementing these analyses was determining what parameters to vary and what parameters to keep constant. For example, I only did my analyses with the HKY model for simplicity, but actual data may be better fit by a different model. This could change the outcome of the simulation. I also chose to only simulate data with a strict molecular clock, which is likely a poor reflection of a true molecular clock with variable rates. 

If I did these analyses again, I would increase the number of MCMC generations from 10 million to 100 million, and have a higher burn in percentage. This may have led to more precise estimates, but would have taken much more time to run on my personal computer.

## References
Duffy, S., L. A. Shackelton, and E. C. Holmes. 2008. Rates of evolutionary change in viruses: Patterns and determinants. Nature Reviews Genetics 9:267–276.

Firth, C., A. Kitchen, B. Shapiro, M. A. Suchard, E. C. Holmes, and A. Rambaut. 2010. Using time-structured data to estimate evolutionary rates of double-stranded DNA viruses. Molecular Biology and Evolution 27:2038–2051.

Gray, M.J., Chinchar, G. V., 2015. Ranaviruses: lethal pathogens of ectothermic vertebrates. Choice Rev. Online 53, 53-1272-53–1272. https://doi.org/10.5860/choice.192841

Olori, J., Netzband, R., McKean, N., Lowery, J., Parsons, K., Windstam, S., 2018. Multi-year dynamics of ranavirus, chytridiomycosis, and co-infections in a temperate host assemblage of amphibians. Dis. Aquat. Organ. 130, 187–197. https://doi.org/10.3354/dao03260

O’Connor, K.M., Rittenhouse, T.A.G., Brunner, J.L., 2016. Ranavirus is Common in Wood Frog (Lithobates sylvaticus) Tadpoles throughout Connecticut , USA. Herpetol. Rev. 47, 394–397.

Jariani, A., Warth, C., Deforche, K., Libin, P., Drummond, A.J., Rambaut, A., Matsen, F.A., Theys, K., 2019. SANTA-SIM: Simulating viral sequence evolution dynamics under selection and recombination. Virus Evol. 5. https://doi.org/10.1093/ve/vez003

R Core Team (2020). R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.

Suchard MA, Lemey P, Baele G, Ayres DL, Drummond AJ & Rambaut A (2018) Bayesian phylogenetic and phylodynamic data integration using BEAST 1.10 Virus Evolution 4, vey016. DOI:10.1093/ve/vey016

Wickham, H., 2016. ggplot2: Elegant Graphics for Data Analysis. Springer-Verlag New York.
