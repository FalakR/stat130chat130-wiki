**LEC 1 New Topics**

1. correlation association (is not causation)
2. y = ax + b
3. predictor, outcome, intercept and slope coefficients, and error terms
4. simple linear regression is a normal distribution

**TUT/HW Topics**

1. [`import statsmodels.formula.api as smf`](06-Simple-Linear-Regression#statsmodel`)
2. [`smf.ols`](06-Simple-Linear-Regression#smf-ols])
    1. ["R-style" formulas I](06-Simple-Linear-Regression#r-style-formulas-i])
    2. ["quoting" non-standard columns](06-Simple-Linear-Regression#quoting])
3. [`smf.ols("y~x", data=df).fit()` and `.params` $\hat \beta_k$ versus $\beta_k$](06-Simple-Linear-Regression#fitting-models)
    1. [`.fittedvalues`](06-Simple-Linear-Regression#fittedvalues)
    2. [`.rsquared` "variation proportion explained"](06-Simple-Linear-Regression#rsquared-variation-proportion-explained)
    3. [`.resid` residuals and assumption diagnostics](06-Simple-Linear-Regression#resid-residuals-and-assumption-diagnostics)
4. [smf.ols("y~x", data=df).fit().summary()` and `.tables[1]`](06-Simple-Linear-Regression#testing-on-average-linear-association)
    3. hypothesis testing no linear association "on average"

**LEC 2 New Topics / Extensions**

1. indicator variables
2. two sample group comparisons
3. normality assumption diagnostic
4. one, paired, and two sample tests
4. two sample permutation tests
5. two sample bootstrapping


# TUT/HW Topics


## `statsmodels`

The `Python` modules providing analogous functionality to "base" `R` statistical programming language are the `scipy.stats` and `statsmodels` modules. The `scipy.stats` module provides functionality related to statistical **distributions** (which we've previously started exploring), but it also provides some **hypothesis testing** capabilities (which we've not examined since we've instead been exploring this topic through **simulation**). The `statsmodels` module, on the other hand, combines the **distributional** and **hypothesis testing** concepts of the `scipy.stats` module to allow us to "fit" **linear regression** models and examine various aspects of the **fitted models**, including analyzing them using **hypothesis testing**.  

The `statsmodels` module is well-regarded for its comprehensive range of tools for statistical modeling and **hypothesis testing** which help us evaluate and understanding evidence about relationships between variables in many fields, such as economics, biology, and social sciences. While there are a number of ways to use the `statsmodels` module, for STA130 we'll always its "formula" version.

```python
import statsmodels.formula.api as smf
```


## `smf.ols` 

The "ols" in `smf.ols()` stands for "ordinary least squares". The **ordinary least squares** methodology will be explored further in HW06, but suffice it to say for now that it is "how linear regression models are fit to data" in STA130. To specify a **simple linear regression** model in the context of a `pandas DataFrame object` dataset `df` where the ("**independent**" **predictor**) column `x` variable is used to predict the ("**dependent**" **outcome**) column `y` variable, use the "formula" `"y ~ x"` which corresponds to 

$$Y_i \sim \mathcal{N}(\beta_0 + \beta_1 x_i , \sigma^2) \quad \text{ or } \quad  Y_i = \beta_0 + \beta_1 x_i + \epsilon_i, \quad \text{ where } \quad \epsilon_i \sim \mathcal{N}(0, \sigma^2)$$

```python
linear_specification = "y ~ x"  # automatically assumes the "Intercept" beta0 will be included
model_data_specification = smf.ols(linear_specification, data=df)
```

In the example above `x` and `y` would be names of columns in `df`. If `x` and `y` in `df` respectively corresponded to "parents average height" and "child's height" (as in Francis Galton's [classical study](https://en.wikipedia.org/wiki/Regression_toward_the_mean#Discovery) which popularized the concept of "regression to the mean" which all **regression** methodologies are now named after), then `model_data_specification` indicates using "parents average height" (`x`) to predict "child's height" (`y`). If rather than `x` and `y` the column names were just `parent_height` and `child_heigh"` directly, then we would instead just use 

```python
import pandas as pd
import statsmodels.formula.api as smf

# Simplified version of Galton's data: this dataset is small and simplified for illustrative purposes
# The actual dataset used by Galton was much larger and included more precise measurements and other variables; however,
# this should give you a sense of the type of data Galton worked with
francis_galton_like_data = {
    'parent_height': [70.5, 68.0, 65.5, 64.0, 63.5, 66.5, 67.0, 67.5, 68.0, 70.0,
                      71.5, 69.5, 64.5, 67.0, 68.5, 69.0, 66.0, 65.0, 67.5, 64.0],
    'child_height': [68.0, 66.5, 65.0, 64.0, 63.0, 65.5, 67.0, 67.5, 68.0, 70.0,
                     70.5, 69.5, 63.5, 66.0, 68.5, 69.0, 66.0, 64.5, 67.5, 63.5]
}
francis_galton_df = pd.DataFrame(francis_galton_like_data)

linear_specification = "child_height ~ parent_height"  # not `linear_specification = "y~x"`
model_data_specification = smf.ols(linear_specification, data=francis_galton_df)
```

### R-Style formulas I

The `linear_specification` is a so-called "R-style formula" which provides a simple way to specify the **linear form** of a **regression** model. A "formula" of the form `"y ~ x"` automatically assumes the "Intercept" $\beta_0$ will be included in the model specification, with **outcome** "y" on the left and **predictors** "x" on the right, as described in the previous section. The "outcome on the left and predictors on the right" formulation becomes increasingly intuitive in the context of **multiple** (as opposed to **simple**) **linear regression**. The `"child_height ~ parent_height"` is a **simple linear regression** specification because there is a single **predictor** variable; whereas, `"child_height ~ parent_height + nationality"` is a **multiple linear regression** specification because there is more than one **predictor** variable. We will return to **multiple linear regression** in Week 07, so consideration of this topic can be safely postponed for now.

### Quoting

If the columns referenced by the **linear form** defined in `linear_specification` have "spaces" or "special characters" (like an apostrophe) then a special "quote" `Q("...")` or `` syntax is required to reference them.  It might be easier and better for readability to just use `df.rename(...)` instead to change the name in these situations...

```python
linear_specification = 'Q("child\'s height") ~ Q("parents average height")' # or
linear_specification = "`child\'s height` ~ `parents average height`"
```


## Fitting Models 

After specifying the **linear form** and the dataset of a **regression model** with `smf.ols(...)` the model is then **estimated** or "fit" using the `smf.ols(...).fit()` **method** which provides the **fitted model** (**estimate** of the **theoretical model**) from the dataset 

$$\hat y_i = \hat \beta_0 + \hat \beta_1 x_i \quad \textrm{ (in the case of simple linear regression)}$$

```python
data_fitted_model = model_data_specification.fit()  # estimate model coefficients and perform related calculations
data_fitted_model.params  # the estimated model coefficients beta0 and beta1 (in the case of simple linear regression)
```

For `linear_specification = "child_height ~ parent_height"` 

- The rows of `data_fitted_model.params` will be `Intercept` ($\hat \beta_0$) and `parent_height` ($\hat \beta_1$)
- The `Intercept` $\hat \beta_0$ is automatically assumed to be a part of the **linear form** specification, and the name of the **fitted slope coefficient** $\hat \beta_1$ will match that of the "**independent**" **predictor** variable used in the model
    - The **fitted slope coefficient** $\hat \beta_1$ is an **estimate** of the average change in the "**dependent**" **outcome** variable for a "one unit" increase of the "**independent**" **predictor** variables
    - The **fitted intercept coefficient** $\hat \beta_0$ must then be the **estimate** of the average of the "**dependent**" **outcome** variable when the "**independent**" **predictor** variable has the value $0$
        - $\hat \beta_0$ may sometimes have a sensible interpretation, but often (as in the case of `parent_height` for which the "average parents height of $0$" could never happen) it doesn't really correspond to anything meaningful in the real world
        - $\hat \beta_1$ is really what captures and represents the best estimate of the **linear relationship** between `x` and `y` based on the data you have since this **estimates** the "average change in the **outcome** for a one-unit increase in the **predictor**"
- The **fitted model** is the "straight line" that (with respect to **ordinary least squares** methodology) "best" reflects the **linear association** observed in the dataset between the "**independent**" **predictor** and "**dependent**" **outcome** variables corresponding to the hypothesized **linear association** of the **theoretical model**

    > The **fitted** [(simple) linear regression] model $\hat y_i = \hat \beta_0 + \hat \beta_1 x_i$ **estimates** the **theoretical** [(simple) linear regression] model
    >
    > $$Y_i \sim \mathcal{N}(\beta_0 + \beta_1 x_i , \sigma^2) \quad \text{ or } \quad Y_i = \beta_0 + \beta_1 x_i + \epsilon_i, \quad \text{ where } \quad \epsilon_i \sim \mathcal{N}(0, \sigma^2)$$
    >
    > If the **theoretical model**  was "true" with $\beta_1 = 2$, then an estimated coefficient of $\hat \beta = 1.86$ would be the result of sample to sample variability. A different sample might produce an estimated coefficient of $\hat \beta = 2.18$. In practice, $\hat \beta_1$ is used to make inferences about $\beta_1$.

The **estimated** $\hat y_i = \hat \beta_0 + \hat \beta_1 x_i$ line specified by `data_fitted_model.params` can be visualized using `px.scatter(..., trendline="ols"`) since "ols" indicates the same **ordinary least squares** fit of the data used by `smf.ols(...).fit()`. 

```python
import plotly.express as px
# Create the scatter plot with the OLS fit line
px.scatter(francis_galton_df, x='parent_height', y='child_height', 
           title="Simple Linear Regression", trendline="ols")
```


### `.fittedvalues` 

The **fitted model coefficients** from the `.params` **attribute** specifying the $\hat y_i = \hat \beta_0 + \hat \beta_1 x_i$ formula define the **fitted values** (and other **predictions**) of the **fitted model**, but the **fitted values** are also immediately available from the `.fittedvalues` **attribute**.  

```python
data_fitted_model.fittedvalues  # model "predicted" *fitted values*, which for the current example is equivalent to 
# y_hat = data_fitted_model.params[0] + data_fitted_model.params[1] * francis_galton_df['parent_height']
```

The $\hat y_i$ are the **fitted model predictions** of the "expected average value" of $y_i$ for a given value of $x_i$. In the case of the **fitted values**, the "expected average value" $\hat y_i$ "predictions" are both "based on the observed $y_i$" and "made for the observed $y_i$". This is why "predictions" has been put in quotes with respect to **fitted values**. The **fitted values** are not really proper **predictions** since they are "predictions" of the same $(y_i, x_i)$ pairs that are used to **estimate** the **fitted model** $\hat y_i = \hat \beta_0 + \hat \beta_1 x_i$ which defines the **predictions** in the first place. 

> The notion that **fitted values** uses $(y_i, x_i)$ to make their own "predictions" $(\hat y_i, x_i)$ raises the topic of **in sample** versus **out of sample prediction**.  We will return to this topic and the use of the `.predict()` **method** (as opposed to the `.fittedvalues` **attribute** in the context of **multiple linear regression** in Week 07.


### `.rsquared` "variation proportion explained"

The **squared correlation** between the "predicted" **fitted values** $\hat y_i$ and observed "**dependent**" **outcomes** $y_i$ is referred to as **R-squared**, and can be accessed through the `.rsquared` **attribute** of a **fitted model**. While the mathematical details are beyond the scope of STA130, the **R-squared** measures "the proportion of variation explained by the model". So, e.g., an R-squared value of 0.7 means that 70% of the "variation" in the "**dependent**" **outcome**  variables $y_i$ is "explained" by the model. 

```python
import numpy as np

# This measures "the proportion of variation explained by the model"
data_fitted_model.rsquared  # model **R-squared**, which for the current example is equivalent to 
# np.corr_coef(data_fitted_model.fittedvalues, francis_galton_df['child_height'])[0,1]**2
```


### `.resid` residuals and assumption diagnostics

Analogously to **fitted values**, the **residuals** of the **fitted model** $\textrm{e}_i = \hat \epsilon_i = y_i - \hat y_i$ can be accessed through the `.resid` **attribute**.

```python
data_fitted_model.resid
```

The **residuals** of the **fitted model** $\textrm{e}_i = \hat \epsilon_i = y_i - \hat y_i$ **estimate** the **error terms** of the **theoretical model** $\epsilon_i$. As such, **residuals** can be used as **diagnostically** to assess **linear regression model assumptions**, such as **normality**, **heteroskedasticity**, and **linearity**. 


```python
# If *residuals* do not appear approximately normally distributed, the "normality assumption" is implausible
residual_normality = px.histogram(data_fitted_model.resid, title="Histogram of Residuals")
residual_normality.update_layout(xaxis_title="Does this appear normally distributed?", yaxis_title="Count")
residual_normality.show()
```

```python
# If *residuals* heights appear to systematically change over the range of y-hat, the "heteroskedasticity assumption" is implausible
residual_heteroskedasticity = px.scatter(x=data_fitted_model.fittedvalues, y=data_fitted_model.resid, 
                                         title="Does this systematically change over y-hat?")
residual_heteroskedasticity.update_layout(xaxis_title="Fitted Values", yaxis_title="Residuals")
residual_heteroskedasticity.add_hline(y=0, line_dash="dash", line_color="red")
residual_heteroskedasticity.show()
```

```python
# If y versus y-hat does not follow the "linearity" of the "y=x" line, the "linearity assumption" is implausible
fitted_values = data_fitted_model.fittedvalues
residual_linearity = px.scatter(x=fitted_values, y=francis_galton_df['child_height'], 
                                title="Is this relationship 'linear' in a 'y=x' way?")
residual_linearity.update_layout(xaxis_title="Fitted Values", yaxis_title="Outcome")
residual_linearity.add_scatter(x=[min(fitted_values), max(fitted_values)], y=[min(fitted_values), max(fitted_values)],
                               mode='lines', line=dict(color='red', dash='dash'), name="The 'y=x' line")
residual_linearity.show()
```


## Testing "On Average" Linear Association

The information needed for statistical **hypothesis testing** analysis of the **fitted model** is accessed using the `.summary()` **method**.

```python
data_fitted_model.summary()  # generates a detailed report of statistical estimates and related information; but, ... 
data_fitted_model.summary().tables[1]  # what we need for *hypothesis testing* is contained in its `.tables[1]` attribute
```

The `coef` column of `data_fitted_model.summary().tables[1]` corresponds to `data_fitted_model.params` discussed above; but, the additional `P>|t|` column includes the **p-values** associated with a **hypothesis test** for each of the **coefficients**.
- Specifically, the values in `P>|t|` are **p-values** for a **null hypothesis** that the **true coefficient value** is zero.
- In the case of the **true slope coefficient** $\beta_1$ representing the "average change in the **outcome** for a one-unit increase in the **predictor**" 
    - this **null hypothesis** corresponds to the assumption of "**no linear association** between the **outcome** and **predictor variables** 'on average'".
- Formally, this "no linear relationship" **null hypothesis** would then be stated as $H_0: \beta_1 = 0$ where $\beta_1$ is the hypothesized **parameter** value which the associated **statistic** $\hat \beta_1$ **estimates**
- And rarely would there be interested in $H_0: \beta_0 = 0$ since an **intercept** value of $0$ does not generally have a meaningful interpretation, and the context of **linear regression** is typically just concerned with evaluating the relationship between the **outcome** and **predictor variables**.

> Recalling the definition of a **p-value**, in the current context a **p-value** would be "the probability of that a dataset would result in an **estimated coefficient** as or more extreme than the **observed coefficient** if the **null hypothesis** (that the **true coefficient value** is zero) were true". The familiar "as or more extreme" definition of the **p-value** is reflected in the `P>|t|` column name, which can be further seen to indicated a "two-sided" hypothesis test specification. 

A small **p-value** for the **slope coefficient** $\hat \beta_1$ therefore suggests the implausibility of the **null hypothesis** and hence suggests that there actually likely is "a **linear association** 'on average' between the outcome and predictor variables" with $\hat \beta_1$ capturing the "on average" change of this **linear association**. 

> The `std err` and `t` columns relate to the **sampling distribution** "under the null", and the `[0.025 0.975]` columns provide **inference** for **estimated coefficients** in the form of **confidence intervals**, but all of this is beyond the scope of STA130 so we'll just concern ourselves with appropriately using the **p-values** from the `P>|t|` column. 