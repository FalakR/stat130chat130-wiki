**TUT/HW Topics**

1. importing libraries... like [`pandas`](01-Data-Summarization#import)
2. loading data... with [`pd.read_csv()`](01-Data-Summarization#read_csv)
3. counting missing values... with [`df.isna.sum()`](01-Data-Summarization#Missingness-I)
4. observations (rows) and variables (columns)... [`df.shape`](01-Data-Summarization#Variables-and-Observations) and [`df.columns`](01-Data-Summarization#Variables-and-Observations)
5. numeric versus non-numeric... [`df.describe()`](01-Data-Summarization#Types-I) and [`df.value_counts()`](01-Data-Summarization#Types-I)
6. removing missing data... with [`df.dropna()`](01-Data-Summarization#Missingness-II) and [`del df['col']`](01-Data-Summarization#Missingness-II)
7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01-Data-Summarization#Grouping-and-Aggregation)

**LEC Extensions**

2. parameters and arguments
3. [boolean values and coercion](01-Data-Summarization#Boolean-Values-and-Coercion)
4. ~function side-effects~
5. 
    1. `.dtypes` and `.astype()`  
    2. dictionary `dict()` objects
    3. statistic calculation functions

**LEC New Topics**

1. sorting and (0-based) indexing
2. subsetting and boolean selection

**Out of Scope**

1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as how to handle missing values using more advanced methods than just "ignoring" or "removing" them
4. ...further "data wrangling topics" such as "joining" and "merging"; "pivoting", "wide to long", and "tidy" data formats; etc.

# TUT/HW Topics

## import

> 1. importing libraries... like [`pandas`](01-Data-Summarization#import)

```python
import pandas as pd # aliasing usage is `pd.read_csv()`
import pandas # original name usaged is `pandas.read_csv()`
from pandas import read_csv # funtion only import usage is `read_csv()`
```

# read_csv

> 2. loading data... with [`pd.read_csv()`](01-Data-Summarization#read_csv)

```python
import pandas as pd

# read a local file
pd.read_csv("./path/to/filename.csv") # "." denotes the "current directory"
# the "csv" file "filename.csv" is in subfolders of the "current directory"

# read an online file
pd.read_csv("https://www.url.com/path/to/filename.csv")
```

**Using URL links**

When accessing an online file, you must link to an actual "raw" `.csv` file.
- This link "looks like" an actual `.csv` file, **but it is not**: [https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv](https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv)
    - If you follow the link you'll see it's some sort of webpage that visualizes a `.csv` file, but it is not the actual "raw" `.csv` file.
- Here is the link to the actual "raw" `.csv` file that can be found on the above page: [https://raw.githubusercontent.com/mwaskom/seaborn-data/master/titanic.csv](https://raw.githubusercontent.com/mwaskom/seaborn-data/master/titanic.csv)
   - If you follow the link you'll see it's now the actual "raw" `.csv` file.

```python
# will not work
failing_url = "https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv"
titanic_df = pd.read_csv(failing_url) 
# does not access an actual "raw" `.csv` file

# will work
working_url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/titanic.csv"
titanic_df = pd.read_csv(working_url) 
# links directly to an actual "raw" `.csv` file
```

**Extensions**

Here are some parameters of the `pd.read_csv` method that can be used to address somewhat common special cases...
- `encoding` for different character sets
- `sep` for different file types
- `skiprows` and `names` to control column names (and see more [here](https://note.nkmk.me/en/python-pandas-read-csv-tsv/))
 

# Missingness I

> 3. counting missing values... with [`df.isna.sum()`](01-Data-Summarization#Missingness-I)

In Python we generally use the pandas library to count missing values.
Specifically, we use the `.isna()` (or equivalently `isnull()` since it is just an **alias** for `.isna()`) methods followed by the `.sum()` method. 
> Sometimes we may additionally use `axis=1` and `.any()`.

Here’s a quick example of how this is done:

```python
import pandas as pd
import numpy as np

# Create a DataFrame with some missing values
df = pd.DataFrame({
    'A': [1, 2, np.nan, 4],
    'B': [np.nan, 2, 3, 4],
    'C': [1, np.nan, np.nan, 4]
})

# Count the number of missing values in each column
missing_values_per_column = df.isna().sum()

# Count the total number of missing values in the DataFrame
total_missing_values = df.isna().sum().sum()

# Count the number of rows with at least one missing value
rows_with_missing_values = df.isna().any(axis=1).sum()

# Count the number of missing values in each row
missing_values_per_row = df.isna().sum(axis=1)

print(missing_values_per_column)
print("Total missing values:", total_missing_values)
print("Number of rows with at least one missing value:", rows_with_missing_values)
print(missing_values_per_row)
```

For more details regarding "boolean values and coercion", see [01.3.i.LEC](01.3.i.LEC).

# Variables and Observations

> 4. observations (rows) and variables (columns)... [`df.shape`](01-Data-Summarization#Variables-and-Observations) and [`df.columns`](01-Data-Summarization#Variables-and-Observations)

```python
import pandas as pd

titanic_df = pd.read_csv('https://raw.githubusercontent.com/mwaskom/seaborn-data/master/titanic.csv')

titanic_df.shape # number of rows and columns 
# this is an "attribute" not a "method" so it does not have end with parenthesis `()`

titanic_df.columns # a list of the names of the columns 
# also an attribute...

# You can rename columns as desired...
# This would make columns named "a" and "b" uppercase
df.rename(columns={'a': 'A', 'b': 'B'}, inplace=True)

# The `{'a': 'A', 'b': 'B'}` is a "dictionary" `dict()` data type object.
# In dictionary parlance, the lowercase letters in the above example are "keys" 
# and the uppercase letters are the "values" which correspond the to the "keys"

# In this case, the code specifies the columns to be renamed (the keys)
# and what their new names should be (the values).
```

**Observations** are usually organized as rows in a dataset. Each observation represents a single entity upon which data has been measured and recorded. For example, if you’re analyzing a dataset of patients in a hospital, each patient would be an observation.

**Variables** are the different things that can be measured and recorded for each entity, and thus usually correspond to the columns in a dataset. So, the **observation** is comprised of all the values in the columns (or **variables**) of a dataset. 

> These concepts are discussed in more detail, [here](https://www.statology.org/observation-in-statistics/).

We're likely to intuitively think of an "observation" as a single value, and we often analyze the values of a single column of data which tends to further bolsters the concept that an "observation" can be thought of as a single value. Since an "observation" refers to whatever set of variables we're considering, there is not a problem with this simplified view of things at the moment. 

Variables can be [numerical (quantitative) or categorical (qualitative)](https://uniskills.library.curtin.edu.au/numeracy/statistics/data-variable-types/). For instance, a [patient dataset](http://www.statistics4u.info/fundstat_eng/cc_variables.html) might include the variables of age, weight, blood type, etc.

> Missing values in datasets need to be handled carefully during analysis because they can affect the results. Different statistical methods and tools have their own ways of dealing with missing values, either by ignoring, removing them, or filling them in with estimated values.

# Types I

> 5. numeric versus non-numeric... [`df.describe()`](01-Data-Summarization#Types-I) and [`df.value_counts()`](01-Data-Summarization#Types-I)

The `.describe()` method provides descriptive statistics that summarize **numerical data** in terms of its location (or position) and scale (or spread). Its provides mean, standard deviation, median (50th percentile), quartiles (25th and 75th percentile), and minimum and maximum values.

> The statistic calculations are based on the non-missing values in the data set, and the number of such non-missing values used to calculate the statistics is given by the "count" value returned from `.describe()`.

The `[column_name].value_counts()` method counts the number of each unique value in a column (named `column_name`). This method is used for **categorical data** to understand the distribution of categories within a feature. It does not count missing values by default, but it can include them in the counts by using `dropna=False`.

Here’s a demonstration using the Titanic dataset.

```python
import pandas as pd

# Load the Titanic dataset 
titanic_df = pd.read_csv('https://raw.githubusercontent.com/mwaskom/seaborn-data/master/titanic.csv')

# Use df.describe() to get descriptive statistics for numerical features
numerical_stats = titanic_df.describe()

# Use df.value_counts() on a categorical column, for example, 'Embarked'
embarked_counts = titanic_df['embarked'].value_counts()

print(numerical_stats)
print(embarked_counts)
```

# Missingness II

> 6. removing missing data... with [`df.dropna()`](01-Data-Summarization#Missingness-II) and [`del df['col']`](01-Data-Summarization#Missingness-II)

```python
# Assuming 'df' is your DataFrame

# Drop rows with any missing values
df.dropna(inplace=True)

# Drop rows that have all missing values
df.dropna(how='all', inplace=True)

# Keep rows with at least 2 non-missing values
df.dropna(thresh=2, inplace=True)

# Remove an entire column named 'col' from df
del df['col']

# Or only drop columns that have any missing values
df.dropna(subset=['col1', 'col2'], axis='columns', inplace=True)
```

The order in which you remove rows or columns with missing values to some degree determines the number of non-missing values that are "thrown away" when rows and columns are removed... so proceed intentionally and cautiously to when removing data so you don't "unnecessarily" through away data when you're removing rows and columns from a dataset.

# Grouping and Aggregation

> 7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01-Data-Summarization#Grouping-and-Aggregation)

Grouping and aggregation are powerful concepts in data analysis, particularly with pandas in Python. They allow you to organize data into groups and then perform operations on those groups to extract insights.

**Grouping** refers to the process of organizing data into groups based on some criteria. In pandas, this is done using the `.groupby()` method. When you group data, you’re essentially splitting the DataFrame into smaller chunks based on unique values of a specified key column or columns. For example, `df.groupby("col1")` will create a group for each unique value in `"col1"`.

**Aggregation** refers to computing summaries of each of the groups once they're separated. Some examples of aggregation functions are the `.sum()`, `.mean()`, `.min()`, `.max()`, and `.count()` methods. When you use `df.groupby("col1")["col2"].describe()` you're doing all of these at once (as well as `np.quantile([25,50,75])`).

> After `df.groupby("col1")` groups the data by unique values in `"col1"`, the subsequent `["col2"]` selects the `"col2"` column from the data, and then for each group the concluding `.describe()` computes the summary statistics for `"col2"` within each group. Namely, the count, mean, standard deviation, minimum, 25% (first quartile), 50% (median), 75% (third quartile), and maximum values for `"col2"` within each group. 

Missing values in the grouping column (`"col1"`) will result in a separate group if there are any, while the `.describe()` method automatically excludes missing values when calculating descriptive statistics for `"col2"`.

**LEC Extensions**

# Boolean Values and Coercion

> 3. [boolean values and coercion](01-Data-Summarization#Boolean-Values-and-Coercion)

In pandas, **boolean** values are represented as `True` or `False`. When you use the `.isna()` method on a DataFrame, it returns a DataFrame of boolean values, where each value is `True` if the corresponding element is missing and `False` otherwise.

**Coercion** refers to the conversion of one data type into another. In the case of `df.isna().sum()`, coercion happens implicitly when the `.sum()` method is called. The `.sum()` method treats `True` as `1` and `False` as `0` and sums these integer values. This is a form of coercion where boolean values are coerced into integers for the purpose of performing arithmetic operations.

Here’s an example to illustrate this:

```python
import pandas as pd
import numpy as np

# Create a DataFrame with some missing values
df = pd.DataFrame({
    'A': [1, np.nan, 3],
    'B': [4, 5, np.nan]
})

# Apply isna() to get a DataFrame of boolean values
bool_df = df.isna()

# Coerce boolean values to integers and sum them
missing_values_sum = bool_df.sum()

print(bool_df)
print(missing_values_sum)
```

In the output, `bool_df` shows that in data in the DataFrame is boolean after applying `.isna()`. The `missing_values_sum` shows the sum of missing values per column, where `True` values have been coerced to `1` and summed up.

> For more details regarding "counting missing values", see [01.3](01.3).
