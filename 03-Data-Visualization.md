**TUT/HW Topics**

1. ["types of data"](03-Data-Visualization#Types-III)... continuous, discrete, nominal and ordinal categorical, and binary
2. [bar plots](03-Data-Visualization#Bar-plots-and-modes) and the [mode](03-Data-Visualization#Bar-plots-and-modes)    
3. [histograms](03-Data-Visualization#Histograms)
4. [box plots](03-Data-Visualization#Box-plots-and-spread), [range](03-Data-Visualization#Box-plots-and-spread), [IQR](03-Data-Visualization#Box-plots-and-spread) and [outliers](03-Data-Visualization#Box-plots-and-spread)
5. [skew](03-Data-Visualization#skew-and-multimodality) and [multimodality](03-Data-Visualization#skew-and-multimodality) 
    1. [mean versus median](03-Data-Visualization#skew-and-multimodality)
    2. [normality and standard deviations](03-Data-Visualization#skew-and-multimodality)
    
**LEC Extensions**

2. plotting... plotly, seaborn, matplotlib, pandas
3. kernel density estimation "violin plots"
4. legends, annotations, figure panels
5. log transformations

**LEC New Topics**

1. populations [`from scipy import stats`](03-Data-Visualization#Populations) (re: `stats.multinomial` and `np.random.choice()`) with `stats.norm` and `stats.poisson`
2. [samples](03-Data-Visualization#Sampling) from populations (distributions) 
3. [statistics estimate parameters](03-Data-Visualization#Statistics-Estimate-Parameters)

**Out of scope**
1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as expectation, moments, integration, heavy tailed distributions...
4. ...such as kernel functions for kernel density estimation
5. ...bokeh, shiny, d3, ...


# TUT/HW Topics

## Types III

> 1. ["types of data"](03-Data-Visualization#Types-III)

Not to be confused with `type()`, `.astype()`, and `.dtypes`; or, `list`, `tuple`, `dict`, `str`, and `float` and `int` (although the latter two correspond to **continuous** and **discrete** numerical variables...). 

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhb3mixP0J2LWeQsK3VWxL8RM0tFg5-8KBLLMYSPfjzZrGw60PCyknoxmqJqyvn6yR2lS4J_8sblZs3blNsagoCq9JHxjSPhtv8PH3DnBZEmPl7zcNZaSjPwCoMQ1j6Cu8tFGULP8uZQmfV/s1600/horst+discrete+continuous.jpg)

![](https://pbs.twimg.com/media/Ehh6v4kVoAIbotc?format=jpg&name=4096x4096)


## Bar plots and modes

> 2. [bar plots](03-Data-Visualization#Bar-plots-and-modes) and the [mode](03-Data-Visualization#Bar-plots-and-modes)

A **bar plot** is a chart that presents categorical data with rectangular bars with heights (or lengths) proportional to the values that they represent. 

```python
import pandas as pd
import plotly.express as px

# Sample data
df = pd.DataFrame({
    'Categories': ['Category1', 'Category2', 'Category3'],
    'Values': [10, 20, 30]
})

# Create a bar plot with Plotly
fig = px.bar(df, x='Categories', y='Values', text='Values') 
# The bars can be plotted horizontally instead of vertically by switching `x` and `y`

# Show the plot
fig.show()
```

A bar plot is essentially a visual representation of the `.value_counts()` method of the `pandas` library.

> Remember, the `.value_counts()` is a method that produces counts of unique values (sorted in descending order starting with the most frequently-occurring element). The the `.value_counts()` method excludes missing values by default, but these can included by instead using `.value_counts(dropna=False)` with the additional `parameter=argument` specification.

```python
# Example data
data = pd.Series(['a', 'b', 'a', 'c', 'b', 'a', 'd', 'a'], name="Variable")

# Use .value_counts()
counts = data.value_counts()

# Create bar plot with Plotly
fig = px.bar(counts, y=counts.values, x=counts.index, text=counts.values)
fig.update_layout(title_text='Value Counts', xaxis_title='Categories', yaxis_title='Count')
# This updates the layout to add a title and labels to the x and y axes to make the plot more immediately sensible
fig.show()
```

In this code, we first count the occurrences of each category in the series using `.value_counts()`. Then, we create a bar plot with `px.bar()`. The `y` argument is set to the counts (the number of occurrences of each category), and the `x` argument is set to the index of the counts (the categories themselves). The `text` argument is used to display the count numbers on top of the bars. For example, the height of the 'a' bar is the count of 'a' in the dataset. 

> A **bar plot** is a simple way to understand the distribution of categorical data that is a good alternative to just reading `df.value_counts()`.
>
> And with interactive `plotly` bar plots, you can hover over the bars to see the exact counts, and you can also zoom in and out, and save the plot as a png file.

The **mode** in statistics is the value (or values) that appears most frequently in a data set. In the bar plot above 'a' is the mode since it appears most frequently in the dataset. 

> The term "modes" is also used to refer to "peaks" in a distribution of data... this will be discussed later in the context of "multimodality"


## Histograms

> 3. [histograms](03-Data-Visualization#Histograms)

A **histogram** is a graphical display of data using bars of different heights. It is similar to a **bar plot**, but a histogram instead counts groups of numbers within ranges. That is, the height of each bar shows how many data points fall into each range. For example if you measure the [heights of trees in an orchard](https://www.mathsisfun.com/data/histograms.html), you could put the heights data into 
and the heights vary from 100 cm to 350 cm in intervals 50 cm, so a tree that is 260 cm tall would be added to the "250-300" range.  Histograms are a great way to show results of numeric data, such as weight, height, time, etc.

- In a histogram, the **width of the bars (also known as bins)** represents the interval that is used to group the data points. The choice of bin size can greatly affect the resulting histogram and can change the way we interpret the data.

- If the **bin size is too large**, each bar might span a wide range of values, which could obscure important details about how the data is distributed. On the other hand, if the **bin size is too small**, the histogram could become cluttered with many bars, making it difficult to see the overall pattern.

- Choosing an appropriate bin size is a balance between accurately representing the data and maintaining readability. 

To illustrate this, the code below generates two histograms for the same dataset, but with different bin sizes. The first histogram uses a smaller bin size, and the second one uses a larger bin size. As you can see, the histogram with the smaller bin size has more bars, each representing a narrower range of values. In contrast, the histogram with the larger bin size has fewer bars, each representing a wider range of values.

```python
import numpy as np
from scipy import stats
import plotly.graph_objects as go

# Generate a random dataset
np.random.seed(0) # scipy random seeds can be set with numpy
n = 500
data = stats.norm().rvs(size=n)

# Create a histogram with smaller bin size
fig1 = go.Figure(data=[go.Histogram(x=data, nbinsx=50)])
fig1.update_layout(title_text='Histogram with Smaller Bin Size')
fig1.show()

# Create a histogram with larger bin size
fig2 = go.Figure(data=[go.Histogram(x=data, nbinsx=10)])
fig2.update_layout(title_text='Histogram with Larger Bin Size')
fig2.show()
```

> In `px.histogram` (from `import plotly.express as px`) the parameter specifying the number of bins is `nbins` (not `nbinsx`); whereas, in `seaborn` and `matplotlib` (and hence `pandas`) the parameter is just `bins`. Here's some nifty code that would let you think about "widths" instead of number of bins, which is probably more useful sometimes.
>
> ```python
> import math
> bin_width = 0.5  # Choose your desired bin width
> nbinsx = math.ceil((data.max() - data.min()) / bin_width) # or `nbins` or `bins` if you're using another
> ```
>
> - Also... please note that the actual bin width in the histogram might not be exactly the same as the desired bin width due to the way that `Plotly` [automatically calculates the bin edges](https://community.plotly.com/t/histogram-bin-size-with-plotly-express/38927)

## Box plots and spread

> 4. [box plots](03-Data-Visualization#Box-plots-and-spread), [range](03-Data-Visualization#Box-plots-and-spread), [IQR](03-Data-Visualization#Box-plots-and-spread) and [outliers](03-Data-Visualization#Box-plots-and-spread)

In statistics, the term **range** refers to the difference between the highest and lowest values in a dataset, so it provides a simple measure of the spread (or dispersion or variability) of the data. For example, in the set {4, 6, 9, 3, 7}, the lowest value is 3, and the highest is 9. So the range is 9 - 3 = 6. 
> The range can sometimes be misleading if the highest and lowest values are extremely exceptional relative to most of the values in the data set, so be careful when considering the range of a numeric variable. For example, the range of salaries is not very representative of most "working class" salaries. 

The **interquartile range (IQR)** is another statistical measure of the spread (or dispersion or variability) of the data. It is defined as [the difference between the 75th and 25th percentiles of the data](https://statisticsbyjim.com/basics/interquartile-range/). In other words, the IQR includes the 50% of data points that are above the first and third quartiles (Q1 and Q3). The IQR is used to assess the variability where most of your values lie. Larger values indicate that the central portion of your data spread out further, while smaller values show that the middle values cluster more tightly.

A **box plot** is a graphical representation for characterizing the relative statistical spread (or dispersion or variability) of a numerical data distribution around its median. A box plot serves a similar purpose to as **histogram**, but it is a simpler alternative that provides a higher-level summary of numerical data. 

The "box" of a box plot is constructed out of the first (Q1) and third quartiles (Q3) separated by the median (which is the second quartile Q2); so, a box plot shows the the 25th, 50th, and 75th percentile with the "box" containing the middle 50% of the data. 

Box plots additionally draw **whiskers** (lines extending from the box) which typically extend to the most extreme data points within 1.5 times the interquartile range (IQR) of the first and third quartiles. 

- The **lower whisker** extends to the smallest data point within **1.5 * IQR** below the first quartile.
- The **upper whisker** extends to the largest data point within **1.5 * IQR** above the third quartile.

Data points that fall outside this range of the whiskers are typically termed **outliers** and are plotted as individual points. 

> The term "outliers" is only meant to be suggestive as outliers can mean different things in different context. In the context of a box plot, referring to the points beyond the extent of the whiskers is simply a technical term related to the definitional construction of a box plot.

![](https://360digit.b-cdn.net/assets/img/Box-Plot.png)

## Skew and Multimodality

> 5. [skew](03-Data-Visualization#skew-and-multimodality) and [multimodality](03-Data-Visualization#skew-and-multimodality) 
>     1. [mean versus median](03-Data-Visualization#skew-and-multimodality)
>     2. [normality and standard deviations](03-Data-Visualization#skew-and-multimodality)

[**Skewness**](https://stats.libretexts.org/Courses/Penn_State_University_Greater_Allegheny/STAT_200%3A_Introductory_Statistics_%28OpenStax%29_GAYDOS/02%3A_Descriptive_Statistics/2.06%3A_Skewness_and_the_Mean_Median_and_Mode) is a measure of the asymmetry of a data distribution. If skewness is present, then it's either left "negative" or right "positive" skew. 

- If the data is spread out more to the left direction then the skew is "negative"
- If the data is spread out more to the right direction then the skew is "positive"

![](https://s3.amazonaws.com/libapps/accounts/73082/images/Skeweness.jpg)

Skewness can be understood in terms of its affect on the relationship between the **mean** and **median**.

- The **mean** is the average of all data points in the dataset.
- The **median** is the middle point of a number set, in which half the numbers are above the median and half are below.

> The median is the 50th percentile of the data so it is the "second quartile" and can be denoted as Q2 similarly to the first and third quartiles Q1 and Q3 (which are the 25th and 75th percentile of the data).

In a **symmetric** distribution, the mean and median are the same. However, when data is skewed, the mean and median will differ.

- In a **positive skew** context, the mean will be greater than the median. This is because the mean is influenced by the high values in the tail of the distribution and gets pulled in the direction of the skew.
- In a **negative skew** context, the mean will be less than the median. This is because the mean is influenced by the low values in the tail of the distribution and gets pulled in the direction of the skew.

A simple way to remember this relationship is that the mean is 'pulled' in the direction of the skew.

**Modality** refers to the general natures of the "peaks" that may be present in a numerical data set. 
> These "peaks" are often informally referred to as "modes" but these should not be confused with the technical definition of **mode** which refers to the most common unique value in a data set.

![](https://miro.medium.com/v2/format:webp/0*m_Fd3Opt6L70LiYS.png)

Box plots are great for comparing the distributions of different groups of data; and, they also clearly indicate the presence of **skew** in a numerical data set; **but, be careful as box plots cannot represent multimodality in a data set; and, box plots do not indicate the amount of data they represent without the addition of an explicit annotations indicating this; while, Histograms, on the other hand, automatically give indications of both modality and sample size (through their "y-axis").**

![](https://www.simplypsychology.org/wp-content/uploads/box-plots-distribution.jpg)

An interesting "special case" of unimodal distributions is the **normal distribution**.
The figures below illustrates the quantiles (Q1 and Q3) for a normal distribution based on the corresponding boxplot.
The "sigma" σ character in the figure below signifies the **standard deviation** and the bottom most figure shows the percentiles that correspond to different multiplicative ranges of the standard deviation.

> The standard deviation is essentially defined specifically for normal distributions and its meaning is exactly clear in the context of normal distributions. It is somewhat harder to interpret the meaning of standard deviation the further from "normality" (the more "non normal") the distribution under consideration is. The greater the degree that a data distribution is skewed or non unimodal, the less clear it is what the meaning of the standard deviation is.  

![](https://jblomo.github.io/datamining290/slides/img/quartiles.png)

### Normal distributions 

In statistics, a normal distribution is a fundamental concept that describes how data points are spread out. It is also known as the Gaussian distribution, named after the mathematician Carl Friedrich Gauss. The normal distribution is essential because it often appears in real-world data and underpins many statistical methods.

**Characteristics of a Normal Distribution**

A normal distribution has several key features:

- Symmetry: The distribution is perfectly symmetric about the mean.
- Central Tendency: The mean, median, and mode of the distribution are all equal and located at the center of the distribution.
- Bell-Shaped Curve: The distribution forms a bell-shaped curve, with the highest point at the mean.
- Spread: The spread of the distribution is determined by the standard deviation. A larger standard deviation means the data is more spread out from the mean, while a smaller standard deviation means the data is more clustered around the mean.



# LEC Extensions

# LEC New Topics

## Populations and Distributions

> 1. populations [`from scipy import stats`](03-Data-Visualization#Populations-and-Distributions) (re: `stats.multinomial` and `np.random.choice()`) with `stats.norm` and `stats.poisson`

A **populations** is generally a theoretical idea that imagines the collection of all possible values the **observations** made for a **variable** could hypothetically be. It can refer to concrete group, such as "All Canadians" or "All UofT Students" or "All UofT International Students"; but, for all but very small populations, it would for all practical purposes not be possible to actually measure every **observation** in a **population** that could possibly occur.

In statistics, we often imagine a theoretical population as an idealized group of "all possible data points"; and, when we represent these mathematical or numerical, we call them **distributions**. We have already seen several of these **distributions**.

```python
# The first we saw was the Multinomial distribution
from scipy import stats
possible_observations = [1,2,3]
observation_frequency = [0.6,0.3,0.1]
n = 10 # sample size
Multinomial_distribution_object = stats.multinomial(possible_observations, p=observation_frequency)

# Two special cases of the Multinomial distribution are

# - the Binomial distribution
p = 0.5 # chance of getting a success ("1" as opposed to "0") <- doesn't need to be 0.5
Binomial_distribution_object = stats.multinomial([0,1], p=p, n=n) # or `stats.binom(p=p)`

# - The Bernoulli distribution
Bernoulli_distribution_object = stats.multinomial([0,1], p=p) # or `stats.bernoulli(p=p)`

# Some other distributions are the Normal, Poisson, and Gamma distributions 

μ = 0 # mean parameter
σ = 1 # standard deviation parameter
Normal_distribution_object = stats.norm(loc=μ, scale=σ)

λ = 1 # mean-variance parameter
Poisson_distribution_object = stats.poisson(loc=λ)

α # shape parameter
θ # scale parameter
Gamma_distribution_object = stats.gamma(a=α, scale=θ)
```

## Sampling 

> 2. [samples](03-Data-Visualization#Sampling) from populations (distributions) 

In statistics, sampling refers to the process of selecting a subset of individuals from a population. Ideally, the sample should be collected in such a way that it is representative of the population. This is because the primary purpose of sampling is to estimate the characteristics of the whole population when it is impractical or impossible to collect data from an entire population. Estimating population characteristics based on a sample is called inference.

```python
# Samples can be taken from the Multinomial distributions (and special cases) in the code above 
# using the "random variable samples" `.rvs(size=n)` method of the distribution objects

Multinomial_distribution_object.rvs(size=n)
Binomial_distribution_object.rvs(size=n)
Bernoulli_distribution_object.rvs(size=n)

# Which are correspondingly correspondingly equivalent to the following
np.random.choice(possible_observations, p=observation_frequency, size=n, replace=True)
np.random.choice([0,1], p=p, size=n, replace=True) 
np.random.choice([0,1], p=p, size=1, replace=True) 

# And this is analogously done for the Normal, Poisson, and Gamma distributions objects
Normal_distribution_object.rvs(size=n) 
Poisson_distribution_object.rvs(size=n) 
Gamma_distribution_object.rvs(size=n)
```

The $n$ samples from any of the above calls would typically be notated as $x_1, x_2, \cdots, x_n$.

## Statistics Estimate Parameters 

> 3. [statistics estimate parameters](03-Data-Visualization#Statistics-Estimate-Parameters)

The greek letters above (μ, σ, λ, α, and θ) are the parameters of their corresponding distributions. Parameters are the characteristics of population which we are trying to estimate by sampling. To make inferences on the population parameters, we estimate the parameter values with appropriately constructed statistics (which are mathematical functions of) samples. For example:

- The population mean of a Normal distribution μ is estimated by the sample mean 

  $$\bar x = \frac{1}{n}\sum_{n=1}^n x_i$$
- The population standard deviation of a normal distribution σ is estimated by the sample standard deviation 

  $$s = \sqrt{\frac{1}{n-1}\sum_{n=1}^n (x_i-\bar x)}$$
- The population mean of a Poisson distribution λ (which is also the variance poisson distribution population) is estimated by the sample mean $\bar x$ or the sample variance $s^2$
- And the shape α and scale θ parameters of a Gamma distribution can also be estimated, but the statistics for estimating these are a little more complicated than the examples above
