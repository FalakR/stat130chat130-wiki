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

As this second simulation example shows, simulation can explore much more interesting downstream behaviours of sampling processes besides just the behavior or "sampling a single observation from a distribution".

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

