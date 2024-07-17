# Bootstrapping

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

