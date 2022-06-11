---
zotero: ["PhD"]
bibliography: book.bib
---

# Variation in the frequency of trait-associated alleles across global human populations {#Fst-chap}

Here we illustrate the distribution of FST scores for loci associated with 587 traits, a subset of the GWAS Catalog which passed our criteria for suitable polygenic traits (Methods). Our analysis is empirical, in that we do not explicitly formulate a statistical test for drift or selection, differential or otherwise. Using high-coverage sequence data for 2,504 individuals from the 1000 Genomes Project phase 3 release, for each trait in the GWAS Catalog we calculated the distribution of FST across all approximately-unlinked SNPs associated with it (trait SNPs), and compared these FST distributions with the FST distributions of random-selected SNPs that were matched to the trait SNPs by their allele frequencies in European populations (control SNPs). Our results show that traits related to the physical correlates of “race” (such as skin-pigmentation, eye colour, and hair shape) tend to have relatively high FST values – signifying relatively high variance in allele frequencies between populations – whereas traits related to intelligence (such as self-reported EA, mathematical ability, and cognitive function measurement) tend to have lower FST values that are similar to those of most polygenic traits such as height and body mass index.

## Datasets

### 1000 Genomes

As the reference for human genomic variation across diverse populations, we used the New York Genome Center high-coverage, phased .vcf files[@IndexVol1Ftp] for the 2,504 individuals described in the 1000 Genomes phase 3 release.[@10002015global] We then annotated those .vcf files with human SNP IDs from dbSNP release 9606.[@smigielskiDbSNPDatabaseSingle2000]

### GWAS Catalog

We used the R package *gwasrapidd*[@magnoGwasrapiddPackageQuery2020] to query all traits in the GWAS Catalog[@macarthurNewNHGRIEBICatalog2017] as of 9 August 2021 ($N_{TRAITS}$ = 3,459). For 541 of these traits, no matching variant IDs could be pulled out from the 1000 Genomes VCFs, leaving $N_{TRAITS}$ = 3,008. 

### Linkage disequilibrium

To obtain our "trait SNP" dataset, for each trait, we sought to isolate the SNP closest to each of its true causal variants, and exclude the SNPs in linkage disequilibrium (LD) with them. To this end, we used PLINK 1.9[@changSecondgenerationPLINKRising2015; @purcellPLINK] to "clump" the SNPs associated with each of the remaining 3,008 traits, using an index variant p-value threshold of $10^{-8}$,[@panagiotouWhatShouldGenomewide2012] $r^2$ threshold of 0.1,[@hillLinkageDisequilibriumFinite1968] and base window size of 1 Mb. This process left us with 2,045 traits with at least one index SNP that met the p-value threshold. The index SNPs for each trait formed our set of trait SNPs. In order to target relatively polygenic traits, we further filtered out traits with fewer than 10 trait SNPs, leaving $N_{TRAITS}$ = 587. Supplementary Fig. 4 shows the counts of unique SNP IDs associated with each trait before and after clumping, and an interactive version is available in the notebook cited above. 

### Control SNPs

To obtain our "control SNP" dataset, we assigned each trait SNP to one of 20 bins based on its minor allele frequency in European populations (as provided in the original 1000 Genomes .vcf files under the column header ‘INFO/AC_EUR’). For example, if a trait SNP had a minor allele frequency of 0.08 in European populations, it was assigned to the $(0.05, 0.1]$ bin. We did the same for all (unassociated) SNPs in the .vcf files, then paired each trait SNP with a random SNP from the .vcf file in the equivalent bin. These allele-frequency-paired random SNPs formed our set of "control SNPs", which we used to infer the $F_{ST}$ distribution of a random set of SNPs with the same allele frequencies as the trait SNPs, and against which we could compare the $F_{ST}$ distribution of the trait SNPs.

### $F_{ST}$ and ranking traits by signed Kolmogorov-Smirnov $D$ statistic

We then calculated $F_{ST}$ for each of the trait SNPs and their matched control SNPs using the Weir and Cockerham method,[@weirEstimatingFStatisticsAnalysis1984] as implemented in the R package *pegas*.[@paradisPegasPackagePopulation2010] To rank all traits based on the directional difference in $F_{ST}$ distributions between trait and control SNPs, we ran three Kolmogorov-Smirnov (KS) tests for each trait $t$ with $x_t$ = $F_{ST, trait SNPs}$ and $y_t$ = $F_{ST, control SNPs}$:

1. two-sided ($D_t$) ; 

1. one-sided "greater" ($D_t^+$) ; and 

1. one-sided "less" ($D_t^-$).

I note that \[D_{t} = max(D_t^+, D_t^-)\], where $D_t^+$ is the greatest vertical distance attained by the eCDF of $x_t$ over the eCDF of $y_t$, and $D_t^-$ is the greatest vertical distance attained by the eCDF of $y_t$ over the eCDF of $x_t$.[@conoverPracticalNonparametricStatistics1999; @durbinDistributionTheoryTests1973] Accordingly, we used a comparison of $D_t^+$ and $D_t^-$ to created a signed D statistic ($D$), based on the logic that trait SNPs with a lower overall $F_{ST}$ than control SNPs tend to have a higher $D$ under the "greater" test than the "less" test, and vice versa. 

Therefore, ${D_t^S}$:

$\begin{aligned}
D_t^- > D_t^+ &: &-D_t \\
D_t^- = D_t^+ &: &0 \\
D_t^- < D_t^+ &: &D_t \\
\end{aligned}$

In **Figure \@ref(fig:FstMain)** we present the $F_{ST}$ distributions of trait SNPs for an illustrative subset of 28 human traits, ranked by the signed Kolmogorov-Smirnov test statistic D when compared with their matched control SNPs. **Figure \@ref(fig:FstMain)A** shows the densities of SNPs as a function of $F_{ST}$, and **Figure \@ref(fig:FstMain)B and C** show their empirical Cumulative Distribution Functions (eCDFs). **Figure \@ref(fig:FstMain)B** includes the eCDFs of control SNPs in grey. eCDF figures for all 587 traits that passed our filters (Methods) are provided in Supplementary Fig. 1.

(ref:FstMain) Distributions of FST across 28 illustrative human traits, ranked by signed-D (Kolmogorov-Smirnov test) comparing trait and control SNPs. **A**. FST density ridge plots with SNP markers. **B**. Empirical Cumulative Distribution Functions (eCDFs) of FST for trait-associated (colour) and random control (grey) SNPs, faceted by trait. **C**. Consolidated eCDFs of trait-associated SNPs from (B). eCDFs for all traits are included in Supplementary Figure 1.  

\begin{figure}
\includegraphics[width=1\linewidth]{figs/fst/20220314_final_0.1_1000} \caption{(ref:FstMain)}(\#fig:FstMain)
\end{figure}
