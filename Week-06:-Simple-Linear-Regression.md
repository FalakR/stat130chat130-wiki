**LEC 1 New Topics**

1. correlation association (is not causation)
2. y = ax + b
3. predictor, outcome, intercept and slope coefficients, and error terms
4. simple linear regression is a normal distribution

**TUT/HW Topics**

1. [`import statsmodels.formula.api as smf`](06-Simple-Linear-Regression#statsmodel-and-smf-ols`)
2. ["R-style" formulas and `smf.ols(y~x, data=df)`](06-Simple-Linear-Regression#R-Style-formulas-i-and-smf-ols-setup)
3. [using `smf.ols(y~x, data=df).fit().summary()`](https://github.com/pointOfive/STA130_ChatGPT/wiki/06-Simple-Linear-Regression#Fitting-the-Model-and-Extracting-Results)
    1. `.tables[1]`, `.params`, `.fittedvalues`, `.rsquared`
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

## `statsmodels` and `smf.ols`

The `Python` modules providing analogous functionality to  "base" `R` programming language are `statsmodels` and `scipy.stats`. The `statsmodels` module allows us to use and statistically analyze statistical predictive models, like **simple** and **multiple linear regression** models (which we'll begin exploring now), whreas the `scipy.stats` provides functionality related to statistical distributions (which we've started to explore previously) as well as some **hypothesis testing** capabilities (which we've not considered since we've instead be considering this topic using **simulation**).

> The `statsmodels` module is well-regarded for its comprehensive range of tools for statistical modeling and hypothesis testing which help us evaluate and understanding evidence about relationships between variables in many fields, such as economics, biology, and social sciences.

While there are a number of ways to use the `statsmodels` module, for STA130 we'll always its "formula" version.

```python
import statsmodels.formula.api as smf
```

The `smf.ols` function is the "ordinary" **least squares** approach to **regression** model fitting. The **ordinary least squares** methodology will be explained in further detail in the Week 06 TUT and it is the way we "fit regression models to data" in STA130.

## R-Style formulas I and `smf.ols` setup

To specify a **simple linear regression** in the `smf.ols` context for a `pandas DataFrame object` dataset `df` where the ("**independent**" **predictor**) column `"x"` variable is used to predict ("**dependent**" **outcome**) column `y` variable we use the "formula" `"y~x"` which corresponds to 

$$ Y_i = \beta_0 + \beta_1 x_i + \epsilon_i, \quad \text{ where } \quad \epsilon_i \sim N(0, \sigma^2)$$

```python
linear_specification = "y~x"
model_data_spefication = smf.ols(linear_specification, data=df)
```

The example above `"x"` and `"y"` are the names of the columns in `df`. If they respectively corresponded to "height" (`x`) and "weight" (`y`), then this model would be using "height" (`x`) to predict "weight" (`y`). If rather than `"x"` and `"y"` the column names were just "height" and "weight" directly, then we would instead just use 

```python
linear_specification = "weight~height"  # not `linear_specification = "y~x"`
```

The `linear_specification` is a so-called "R-style" formula which provide an intuitive and easy to read way to express the specification of the **linear form** of a **regression** model. 

> If the columns referenced by the **linear form** defined in `linear_specification` have spaces or "special characters" (like an apostrophe) > then a special "quote" `Q("...")` or `` syntax is required to reference them.  It might be easier and better for readability to just use `df.rename(...)` instead to change the name in these situations...
> 
> ```python
> linear_specification = 'Q("person\'s weight") ~ Q(person\'s height")' # or
> linear_specification = "`person\'s weight` ~ `person\'s height`"
> ```





The `"y~x"` formula syntax clearly defines the relationship between the dependent and independent variables, making it easy to translate statistical concepts into code.
   - **Syntax Breakdown**:
     - The formula is written as `y ~ x1 + x2 + ...`, where:
       - `y` is the dependent variable (the outcome you’re predicting).
       - `x1`, `x2`, etc., are the independent variables (the predictors).
     - The formula can also include interaction terms (e.g., `x1:x2`) and polynomial terms (e.g., `I(x1**2)`).
   - **Code Snippet**:
     ```python
     model = smf.ols('y ~ x', data=df)
     ```
   - **Example**:
     - **Simple Example**: If `y` is the weight of an individual and `x` is their height, the formula `'weight ~ height'` models weight as a function of height.
     - **Multiple Regression Example**: If you want to model weight as a function of height and age, you can use the formula `'weight ~ height + age'`.
     - **Interaction Example**: If you believe that the effect of height on weight depends on age, you might include an interaction term: `'weight ~ height * age'`, which expands to `'weight ~ height + age + height:age'`.
   - **Usage Scenarios**:
     - **Simple Linear Regression**: One predictor, one outcome (e.g., predicting salary based on years of experience).
     - **Multiple Linear Regression**: Multiple predictors, one outcome (e.g., predicting house prices based on size, location, and number of rooms).

<br></br>
## Fitting the Model and Extracting Results
3. **Model Fitting and Results Interpretations**
   - **Fitting the Model**:
     - Once you’ve specified your model with the formula, the next step is to fit the model to your data. Fitting the model involves estimating the coefficients that best describe the relationship between the dependent and independent variables.
   
   - **Code Snippet**:
     ```python
     results = model.fit()
     summary = results.summary()
     print(summary)
     ```
   - **Understanding the Output**:
     - **`fit()`**: This method estimates the parameters (coefficients) of the model using the data provided.
     - **`summary()`**: This method generates a detailed report that includes important statistical metrics and tests related to the model.
   <br></br>
   1. **Key Outputs Explained**:
     - **`.tables[1]`**:
       - This table contains detailed statistics about each coefficient in the model:
         - **Coefficient (`coef`)**: The estimated impact of each predictor on the dependent variable. For example, if the coefficient for height is 0.5, it means that each additional unit of height is associated with a 0.5 increase in weight.
         - **Standard Error (`std err`)**: Reflects the precision of the coefficient estimate. A smaller standard error indicates more confidence in the estimate.
         - **t-value**: The ratio of the coefficient to its standard error. It’s used to determine if the coefficient is significantly different from zero.
         - **p-value**: Indicates the probability of observing the data if the null hypothesis (that the coefficient is zero) were true. A small p-value (< 0.05) suggests that the coefficient is statistically significant.
     - **`.params`**:
       - This attribute provides the estimated coefficients directly, which are essential for interpreting the model.
       - **Example**:
         - If the output is:
           ```python
           const     2.0
           height    0.5
           dtype: float64
           ```
         - The intercept (`const`) is 2.0, meaning that when height is zero, the predicted weight is 2.0 (although this might not make sense in a real-world context).
         - The slope for height is 0.5, indicating that for each unit increase in height, the weight increases by 0.5 units.
     - **`.fittedvalues`**:
       - This attribute gives you the predicted `y` values for each observation in your dataset, based on the fitted model.
       - **Usage**:
         - You can use these values to compare against the actual observed `y` values to assess the model's accuracy.
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




