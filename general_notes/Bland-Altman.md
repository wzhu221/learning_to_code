# What’s the thing with Bland-Altman all about?

**The following paragraph is an excerpt of [this article](https://validationmanager.com/bland-altman-plots-bias-estimations/).**

> Usually when evaluating bias, **we do not have access to a reference method that would give true values**. Instead we have a comparative measurement procedure (e.g. an instrument that will be replaced by a new one) that we assume to give about as accurate results as the candidate measurement procedure. Sometimes we even expect the candidate measurement procedure to be better than the comparative measurement procedure. That’s why we cannot calculate the actual trueness (i.e. bias compared to true values) of the new measurement procedure by just comparing the results of our candidate measurement procedure to the comparative measurement procedure.

> So what does the bias mean if we don’t have access to a reference method? How can we estimate trueness?

> In this case, use of Bland-Altman difference is recommended. It compares the candidate measurement procedure to the mean of the candidate and the comparative measurement procedures.

> The point of Bland-Altman difference is pretty simple. The results of both candidate and comparative measurement procedures contain error. Therefore the difference between their results does not describe the trueness of the candidate measurement procedure. Instead, it only gives the difference between the two measurement procedures. That’s why, if we want to estimate trueness, we need a way to make a better estimation about the true concentrations of the samples than what the comparative measurement procedure alone would give.

> Typically we only have two values to describe the concentration of a sample: the one produced by the comparative measurement procedure, and the one produced by the candidate measurement procedure. As mentioned above, the candidate measurement procedure gives often at least as good an estimate as the comparative measurement procedure. That’s why the average of these results gives us a practical estimate for the true values (i.e. the results of a reference method). It reduces the effect of random error, and even the weaknesses of an individual measurement procedure in estimating the true value. This makes Bland-Altman difference a good choice when determining the average bias.

# How to make a Bland-Altman plot

**The following paragraph is an excerpt of [this article](https://stats.stackexchange.com/questions/527/what-ways-are-there-to-show-two-analytical-methods-are-equivalent).**

The simple correlation approach isn't the right way to analyze results from method comparison studies. There are (at least) two highly recommended books on this topic that I referenced at the end (1,2). Briefly stated, when comparing measurement methods we usually expect that (a) our conclusions should not depend on the particular sample used for the comparison, and (b) measurement error associated to the particular measurement instrument should be accounted for. This precludes any method based on correlations, and we shall turn our attention to variance components or mixed-effects models that allow to reflect the systematic effect of item (here, item stands for individual or sample on which data are collected), which results from (a).

In your case, you have single measurements collected using two different methods (I assume that none of them might be considered as a gold standard) and the very basic thing to do is to plot the differences $ X_1 + X_2 $ versus the means ((𝑋1+𝑋2)/2); this is called a bland-altman-plot. It will allow you to check if (1) the variations between the two set of measurements are constant and (2) the variance of the difference is constant across the range of observed values. Basically, this is just a 45° rotation of a simple scatterplot of 𝑋1 vs. 𝑋2, and its interpretation is close to a plot of fitted vs. residuals values used in linear regression. Then,





