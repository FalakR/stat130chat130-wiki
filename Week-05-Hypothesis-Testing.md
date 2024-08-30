**TUT/HW Topics**

1. [null and alternative hypotheses](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#Null-and-Alternative-Hypotheses)
2. [sampling distribution under the null](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#The-Null-Sampling-Distribution)
    1. [role of sample size n](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#The-Role-of-Sample-Size-n) (re: [how standard error is driven by n](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/04-Bootstrapping#How-n-Drives-Standard-Error))
    2. [one sample "paired difference" hypothesis tests with a "no effect" null](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/04-Bootstrapping#one-sample-paired-difference-hypothesis-tests-with-a-no-effect-null)
3. [p-values](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#p-values)
4. [one- versus two-sided hypothesis tests](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/_edit#one--versus-two-sided-hypothesis-tests)
    
**LEC Extensions**

3. using p-values

**LEC New Topics**

1. Type I and Type II errors


# TUT/HW Topics

## Null and Alternative Hypotheses

> 1. [null and alternative hypotheses](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#Null-and-Alternative-Hypotheses)

Statistical hypothesis testing implements the scientific method of using data to falsify a theory. Specifically, it follows a very technically precise protocol of proposing an assumption about a population, and then giving (or failing to give) evidence against this assumption. The way this evidence (or lack of evidence) is created is by using statistics (of samples) to make inferences about the population. That is, a hypothesis about a parameter of the population is made, a statistic is then used to provide inference about this parameter, and this in turn allows us to consider the plausibility of the hypothesis regarding the parameter of the population (based on 
the sample). In essence, hypothesis testing allows researchers to make probabilistic statements about [population parameters](https://github.com/pointOfive/STA130_ChatGPT/wiki/03-Data-Visualization#statistics-estimate-parameters) without knowing the entire population. 

The methodical approach of hypothesis testing to making decisions about a population based on observed sample data begins by formulating the null hypothesis, $H_0$. Hypothesis testing works by giving (or failing to give) evidence against $H_0$, which at first may appear confusing because we are "assuming $H_0$ is true" just to immediately turn around and attempt to "give evidence $H_0$ is not true". The thing is, the null hypothesis is only assumed to be true until there is sufficient evidence to suggest otherwise.

- We assume $H_0$ is true, but in fact hypothesis testing is based on giving (or failing to give) evidence against $H_0$ in order to reject $H_0$... so when we "assume $H_0$ is true", we're secretly really thinking "perhaps $H_0$ is actually really not true".

The most common forms of $H_0$ therefore tend to be the most default "uninteresting" beliefs about a situation, such as "there is no effect of the treatment" or "the coin in fair". Then, if we give evidence against $H_0$, that means we believe that indeed there is in fact something "interesting" going on, such as "there is an effect of the treatment!" or "the coin isn't fair -- there's cheating!" which is probably really what we're trying to use hypothesis testing to do anyway. 

Here are some acceptable forms that a null hypothesis could take.

- $H_0:$ There is "no affect" of a treatment intervention on the average value in the population
    - $H_0: \mu = 0$ [is an equivalent statement to $H_0$ above if $\mu$ is the parameter representing the change in average values in the population due to the treatment intervention]
- $H_0: \mu = \mu_0$ [for example, $\mu_0$ might be equal to $0$ or some other hypothesized value]
    - $H_0: p = 0.5$ [for example could refer to the statement that "the chance a coin is heads is 50/50"]
    - $H_0:$ The coin is "fair"

A somewhat curious circumstance is the alternative hypothesis $H_A$, which initially appears to be the same "kind" of thing as the null $H_0$.  But the alternative $H_A$ is only the very simple statement "$H_A: H_0 \text {is false}$", which emphasizes that the only thing hypothesis testing really addresses in the null hypothesis $H_0$.

- If we have "strong evidence against $H_0$" then we will likely "choose to reject $H_0$".
- Curiously again, it is not recommended to therefore correspondingly say "we accept $H_A$"... this is because rather than "proving" anything, hypothesis testing is only concerned with "giving evidence against $H_0$"... so saying "we accept $H_A$" is just not in the spirit of hypothesis testing, whereas saying "we choose to reject $H_0$" is more appropriate language.
- In the same way, since we are never "proving" anything with hypothesis testing, we would never say "we accept $H_0$" (if we failed to provide sufficient evidence against $H_0$) when doing hypothesis testing... we would instead rather say "we fail to reject $H_0$".

> These "appropriate language usage" caveats may well seem like they are splitting hairs in an overly pedantic manner; but, adopting the correct use of terminology and nomenclature will all you to more quickly correctly orient your understanding of hypothesis testing around the use and meaning of $H_0$.

## The Null Sampling Distribution

> 2. [sampling distribution under the null](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#The-Null-Sampling-Distribution)
>    1. [role of sample size n](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#The-Role-of-Sample-Size-n) (re: [how standard error is driven by n](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/04-Bootstrapping#How-n-Drives-Standard-Error))
>    2. [one sample "paired difference" hypothesis tests with a "no effect" null](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/04-Bootstrapping#one-sample-paired-difference-hypothesis-tests-with-a-no-effect-null)

The sampling distribution under the null characterizes the variability/uncertainty of the observed sample statistic in the hypothetical situation that the null hypothesis $H_0$ is true. Of course the observed sample statistic has already been observed, but we could nonetheless imagine using simulation to repeatedly create synthetic samples (and corresponding sample statistics) under the assumption of the null hypothesis $H_0$ to understand the variability/uncertainty sample statistic (in the hypothetical situation that the null hypothesis $H_0$ is true).

As a quick example, if we wanted to test the assumption that a coin is fair, our hypotheses would be

- $H_0$: The coin is fair (i.e., the probability of heads is 0.5).
- $H_1$: $H_0$ is false: The coin is not fair (i.e., the probability of heads is not 0.5).

Then there are three characteristics of an experiment simulating the sampling distribution under $H_0$.

- `n_coinflips_per_sample`: the sample size (i.e., the number of coin flips that we're considering to use as the evidence regarding the null hypothesis)
- `n_sample_simulations`: the number of samples to simulate (i.e., the number of times to flip a coin `n_coinflips_per_sample ` times)
- `H0_p = 0.5`: the hypothesized value of the parameter specified by the null hypothesis; namely, $H_0: p = 0.5$ (where $p$ denotes the probability of the coin landing on heads)

```python
import numpy as np
from scipy import stats
import plotly.express as px

# Set the parameters
n_coinflips_per_sample = 100  # Number of coin flips per sample
n_sample_simulations = 10000  # Number of samples to simulate
simulated_proportion_coinflips_heads = np.zeros(n_sample_simulations)
H0_p = 0.5

# Simulate the samples
for i in range(n_sample_simulations):
    # Simulate the coin flips
    n_coinflips = np.random.choice(['heads','tails'], p=[H0_p,1-H0_p], size=n_coinflips_per_sample)
    simulated_proportion_coinflips_heads[i] = (n_coinflips=='heads').mean()
    # or just use `proportion_coinflips_heads[i] = 
    #              np.random.choice([0,1], p=[H0_p,1-H0_p], size=n_coinflips_per_sample).mean()`

# or instead of the `for` loop, this could be done as
# simulated_coinflips = stats.binom(n=1, p=0.5, size=(n_samples, n_coinflips_per_sample))
# simulated_proportion_coinflips_heads = simulated_coinflips(axis=1)

# Create the histogram using plotly.express
fig = px.histogram(simulated_proportion_coinflips_heads, nbins=30, 
                   title='Sampling Distribution of Proportion of Heads (Fair Coin)',
                   labels={'value': 'Proportion of Heads', 'count': 'Frequency'})
fig.update_traces(name='Simulated Proportions')
fig.show()
```

### The Role of Sample Size n

The exercise "creating the sampling distribution of the proportion of heads when a coin is fair" above may at first seem somewhat strange and counter intuitive. Why are we running simulations at all? Why don't we just flip the coin a really large number of times to find out if it's fair or not? In fact, the `stats.binom(n=1, p=0.5, size=(n_samples, n_coinflips_per_sample))` formulation even shows that this is exactly what we're doing, and all that we've really done is to just organize the coin flips into simulations (on the rows) with individual coin flips (in the columns on a row)...

Think of the "fair coin" example as just being an example of the template that we have when we collect data. We collect some number of data points (with a sample sample size of n), and then we can calculate a statistic using that sample.  The question now is, what is the variability/uncertainty of that sample statistic? And that (of course) depends on the sample size n. The statistic can then be used to provide inference regarding the population parameter it correspondingly estimates, and the strength of the evidence it provides depends on the variability/uncertainty of the statistic (which in turn depends on the sample size n used to construct the statistic).

> This general concept was previously discussed in terms of the [standard error versus the standard deviation](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/04-Bootstrapping#How-n-Drives-Standard-Error) in the context of confidence intervals; and, the standard deviation of the sampling distribution (of a statistic) under the null exactly addresses the same topic; namely, there is a **standard error** of a statistic under the assumption of the null hypothesis which captures the variability/uncertainty of the statistic and therefore subsequently influences the strength of evidence that the data will be able to provide regarding the null hypothesis $H_0$.


### One Sample "paired difference" Hypothesis Tests with a "no effect" Null

In contexts with two samples where the observations of interest are "paired difference" (such as in a "before versus after" treatment intervention) the following two null hypotheses can be considered.

- $H_0: \mu = 0$ [there is "no effect" of the treatment intervention: the change in the population average $\mu$ is $0$]
- $H_0: p = 0.5$ [changes due to treatment intervention are "random": the chance of improvement $p$ is 50/50"]

The former null hypothesis results in what's known as a "paired t-test" but this hypothesis test is beyond the scope of STA130 so we will not consider this null hypothesis further. The latter null hypothesis can be seen to have the same template format as the "coin flipping" hypothesis testing discussed above.

- The sample pairs are compared and the proportion "greater than" is the observed test statistic
- The sampling distribution under the null is then simulated with a "fair" coin flipping exercise

> If you cannot complete the exercise of leveraging the "coin flipping code" to do a "paired difference" hypothesis test, you should immediately seek help from a ChatBot, office hours, or study buddies to understand why "coin flipping" and "paired difference" hypothesis test are "equivalent"

## p-values

> 3. [p-values](https://github.com/pointOfive/STA130_ChatGPT/wiki/05-Hypothesis-Testing#p-values)

A **p-value** is **the probability that a test statistic is as or more extreme than the observed test statistic if the null hypothesis is true**.

A p-value is a measure of the strength of the evidence against $H_0$. The smaller the p-value the stronger the evidence is against the null hypothesis. But why is this so? A **p-value** is created by comparing the observed sample statistic against the sampling distribution of the statistic under the assumption that the null hypothesis is true.  If the observed statistic does not seem plausible relative to the sampling distribution of the statistic under the assumption that the null hypothesis is true, then the evidence in the sample data does not agree with the sampling distribution of the statistic under the assumption that the null hypothesis is true. Therefore, the sample data provides evidence against the sampling distribution of the statistic under the assumption that the null hypothesis is true. Therefore, the sample data provides evidence against the assumption that the null hypothesis is true.

To give a measure of how much the data does not agree with the sampling distribution of the statistic under the assumption that the null hypothesis is true, we compute the **p-value**. That is, we find out the proportion of the statistics sampled from the sampling distribution of the statistic under the assumption that the null hypothesis is true that look "as or more extreme" than the observed statistic calculated from the sample data. The "as or more extreme" consideration requires us to calculate the relative differences of the observed statistic with the parameter valued specified by the null hypothesis, and compare these with the analogously calculated relative differences of the statistics sampled from the sampling distribution of the statistic under the assumption that the null hypothesis is true (with the parameter valued specified by the null hypothesis).

For the "coin flipping" simulation example above, this would be done as follows.

```python
import plotly.graph_objects as go

observed_proportion_statistic = 0.6
print("If we observed a proportion of", observed_proportion_statistic, "heads out of", n_coinflips_per_sample, "coin flips")
print("The (p-value) chance that we would get a proportion (statistic)")
print('further from 50/50 ("as or more extreme") than the proportion of')
print("heads we saw (observed statistic) if the coin was fair (H0 is true)")
as_or_more_extreme = abs(simulated_proportion_coinflips_heads - H0_p) >= abs(observed_proportion_statistic - H0_p)
print("would be", as_or_more_extreme.sum()/n_sample_simulations, "which we estimated using", n_sample_simulations, "simulations of", n_coinflips_per_sample, "coin flips")
print("as illustrated below")

fig = px.histogram(simulated_proportion_coinflips_heads, nbins=30, 
                   title='Sampling Distribution of Proportion of Heads (Fair Coin)',
                   labels={'value': 'Proportion of Heads', 'count': 'Frequency'})
fig.update_traces(name='Simulated Proportions<br>under the assumption<br>that H0 is true<br>')

# Add vertical line for observed proportion
fig.add_trace(go.Scatter(x=[observed_proportion_statistic] * 2,
                         y=[0, max(np.histogram(simulated_proportion_coinflips_heads, bins=30)[0])],
              mode='lines', line=dict(color='red', width=2), name='Observed Proportion'))

# Mark the p-value area
x_as_or_more_extreme = simulated_proportion_coinflips_heads[as_or_more_extreme]
fig.add_trace(go.Scatter(x=x_as_or_more_extreme, y=0*x_as_or_more_extreme,
    mode='markers', marker=dict(color='red', size=10, symbol='x'),
    name='"as or more extreme" (symmetric)'))
fig.show()
```

## One- versus Two-Sided Hypothesis Tests

> 4. [one- versus two-sided hypothesis tests](https://github.com/pointOfive/STA130_ChatGPT/wiki/hypothesis-stuff/_edit#one--versus-two-sided-hypothesis-tests)

Also known as one-tailed hypothesis tests, a variation on the traditional "equality" null hypothesis (such as $H_0: p = 0.5$) which allows us to address comparisons in an ordered manner replaces the two-sided (or two-tailed) alternative hypothesis (such as $H_A: p \neq 0.5$) with one-sided (or one-tailed) alternatives; namely...

$$\Large H_A: p > 0.5 \quad\text{ or }\quad H_A: p < 0.5 \quad\text{ which then correspond to }\quad H_0: p \leq 0.5 \quad\text{ or }\quad H_0: p \geq 0.5$$

Naturally, the (now familiar) $H_0$-centric form remains preferred, so these correspond to 

$$\Large H_0: p \leq 0.5 \text{ and } H_A: H_0 \text{ is false} \quad\text{ or }\quad H_0: p \geq 0.5 \text{ and } H_A: H_0 \text{ is false}$$

Using a one-sided (one-tailed) hypothesis test allows us to address questions of directional difference as opposed to simply "no difference". For example, this allows us to address the question of whether an unfair coin is bias in favor "heads" or against heads, depending on our choice of null and alternative hypothesis pair.  Depending on the research question, we may indeed be interested in giving evidence against a null hypothesis in a manner that takes into affect a single comparative direction, as opposed to "a difference in either direction". 

In addition, by using a one-sided test that is specifically interested in detecting an difference in one direction only, the p-value will necessarily become smaller since "as or more extreme" is now only considered in a single tail of the sampling sampling distribution of the statistic. 


# LEC Extensions

## Using p-values

# LEC New Topics

## Type I and Type II errors



We would then flip the coin many times, collect the data, and use a statistical test to determine whether to accept or reject the null hypothesis.

* Collect Data and Perform the Test: Use statistical methods to test the null hypothesis.
* Make a Decision: Based on the statistical analysis, decide whether to accept or reject the null hypothesis.

That said, whether there is enough evidence in a sample of data to infer that a certain condition holds true for the entire population. 

**Steps in Hypothesis Testing**

1. **Formulate the Null Hypothesis (H₀):** This hypothesis states that there is no effect or no difference.
2. **Formulate the Alternative Hypothesis (H₁):** This hypothesis states that there is an effect or a difference.
3. **Collect Data and Perform the Test:** Use statistical methods to test the null hypothesis.
4. **Make a Decision:** Based on the statistical analysis, decide whether to accept or reject the null hypothesis.



- In statistical hypothesis testing, we do not "prove $H_0$ is false"; rather, we instead give evidence against the $H_0$. 

We would say something like, 
> "We reject the null hypothesis with a p-value of abc, meaning we have xyz evidence against the null hypothesis"

We do not prove $H_0$ is true, we instead do not have evidence to reject $H_0$:
> "We fail to reject the null hypothesis with a p-value of abc"



**Qualitative interpretation of p-value**

A numerical p-value is usually converted to a "qualitative" result. In practical terms, decisions often need to be made in a straightforward, binary manner (e.g., accept or reject the null hypothesis). By setting a significance level (commonly $\alpha = 0.05$), researchers can make a clear decision based on whether the p-value is below or above this threshold.

|p-value|Evidence|
|-|-|
|$$p > 0.1$$|No evidence against the null hypothesis|
|$$0.1 \ge p > 0.05$$|Weak evidence against the null hypothesis|
|$$0.05 \ge p > 0.01$$|Moderate evidence against the null hypothesis|
|$$0.01 \ge p > 0.001$$|Strong evidence against the null hypothesis|
|$$0.001 \ge p$$|Very strong evidence against the null hypothesis|
