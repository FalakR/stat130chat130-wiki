**LEC 1 New Topics**

1. correlation association (is not causation)
2. y = ax + b
3. predictor, outcome, intercept and slope coefficients, and error terms
4. simple linear regression is a normal distribution

**TUT/HW Topics**

1. [`import statsmodels.formula.api as smf`](06-Simple-Linear-Regression#statsmodel`)
2. [`smf.ols` and "R-style" formulas](06-Simple-Linear-Regression#smf-ols-and-r-style-formulas-i])
3. [`smf.ols(y~x, data=df).fit().summary()`](06-Simple-Linear-Regression#fitting-models)
4. [`.tables[1]`, `.params`, `.fittedvalues`, `.rsquared`](06-Simple-Linear-Regression#fitted-values and residuals)
    2. $\hat \beta_k$ versus $\beta_k$
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

The `Python` modules providing analogous functionality to  "base" `R` programming language are `statsmodels` and `scipy.stats`. The `statsmodels` module allows us to use and statistically analyze statistical predictive models, like **simple** and **multiple linear regression** models (which we'll begin exploring now), whereas the `scipy.stats` provides functionality related to statistical distributions (which we've started to explore previously) as well as some **hypothesis testing** capabilities (which we've not considered since we've instead be considering this topic using **simulation**).

The `statsmodels` module is well-regarded for its comprehensive range of tools for statistical modeling and hypothesis testing which help us evaluate and understanding evidence about relationships between variables in many fields, such as economics, biology, and social sciences. While there are a number of ways to use the `statsmodels` module, for STA130 we'll always its "formula" version.

```python
import statsmodels.formula.api as smf
```

## `smf.ols` and R-Style formulas I

The `smf.ols()` function is the "ordinary" **least squares** approach to **regression model fitting**. The **ordinary least squares** methodology will be explained in further detail in the Week 06 TUT but suffice it say it is the way we "fit regression models to data" in STA130. To specify a **simple linear regression** for a `pandas DataFrame object` dataset `df` where the ("**independent**" **predictor**) column `"x"` variable is used to predict ("**dependent**" **outcome**) column `y` variable we use the "formula" `"y~x"` which corresponds to 

$$Y_i = \beta_0 + \beta_1 x_i + \epsilon_i, \quad \text{ where } \quad \epsilon_i \sim N(0, \sigma^2)$$

```python
linear_specification = "y ~ x"  # automatically assumes the "Intercept" beta0 will be included
model_data_spefication = smf.ols(linear_specification, data=df)
```

In the example above `x` and `y` would be names of columns in `df`. If `x` and `y` in `df` respectively corresponded to "parents average height" and "child's height" (as in Francis Galton's [classical study](https://en.wikipedia.org/wiki/Regression_toward_the_mean#Discovery) which popularized the notion of "regression to the mean" from which the term **regression** was derived), then `model_data_spefication` would be using "parents average height" (`x`) to predict "child's height" (`y`). If rather than `x` and `y` the column names were just `parent_height` and `child_heigh"` directly, then we would instead just use 

```python
import pandas as pd
import statsmodels.formula.api as smf

# Simplified version of Galton's data: this dataset is small and simplified for illustrative purposes
# The actual dataset used by Galton was much larger and included more precise measurements and other variables; however,
# this should give you a sense of the type of data Galton worked with
data = {
    'parent_height': [70.5, 68.0, 65.5, 64.0, 63.5, 66.5, 67.0, 67.5, 68.0, 70.0,
                      71.5, 69.5, 64.5, 67.0, 68.5, 69.0, 66.0, 65.0, 67.5, 64.0],
    'child_height': [68.0, 66.5, 65.0, 64.0, 63.0, 65.5, 67.0, 67.5, 68.0, 70.0,
                     70.5, 69.5, 63.5, 66.0, 68.5, 69.0, 66.0, 64.5, 67.5, 63.5]
}
df = pd.DataFrame(data)

linear_specification = "child_height ~ parent_height"  # not `linear_specification = "y~x"`
model_data_spefication = smf.ols(linear_specification, data=df)
```

The `linear_specification` is a so-called "R-style" formula which provide an intuitive and easy to read way to express the specification of the **linear form** of a **regression** model. If the columns referenced by the **linear form** defined in `linear_specification` have "spaces" or "special characters" (like an apostrophe) then a special "quote" `Q("...")` or `` syntax is required to reference them.  It might be easier and better for readability to just use `df.rename(...)` instead to change the name in these situations...

```python
linear_specification = 'Q("child\'s height") ~ Q("parents average height")' # or
linear_specification = "`child\'s height` ~ `parents average height`"
```


## Fitting Models 

After specifying the data and the **linear form** of the **regression model** in `smf.ols(...)` the model is then "fit" using the `smf.ols(...).fit()` **method** which provides the **fitted model** estimated from the dataset 

$$\hat y_i = \hat \beta_0 + \hat \beta_1 x_i$$

```python
model_data_fit = model_data_spefication.fit()  # estimate model parameters (coefficients) based on `df` and perform related calculations
model_data_fit.params  # the estimate model parameters (coefficients) based on `df`
```

For `linear_specification = "child_height ~ parent_height"` the rows of `model_data_fit.params` will be `Intercept` and `parent_height`. The `Intercept` is automatically assumed to be a part of the **linear form** specification, and the **fitted slope coefficient** names will match those of "**independent**" **predictor** variables used in the model. The **fitted slope coefficient** estimates the average change in the "**dependent**" **outcome** variable for a "one unit" increase of the "**independent**" **predictor** variables. The **fitted intercept coefficient** must be then the average of the "**dependent**" **outcome** variable when the "**independent**" **predictor** variable has the value $0$.  The **fitted intercept coefficient** may sometimes have a sensible interpretation, but often (as in the case of `parent_height`) it doesn't really correspond to anything meaningful in the real world. 

The **fitted model coefficients** can be used in the formula of the **fitted model** to create **fitted values** (and other predictions). The **fitted values** are immediately available from the **fitted model** though the `.fittedvalues` **attribute**.

```python
model_data_fit.fittedvalues  # model fitted value "predictions"
```


## Analyzing Models 

The **fitted model** can be analyzed statistically using the `.summary()` **method** on the fitted model object.

```python
model_data_fit.summary()  # provide information for statistical analysis 
# `summary()` generates a detailed report that includes the statistical estimates, tests, and other information related to the model fit
```

Fitting the model with `.fit()` involves estimating the **coefficients** that best describe the relationship between the "**independent**" **predictor** and "**dependent**" **outcome** variables based on the **ordinary least squares** methodology. The related information needed for statistical analysis of these results is accessed by using `.summary()` on the **fitted model object**. 

## Fitted values and residuals


The specific parts of the `.summary()` out that are of interest now are 

- hypothesis testing information related the estimated **coefficients** `model_data_fit.summary().tables[1]`<br>

  - `P>|t|` (the **p-value**) is the probability of that a dataset would result in an **estimated coefficient** as or more extreme than the **observed coefficient** if the **null hypothesis** (that the **true coefficient value** is zero) were true, with a small **p-value** therefore suggesting the implausibility of the **null hypothesis**
      
       - the `std err`, `t`,  and `[0.025 0.975]` are additional details and uses related to the **p-value** which are beyond the scope of STA130

The **fitted values** and **residuals** are accessed with `model_data_fit.fittedvalues` and `model_data_fit.residuals`, both of which can be used in connection with the actual observed "**dependent**" **outcome** variable (`df.y` or `childs_height` or `Q("child\'s height")` in the case of the ongoing example), and the **squared correlation** between the predicted **fitted values** and observed "**dependent**" **outcome** variable outcomes, which measures the "proportion of variation explained", can be gotten using `model_data_fit.rsquared`. 

     - **`.fittedvalues`**:
       - This attribute gives you the predicted `y` values for each observation in your dataset, based on the fitted model.
       - **Usage**:
         - You can use these values to compare against the values to assess the model's accuracy.
         - **Example**:
           ```python
           predicted_weights = results.fittedvalues
           ```
           - This would give you the predicted weights for each individual in your dataset.
     - **`.rsquared`**:
       - The R-squared value measures the proportion of variability in the dependent variable that is explained by the independent variable(s).
       - **Interpretation**:
         - An R-squared value of 0.7 means that 70% of the variability in the dependent variable is explained by the model. However, it’s important to note that a higher R-squared doesn’t always mean a better model, as it could indicate overfitting, especially in models with many predictors.
       - **Caveat**:
         - In models with multiple predictors, you might also consider the Adjusted R-squared, which adjusts for the number of predictors and helps prevent overfitting.\\
   <br></br>
   2. **$\hat\beta_k$ versus $\beta_k$**:
     - **$\hat\beta_k$**: The estimated coefficient from your sample data, representing the best estimate of the relationship between `x` and `y` based on the data you have.
     - **$\beta_k$**: The true, but unknown, population coefficient. In practice, $\hat \beta_k$ is used to make inferences about $\beta_k$.
     - **Example**:
       - Suppose the true effect of study hours on exam scores is $\beta = 2$. If your estimated coefficient is $\hat \beta = 1.8$, your estimate is close, but there’s some error due to the variability in your sample.

   <br></br>

   3. **Hypothesis Testing**:
     - **Null Hypothesis ($H_0$)**: In the context of linear regression, the null hypothesis typically states that there is no linear relationship between the independent and dependent variables ($\beta_1 = 0$).
     - **Alternative Hypothesis ($H_a$)**: The alternative hypothesis suggests that there is a linear relationship ($\beta_1 \neq 0$).
     - **Interpreting the p-value**:
       - A p-value less than 0.05 generally suggests rejecting the null hypothesis, indicating a significant linear relationship.
     - **Practical Example**:
       - If you’re testing whether the number of study hours (`x`) affects exam scores (`y`), and the p-value is 0.02, you would reject the null hypothesis, concluding that study hours do significantly affect exam scores.

     - **Additional Considerations**:
       - **One-sided vs. Two-sided Tests**: 
         - A two-sided test examines whether the coefficient is significantly different from zero in either direction (positive or negative).
         - A one-sided test focuses on only one direction (e.g., testing whether the coefficient is greater than zero).
       - **Confidence Intervals**:
         - Confidence intervals provide a range within which we expect the true coefficient to lie. For example, a 95% confidence interval might be [1.5, 2.5], meaning that there’s a 95% chance the true coefficient lies within this range.




