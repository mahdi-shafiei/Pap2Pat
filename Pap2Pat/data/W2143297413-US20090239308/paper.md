# Introduction

## Digital Pcr And Digital Array

Digital PCR conventionally utilizes sequential limiting dilutions of target DNA, followed by amplification using the polymerase chain reaction (PCR) [1], [2]. As a result, it is possible to quantitate single DNA target molecules. We utilize the digital array, which is a novel nanofluidic biochip [2], [3] where digital PCR reactions can be performed (Figure 1) by partitioning DNA molecules, instead of diluting them. This chip utilizes integrated channels and valves that partition mixtures of sample and reagents into 765 nanolitre volume reaction chambers. DNA molecules in each mixture are randomly partitioned into the 765 chambers of each panel (the total volume of the PCR mix in each panel: 6 nl×765 = 4.59 µl). The chip is then thermocycled and imaged on Fluidigm's BioMark real-time PCR system and the positive chambers that originally contained 1 or more molecules can be counted by the digital array analysis software (Figure 2).

## Copy Number Variation

Copy number variations (CNVs) are the gains or losses of genomic regions which range from 500 bases on upwards in size. Whole genome studies have revealed the presence of large numbers of CNV regions in human and a broad range of genetic diversity among the general population [4], [5], [6]. CNVs have been the focus of many recent studies because of their roles in human genetic disorders [7], [8], [9].

Current whole-genome scanning technologies use array-based platforms (array-CGH and high-density SNP microarrays) to study CNVs. They are high throughput but lack resolution and sensitivity. Real-time PCR is a sequence-specific technique which is easy to perform, but is limited in its discriminating power beyond a 2-fold difference [11], [12].

CNV determination on the digital array is based upon its ability to partition DNA sequences. Given the number of molecules per panel and the dilution factor, the concentration of the target sequence in a DNA sample can be accurately calculated. In a multiplex PCR reaction with 2 or more assays, multiple genes can be quantitated simultaneously and independently, effectively eliminating any pipetting errors if separate reactions have to be set up for different genes. When a single copy reference gene (RNase P in this study, [10]) is used in the reaction, the ratio of the target gene to the reference gene would reflect the copy number per haploid genome of the target gene.

## Primary Contribution Of This Paper

In this paper we will show that the digital array provides a robust and easy-to-use platform to study CNVs. We have derived a mathematical framework to calculate the true concentration of molecules from the observed positive reactions in a panel. We also show how to perform statistical analysis to find the 95% confidence intervals of the true concentrations and the ratio of two concentrations in a CNV experiment using the digital array with multiplex PCR.

The copy number variation problem can be stated as follows. Given two counts h 1 and h 2 of positive chambers for two genes in a digital array panel, how can one estimate a ratio of true concentrations r = λ 1 /λ 2 of the two genes and a confidence interval [rLow, rHigh] on the estimation?

Our approach is built on well-known tools and techniques from statistics. It decomposes the problem into two parts.

It turns out that the first question can be answered by applying sampling and estimation theories from statistics and probability, and the second question can be answered by a numerical algorithm based on generalization of a mathematical theorem.

For related work on answering the first question, using Bayesian approach, see unpublished preprint by Warren et al. titled “The Digital Array Response Curve” dated March 2007 at http://thebigone.stanford.edu/papers.htm. Warren et al. assumed a uniform probability distribution of number of molecules, with maximum number assumed to be 4000, and using Bayesian and combinatorial methods, presented a solution. The confidence interval obtained using Bayesian probabilistic framework, is often referred to as credible interval or Bayesian confidence interval which requires one to incorporate problem-specific contextual information from the prior distribution.

This paper differs from this prior work by Warren et al. in two different ways. First, we consider the parameter λ to be a fixed constant, unlike having a probability distribution as in Bayesian approach. Second, in addition to providing the answer to the first question, we are interested in estimating the confidence interval of the ratio of two concentrations which is new work. For difference between credible interval and confidence interval, see [13]. Both approaches give good results depending upon the question one is trying to answer.

We will prove mathematical correctness of our results in this paper and present simulation results to help the reader build useful insight. Finally, we present actual CNV experiments on the digital array with known ratios and show the results using the techniques developed in this paper.

# Results

DNA quantitation in the digital array is based on the partitioning of a PCR reaction into an array of several hundreds or even few thousands of chambers or wells. One panel of the digital array consists of 765 chambers and one can use up to 12 panels at a time. If the concentration of the target molecules is low in the DNA sample, most of the chambers capture either one or no molecules and the number of positive chambers at the end point of the PCR yields close approximation to the true concentration of the target. However, if the number of molecules is large, then there is greater probability of several molecules being in the same chamber, and therefore the number of positive chambers would be significantly lower compared with the number of molecules in the chambers.

We are interested in estimating the true concentration of the molecules in the DNA sample from which we extracted 6 nl×765 = 4.59 µl of sample for each panel.

Consider the universe of infinite number of the digital array chambers filled with an infinite amount of the DNA sample where the true concentration of the target molecules is λ per chamber (per 6 nl). The true concentration is an unknown population parameter of this infinite DNA sample. If a chamber gets no molecule then it constitutes failure in the sense of Bernoulli experiment. If it gets one or more molecules, that is, if it gets a “hit” and is therefore positive, then it constitutes success. Let the probability of success be p. Note that p is an unknown population parameter. We will use the standard hat notation to denote sample estimators of population parameters. For example, pˆ and  will denote the estimators of p and λ, respectively.

## Relationship Between P And Λ

One can model K, the number of molecules in each chamber as a Poisson process, and this gives the relationship between p and λ as followsAlternatively, consider M molecules randomly distributed in C chambers. The probability of any given molecule being in any given chamber is . So the probability p of a given chamber having at least one molecule isSince M = λC, we haveAs number of chambers becomes arbitrarily large, the above approaches e −λ. Therefore,which establishes the relationship between λ and p.

## Confidence Intervals For Estimation Of P And Λ

A chamber getting a hit or no hit is a binomial process, same as toss of a coin, with success probability p. Let the number of positive chambers in the panel be H. Consider  as an estimator of p. It is well known that pˆ is an unbiased estimator of p and has expectation p and standard deviation  and its sampling distribution f(pˆ) is approximately normal for large C. See Figure 3 for illustration of the above ideas.

See [13], [14], [15] for extensive literature on obtaining confidence interval for the estimation of binomial probability. It is referred to as binomial sign test when the test statistic can be approximated with the chi-square distribution, specifically through the use of the chi-square goodness-of-fit. An alternative and equivalent approximation is obtained by using the normal distribution and then the test is referred to as the population proportion test, see [15].

If C is large enough, then the confidence limits are approximately given byFor 95% confidence interval, zc = 1.96. For the digital array, C is an integral multiple of 765 and is comfortably large enough for the above approximation.

Define the estimator of λ asSince the probabilities in any given differential area of a probability density function are preserved under change of variables, the 95% confidence interval [, ] is directly given as follows See Figure 4 for illustration.

## Expectation Of Estimation Of Λ

Let a random variable X have probability density function fX(x). If h(x) is either increasing or decreasing in x, then U = h(X) has density function given bywhich follows from the fact that probabilities in any given differential area have to be invariant under change of variables, see [16]. Furthermore,which can be expanded using Taylor series expansion of h(x) around the mean ξ = E(X), as followsSince in our case, we have the followingtherefore, in above, we have x = pˆ,  and h(x) = −ln(1−x). Since  is a monotonically increasing function of pˆ, one can get the sampling distribution of  from the sampling distribution of pˆ asNote that due to nonlinear relationship between  and pˆ, one can not make assumptions about g. In general, g is not normal and .

Now we derive an approximation for  from the Taylor series expansion shown above. Higher order central moments of Gaussian function f(pˆ) with mean p areFor proof see [17]. Since f(pˆ) has very small σ due to very large number of chambers, the higher order terms for all n>0 in the Taylor expansion are small,and therefore the only contributing term is when n = 0, which impliesAs C→∞,  approaches the true concentration λ of molecules.

## Simulation Results On Estimation Of P

It is informative and useful to run a simulation experiment on the computer to see how the real-world matches with the theory developed above. For this purpose, one can use a random number generator and a computer program to simulate the universe of the digital array chambers.

If a panel has C chambers, consider a universe of C×K many chambers where K is a large number chosen for simulation. Choose some value of λ as the true concentration of molecules in one chamber. Therefore, in total, there will be λ×C×K molecules. Assign each of these molecules randomly to one of the chambers.

Extract K panels out of this universe and for each of the panels, compute  as an estimator of p and plot its histogram over all the K panels. The mean should be p = 1−e −λ and standard deviation should be . For each of these panels, estimate λ and compute the 95% confidence interval. In 95% of the K panels, the true value of λ should lie within the confidence interval.

For our simulation experiments we chose M = 400, that is, . We chose K = 70000. In Figure 5 we show the histogram of H which is really same as distribution of P scaled by a factor of 765. In Table 1 we show how the predicted values match with the actual simulation values. In the same way, the sampling distribution of number of molecules matched with what is predicted by theory. Though the results of the simulation follow from elementary probability, we conducted these simulations in order to build more advanced simulations for ratios of concentrations later. They also illustrate the meaning of the confidence interval.

## Determination Of Ratio Of Concentrations

In previous section, we established a method for estimating the true concentration of the target molecules in the DNA sample from the count of positive chambers as well as the 95% confidence interval for this estimation. We also showed how the sampling distribution  is related to the sampling distribution f(pˆ).

In CNV, the goal is to determine ratio of true concentrations of two genes, one being reference gene and the other being test gene, and associated confidence interval, which we now accomplish in next subsections.

## Fieller'S Theorem And Its Geometric Interpretation

Let the sampling distributions of the test gene and the reference gene be  and , respectively. If these distributions were normal, then one can make use of Fieller's Theorem [18], [19].

However, as mentioned in previous section, one can not make this assumption in general. It is useful to go through the geometric interpretation of Fieller's theorem so that one can solve the problem for arbitrary sampling distributions. See Figure 6 for geometric interpretation of Fieller's Theorem [20], [21].

Assume  and  are normal. For  and , the ratio  can be shown as the slope of the line in the two-dimensional plane which passes through the origin and the 2-D point (, ). Luxburg et al. show in [20], [21] how a confidence ellipse in the two-dimensional plane can be constructed. Consider the two lines which pass through the origin and are tangents to this ellipse. The intersection of these lines with the vertical line at  gives the desired confidence interval.

## Ratio Of Concentrations

In this paper we have presented data in a controlled experimental system, where a synthetic DNA construct was spiked into human cell line DNA at different concentrations. In this case, the synthetic construct, which was to the RPP30 gene, was used as the target, and the RNase P gene which was endogenous to the human cell line, was used as the reference gene. The two genes were identified using two separate PCR reactions, using separate PCR primers and probes. Since there is no reason to assume that the amplification and detection of the target and reference genes are linked,  and  are independent variables.

It is easy to see from the proof of Fieller's theorem and its geometric interpretation that one can compute sampling distribution q of the ratio estimator  as follows:This can be interpreted as cutting out thin wedges in the joint distribution of  and  and accumulating the probabilities inside the wedge to compute the function q in the corresponding thin interval of the ratio. This is the basis of our numerical algorithm which implements integration in order to compute q(rˆ):

See Figure 7 for illustration of the above algorithm.

One can still use direct formulas, as an approximation, to compute confidence interval as follows.

The means of  and  are λ 1 and λ 2 respectively. Let the standard deviations be σx and σy respectively. For given estimations  and , assuming that distributions are normal, it follows from the analysis in [20], [21] that the boundary of the confidence ellipse for a given confidence level zc would be defined byIt is easy to generalize this to the case, under a reasonably close approximation, when we have asymmetric distributions which are assumed to be normal in each of the four quadrants of the coordinate system centered at (, ). Then the confidence region is made of union of four quadrant-wise elliptic regions.

Let the asymmetric confidence intervals for specified zc and the two concentrations be [, ] and [, ].

If WR = WL = zcσx and HT = HB = zcσy, it is symmetric case [20], [21]. Using simple algebraic manipulations, it can be shown, as in symmetric case, that the slopes of lines that will be tangents to this union of four quadrant-wise ellipses will beThe above equations can be used as an approximation though numerical algorithm will give more accurate results as the algorithm does not make any assumptions and works with arbitrary sampling distributions.

One detail has to be mentioned. Special care has to be taken if the confidence region gets too close to  axis when  is small. If it touches  axis, then rHigh = ∞. If either  or  is too small, one can build respective histogram with smaller bin size to get more accurate results.

See Table 2 for summary of equations derived in order to solve the copy number variation problem. Though the numerical approach based on histograms is recommended as it does not make assumptions, these direct formulas can be used as close approximation.

## Simulation Results On Estimation Of Ratio

We conducted simulation studies, using a random number generator and a computer program as in previous section, by choosing a ratio of 2 of concentrations of two genes, which are independent of each other, and building a distribution of estimated ratios over 50 thousand panels. In 94.9% of the panels, the true chosen ratio did lie in the computed confidence intervals thereby showing the correctness of our mathematical analysis.

## Actual Results On Estimation Of Ratio

The copy number variation results for known ratios of 1, 1.5, 2, 2.5, 3 and 3.5 are shown in Figure 8. Materials and methods for this experiment are discussed in next section. As the number of panels P increases, then the number of chambers C = 765 Pincreases and therefore the estimation of the ratio becomes more accurate as well as the confidence interval shrinks. When only 1 panel is used, there is significant overlap between 95% confidence intervals of certain ratios e.g. between ratio 2 and 2.5. There is no overlap when 3 or more panels are used. In all cases the known ratio lies within the computed 95% confidence interval. Note that using mathematical analysis one can find optimal numbers of positive chambers for each ratio which give smallest confidence intervals and which will therefore improve the results.

In summary, Fluidigm's digital array is capable of accurately quantitating DNA samples and is a valuable platform for studying copy number variation. It is a robust technology that is sequence-specific, easy-to-use, and extremely flexible. We have presented mathematical and algorithmic solutions to analyze CNV on a digital array. The solution is an elegant application of statistical sampling and estimation theories to such an important real-world problem. We have shown how one can compute the true concentration of a target sequence in a DNA sample and the associated confidence interval on this estimation, and how one can compute the ratio of true concentrations of multiple sequences and the associated confidence interval on the estimation of this ratio.

# Materials And Methods

A 10-µl reaction mix is normally prepared for each panel. It contains 1× TaqMan Universal master mix (Applied Biosystems, Foster City, CA), 1× RNase P-VIC TaqMan assay, 1× TaqMan assay for the target gene (900 nM primers and 200 nM FAM-labeled probe), 1× sample loading reagent (Fluidigm, South San Francisco, CA) and DNA with about 1,100–1,300 copies of the RNase P gene. 4.59 µl of the 10-µl reaction mix was uniformly partitioned into the 765 reaction chambers of each panel and the digital array was thermocycled on the BioMark system. Thermocycling conditions included a 95°C, 10 minute hot start followed by 40 cycles of two-step PCR: 15 seconds at 95°C for denaturing and 1 minute at 60°C for annealing and extension. Molecules of the two genes were independently amplified. FAM and VIC signals of all chambers were recorded at the end of each PCR cycle. After the reaction was completed, Digital PCR Analysis software (Fluidigm, South San Francisco, CA) was used to process the data and count the numbers of both FAM-positive chambers (target gene) and VIC-positive chambers (RNase P) in each panel.

A spike-in experiment was performed using a synthetic construct to explore the digital array's feasibility as a robust platform for the CNV study. A 65-base oligonucleotide was ordered from Integrated DNA Technologies (Coralville, IA) that is identical to a fragment of the human RPP30 gene. The sequences of the primers and FAM-BHQ probe used to amplify this construct are from Emery et al [22]. The primers and probe were ordered from Biosearch Technologies (Novato, CA).

Both RPP30 synthetic construct and human genomic DNA NA10860 (Coriell Cell Repositories Camden, NJ) were quantitated using the RPP30 assay on a digital array. Different amounts of RPP30 synthetic construct was then added into the genomic DNA so that mixtures with ratios of RPP30 to RNase P of 1∶1 (no spike-in), 1∶1.5, 1∶2, 1∶2.5, 1∶3, and 1∶3.5 were made simulating DNA samples containing 2 to 7 copies of the RPP30 gene per diploid cell.

These DNA mixtures were analyzed on the digital arrays as described above. Five panels were used for each mixture and 400–500 RNase P molecules were present in each panel. The ratios of RPP30/RNase P of all samples were calculated using the techniques developed in this paper. For each ratio, we did pooled analysis by adding the numbers of positive chambers in the first P = 1,2,3,4,5 panels. The results are summarized in the previous section and in Figure 8.

