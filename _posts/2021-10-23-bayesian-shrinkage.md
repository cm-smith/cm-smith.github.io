---
title: "Dialysis Procedures and Bayesian Shrinkage"
layout: post
---

{% include mathjax-cdn.html %}

*This analysis was performed during my role at a previous employer. All proprietary information (including data specifics, visuals, and results) were censored. The focus of this post is the application and discussion of Bayesian shrinkage methods.*

The Bayes shrinkage methodology was applied to an analysis investigating variation in how patients receive dialysis given nephrologist influence.

Dialysis procedures consist of two approaches, hemodialysis or peritoneal dialysis. Hemodialysis redirects blood out of the body for filtration, whereas peritoneal transfuses dialysis solution into the peritoneum (abdomenal region) where it aids in filtration. Peritoneal dialysis (PD) can be performed at home after a catheter has been surgically attached to the abdominal section. Hemodialysis can also be performed at home, but this seems to be rarer. Although performing these procedures at home can take weeks of training by medical professionals, the at-home application of dialysis has been shown to be just as effective and may even improve the patient's wellbeing (e.g., no longer required to visit a medical center monthly). Additionally, this method is much less expensive, making it a prime target for future investment.

We asked the question,*"Do some providers refer peritoneal dialysis (PD) or home hemodialysis (HHD) more often than in-facility hemodialysis?"* The underlying goal of this question is to determine whether a patient's choice in provider may affect their mode of dialysis. If we find that the variation in dialysis rates by providers vary more than random chance would expect, then there is reason to believe some providers are biased more heavily to certain modes of dialysis. Ultimately, this informs strategy within CKD care and patient education.

The main dataset was comprised of nephrologist visits immediately prior to the initial dialysis claim (up to 1 year prior). This allowed us to analyze provider (nephrologist) behavior and the type of dialysis they referred to their patients. Rather than aggregating at individual provider-level, we aggregated at the "provider group" level. When deteriming if a nephologist recommended HD (inc. HHD) or PD, we only considered th initial dialysis occurrence.

The majority of provider groups saw very few individuals (<10), resulting in noisy data with extreme rates of PD (say, 0% or 100%). To adjust for these small sample sizes, we used shrinkage techniques to reduce noise in the variation within the provider groups. These "shrinkage techniques" weight both the provider's PD rate and the population mean (across all provider groups). For example, we expect a provider group with 1,000 patients to have a less noisy PD rate than a provider group with 10 patients. This led us to use an Empirical Bayes algorithm to estimate howwe would weight these group means. We considered using either [Beta priors](https://varianceexplained.org/r/empirical_bayes_baseball/) or [the James-Stein estimator](http://chris-said.io/2017/05/03/empirical-bayes-for-multiple-sample-sizes/).

## Empirical Bayes: Fitting a Beta Prior

We start by approximating a Beta distribution using the PD rates for provider groups that have at least 2 patients (help control noise). Because the data is highly skewed to the left, the Beta distribution is parameterized with $\alpha < 1$ and $\beta \approx 1$. Beta distribution of this kind do not support x=0 and x=1, however, so the numerical fitting techniques using R package "MASS" (using the optim library in the backend) fail when including PD rates equal to 0 or 1. The workarounds include shifting the values at 0 and 1 slightly inside the bounds (0, 1), e.g., +/-1e-4, or excluding these values. Note that constraining the rates within (0, 1) and maintaining the highly left-skewed data results in very small shrinkage toward the mean (the rates at 0 dominate the estimate of the prior distribution). Excluding the rates at 0 and 1 result in more shrinkage, but is much more biased. Neither approach with the Beta prior are favorable, either due to over-shrinking or under-shrinking. Instead, due to the nature of this highly skewed data, we focus our strategy on the JS Estimator.

## Empirical Bayes: James-Stein Estimator

Instead of constraining the prior to a certain distribution, the James-Stein (JS) Estimator determines how to weight the sample mean ($x_i$) and population mean ($X$) in order to shrink toward the population mean:

$$x'_i = (1-\beta_i) x_i + \beta_i X,$$

The main approach assumes that the variation in the samples is the same ($s_i^2=s^2$), which is typically not true for samples varying in size. If we do take this assumption, then we can solve for a global beta across k samples:

$$\beta = \frac{(k-3)s^2}{\Sigma_i(x_i - X)^2}$$

If we substitute (k-3) for (k-1), we find:

$$\beta \approx \frac{s^2}{\Sigma_i(x_i - X)^2/(k-1)} = \frac{s^2}{\tau^2 + s^2},$$

where $\tau^2$ is the between-group variance of the sample mean ($x_i$). That is, our weight beta is determined by the fraction of variance coming from within-group variation. That is, when the sample mean comes with high uncertainty, we should weight the gloabl mean more. When the sample mean comes with low uncertainty, we should weight the global mean less.

However, this estimator uses the assumption that the variation in each sample is the same, which does not occur very often. The blog post linked above suggests a solution for solving for a Multi-Sample Size James-Stein (MSS JS) Estimator. Instead of saolving for a global beta, we can solve for $\beta_i$ by using the sample variance ($s_i^2$) for each sample $i$:

$$\beta_i = \frac{s_i^2}{\tau^2+s_i^2}$$

An even better approach to this would be to pool the within-group variance, solving for the population variance ($s^2$) and then estimating the group variance:

$${s'}_i^2 = (s^2)(n_i),$$

where $n_i$ is the sample size for group $i$.

This approach results in a shrinkage effect between our two Beta prior extremes. We use this to estimate the coefficient of variation in PD rates between provider groups.

## Closing Thoughts

After landing on the JS Estimator for our final analysis, the results focused on the magnitude of the coefficient of variation. This informed whether the analytics team would pursue more refined analyses. A high coefficient of variation suggested we should do more digging. Additional efforts would include accounting for regional and case mix differences across provider groups.

With respect to the shrinkage techniques, other approaches could have been used, such as MCMC, regularization, Mixed Models, Efron and Morris' JS Estimator, Double Shrinkage, and Kleinman's Weighted Moment Estimator. Many of these are numerical (not analytical) solutions and take a bit more time, but may be worth consideration (especially Mixed Models and Double Shrinkage).
