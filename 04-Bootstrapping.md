# TUT/HW Topics

## Simulation

> 1. [simulation](04-Bootstrapping#Simulation) (with `for` loops and `from scipy import stats`)

In statistics, simulation refers to the process of repeating a sampling process a large number of times in order to understand some aspect of the behavior of the process. The following code visualizes a Normal distribution, and the subsequent code shows the simulation based visualization of the Normal distribution. 

**A distribution:**

```python
# plotly.express provides visualization, scipy.stats provides distributions
# numpy provides numerical support, and pandas organizes data
import plotly.express as px
from scipy import stats
import numpy as np
import pandas as pd

# mean (mu) and standard deviation (sigma) parameters 
# determine the location and spread of a normal distribution 
mu, sigma = 1, 0.33
normal_distribution_object = stats.norm(loc=mu, scale=sigma)

# `np.linspace` creates an array of values evenly spaced within a range 
# over which we will visualize the distribution
support = np.linspace(-1, 3, 100)

# probability density functions (PDFs) show the long term relative frequencies 
# that will be seen when sampling from a distribution
pdf_df = pd.DataFrame({'x': support, 'density': normal_distribution_object.pdf(support)})
fig = px.line(pdf_df, x='x', y='density')
fig.show()
```

**A simulation of a distribution:**

```python
n = 10000 # sample size
normal_distribution_sample = np.zeros(n)

for i in range(n):
    normal_distribution_sample[i] = normal_distribution_object.rvs(size=1)[0]

# or, more simply
normal_distribution_sample = normal_distribution_object.rvs(size=n) 

# Plot the PDF and sample histogram
fig = go.Figure()

# Add histogram using Plotly's built-in histogram functionality
fig.add_trace(go.Histogram(x=normal_distribution_sample, 
                           histnorm='probability density', 
                           nbinsx=30, name='Sample Histogram', opacity=0.6))

# Add PDF line
fig.add_trace(go.Scatter(x=pdf_df['x'], y=pdf_df['density'], mode='lines', name='PDF'))

# Update layout
fig.update_layout(title='Normal Distribution PDF and Sample Histogram',
                  xaxis_title='Value', yaxis_title='Density')
```

The sampling process being simulated is "sampling a single observation from a Normal distribution".  The number of simulations is `n` and the `for` loop repeats the simulation the sampling process a large number of times (as determined by `n`) in order to understand the behavior of the process of "sampling a single observation from a Normal distribution".  Of course, the `.rvs(size=n)` method let's us do this without a `for` loop by instead just using `normal_distribution_object.rvs(size=n)`.

- Consider experimenting with different values of `n` to explore how the "size" of the simulation determines how well clearly the behavior of the process is understood.

**Another simulation:**

Consider the following alteration on the previous simulation.

```python
import plotly.express as px

number_of_simulations = 1000
n = 100 # sample size
normal_distribution_sample_means = np.zeros(number_of_simulations)

for i in range(number_of_simulations):
    normal_distribution_sample_means[i] = normal_distribution_object.rvs(size=n).mean()

df = pd.DataFrame({'Sample Mean': normal_distribution_sample_means})
fig = px.histogram(df, x='Sample Mean', nbins=30, title='Histogram of Sample Means', 
                   labels={'Sample Mean': 'Sample Mean', 'count': 'Frequency'})
fig.show()
```

Here are some questions to answer to make sure you understand what this simulation is doing (compared to the previous simulation):

1. Why is `number_of_simulations` introduced and why is it different than `n`?
2. What is the effect of appending the `.mean()` method onto `normal_distribution_object.rvs(size=n)` inside the simulation `for` loop?
3. Are the histograms from the two simulations comparable? If not, why are they not really comparable? 

As this second simulation example shows, simulation can explore much more interesting downstream behaviours of sampling processes besides just the behavior or "sampling a single observation from a Normal distribution" (or some other distribution if you were to sample from something other than a Normal distribution).

## Variability/Uncertainty of the Sample Mean

> 2. [the sampling distribution of the mean](04-Bootstrapping#Variability/Uncertainty-of-the-Sample-Mean)

The previously code simulated the so-called **sampling distribution of the mean**, which captures the variability/uncertainty of the sample mean. So, just as data points are sampled from some distribution, statistics (which are mathematical functions) of samples as well can be viewed on a higher level as themselves (that is, the statistics themselves) as being "a sample" from the "distribution of the statistic". 

The concept that a statistics (such as the sample mean $\bar x$) would itself be drawn from its own distribution is very intuitive when viewed from the perspective of simulation (as demonstrated in the example given in the previous section). 

> The theory related to the **sampling distribution of the mean** is known as **the Central Limit Theory (CLT)** and relates as well to the so called **Law of Large Numbers (LLN)**; but, these topics are beyond the scope of STA130 -- and not the focus of STA130 -- so for STA130 it's very important that you approach understanding sampling distributions through a simulation-based perspective. 


## Standard Deviation versus Standard Error

> 3. [standard deviation versus standard error](04-Bootstrapping#Standard-Deviation-versus-Standard-Error)

A simple summary of the difference of these two concepts is that 
1. "standard deviation" refers to the **standard deviation** of a sample or a population; whereas, 
2. "standard error" (that is, the **standard error of the mean**) refers to the "standard deviation of the sampling distribution of the sample mean"
 
The **sample standard deviation** is defined as 

$$s = \sqrt{\frac{1}{n-1}\sum_{n=1}^n (x_i-\bar x)}$$

and (as you can see with some inspection and consideration) it measures something how spread out the data is by measuring something like "how far on average" individual data points are from the sample mean $\bar x$ of the data set. **Standard deviation** describes the variation (or dispersion) in a set of data points. A high standard deviation indicates that the data points are spread out over a wide range of values, while a low standard deviation indicates that they are clustered closely around the mean.

> Be careful to distinguish between the **sample standard deviation** statistic $s$ and the Normal distribution **standard deviation parameter** σ (which $s$ estimates): $s$ is the sample analog of the population concept σ.

The **standard error of the mean** is (due to the **Law of Large Numbers (LLN)**) defined as 

$$s_{\bar x} = \frac{s}{\sqrt{n}}$$ 

and measures how much the sample mean (average) of the data is expected to vary from the true population mean (such as μ if the sample was drawn from a Normal distribution). The **standard error of the mean** therefore captures the precision of the sample mean as an estimate of the population mean. 

> There are key differences between the **sample standard deviation** and the **standard error of the mean** that should be readily recognizable. First, the **standard deviation** applies to the entire sample or population, reflecting the variability among individual data points; whereas, the **standard error of the mean** only applies to the sample mean and reflects the variability/uncertainty of the sample mean ($\bar x$) as an estimate of the population mean (μ). Second, the **standard deviation** is independent of the sample size since the size of a sample is not the determining factor in how variability there is between individual data points (since it is the nature of the variability in the individual data points in the population which determines this); whereas, **the standard error of the mean** decreases as the sample size increases, indicating more precise estimates with larger samples.

> A theoretical form that you will see related to the **standard error of the mean** $s_{\bar x}$ is 
> 
> $$ \bar x \pm 1.96 s_{\bar x}$$ 
> 
> which produces a 95% confidence interval based on the **Central Limit Theorem (CLT)** using the so-called "pivot" trick. 
> However, since STA130 is focussed on understanding the variability/uncertainty of the sample mean from a simulation-based perspective, this methodological framework is beyond the scope and not the focus of STA130. 

## How n Drives Standard Error

> 4. [how standard error is driven by n](04-Bootstrapping#How-n-Drives-Standard-Error)

As seen above, **standard error of the mean** is $s_{\bar x} = \frac{s}{\sqrt{n}}$; so, the theoretical value of the **standard error** is calculated by dividing the standard deviation by the square root of the sample size $n$. 

So, the larger the sample size $n$, the smaller **standard error** (so the smaller the "standard deviation of the sampling distribution of the sample mean"), and thus the more precisely the sample mean $\bar x$ estimates the population mean μ.

> There might be a question as to whether or not the **sampling distribution of the sample mean** should be increasingly centered on the corresponding population mean μ, and the answer is "yes" under the usual assumptions underlying a statistical analysis.


## Bootstrapping

Bootstrapping is a statistical method that involves resampling with replacement from a sample to create many simulated samples. It is used to estimate the sampling distribution of a statistic (like the mean) and to assess the uncertainty of estimates.

**Steps in Bootstrapping**

Take an initial sample from the population.
- Resample with replacement to create many simulated samples (bootstrapped samples).
- Calculate the statistic of interest (e.g., mean) for each bootstrapped sample.
- Analyze the distribution of the calculated statistics to make inferences.

**Example Code for Bootstrapping**

Let's walk through a Python code example to perform bootstrapping using a theoretical population.

**Step-by-Step Guide**

1. **Import Libraries:** Import the necessary libraries to handle statistical functions, numerical operations, and data manipulation.

```python
import numpy as np
import pandas as pd
import plotly.express as px
```

2. **Define Parameters:** Set the number of bootstrap repetitions (reps) and the sample size (sample_size).

```python
reps = 1000
sample_size = 333
```

3. **Initialize Storage:** Create an array to store the means of the bootstrapped samples.

```python
my_theoretical_population_sample_means = np.zeros(reps)
```

4. **Set Random Seed:** Set a random seed to ensure reproducibility of results.

```python
np.random.seed(130)
```

5. **Bootstrapping Loop:** Perform the bootstrapping by taking repeated samples from the theoretical population, calculating the mean of each sample, and storing it.

```python
for i in range(reps):
    my_theoretical_sample = my_theoretical_population.rvs(size=sample_size)
    my_theoretical_population_sample_means[i] = my_theoretical_sample.mean()
```

6. **Create DataFrame:** Create a DataFrame to store the bootstrapped sample means.

```python
df = pd.DataFrame({'sample_averages': my_theoretical_population_sample_means})
```

7. **Visualize Results:** Use Plotly Express to create a histogram of the bootstrapped sample means.

```python
px.histogram(df, x='sample_averages')
```

**Complete Code Example**

Here is the complete code to perform bootstrapping and visualize the results:

```python
import numpy as np
import pandas as pd
import plotly.express as px

# Define the number of repetitions and sample size
reps = 1000
sample_size = 333

# Initialize an array to store the means of the bootstrapped samples
my_theoretical_population_sample_means = np.zeros(reps)

# Set a random seed for reproducibility
np.random.seed(130)

# Perform bootstrapping
for i in range(reps):
    # Take a sample from the theoretical population
    my_theoretical_sample = my_theoretical_population.rvs(size=sample_size)
    
    # Calculate the mean of the sample and store it
    my_theoretical_population_sample_means[i] = my_theoretical_sample.mean()

# Create a DataFrame to store the bootstrapped sample means
df = pd.DataFrame({'sample_averages': my_theoretical_population_sample_means})

# Create a histogram to visualize the bootstrapped sample means
px.histogram(df, x='sample_averages')
```

# Confidence

**Interpretation of Confidence Intervals**

If we calculate a 95% confidence interval for a population mean, it means we are 95% confident that the interval contains the true population mean. This does not mean that there is a 95% probability that the interval contains the true mean; rather, it means that if we were to take many samples and build a confidence interval from each of them, we expect about 95% of those intervals to contain the true mean.

# Confidence Intervals

A confidence interval is a range of values that is likely to contain a population parameter with a certain level of confidence. It provides an estimated range that is likely to include the true value of the parameter we are interested in.

**Steps to Calculate Confidence Intervals**

1. **Collect Sample Data:** Gather data from a sample of the population.
2. **Calculate the Sample Statistic:** Calculate the mean or other statistic of interest from the sample.
3. **Determine the Confidence Level:** Common choices are 90%, 95%, or 99%.
4. **Calculate the Margin of Error:** Use statistical formulas to calculate the margin of error based on the sample data and confidence level.
5. **Construct the Confidence Interval:** Add and subtract the margin of error from the sample statistic to get the confidence interval.

**Example Code for Calculating a 95% Confidence Interval**

Let's walk through a Python code example to calculate a 95% confidence interval using bootstrapped sample means.

**Step-by-Step Guide**

**1. Import Libraries:** Ensure the necessary libraries for numerical operations and data manipulation are imported.

```python
import numpy as np
import pandas as pd
```

**2. Calculate Quantiles:** Use the np.quantile function to calculate the 2.5th and 97.5th percentiles of the bootstrapped sample means. This gives us the 95% confidence interval.

```python
data_quantile = np.quantile(my_theoretical_population_sample_means, [0.025, 0.975])
```

**3. Print the Confidence Interval:** Output the calculated confidence interval.

```python
print(data_quantile)
```

**Complete Code Example**

Here is the complete code to calculate a 95% confidence interval from the bootstrapped sample means:

```python
import numpy as np
import pandas as pd

# Assume my_theoretical_population_sample_means contains the bootstrapped sample means from previous section
my_theoretical_population_sample_means = np.zeros(1000)  # Example initialization

# Calculate the 95% confidence interval
data_quantile = np.quantile(my_theoretical_population_sample_means, [0.025, 0.975])

# Print the confidence interval
print(data_quantile)
```

