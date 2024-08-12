**TUT/HW Topics**

1. importing libraries... like [`pandas`](01-Data-Summarization#import)
2. loading data... with [`pd.read_csv()`](01-Data-Summarization#read_csv)
3. counting missing values... with [`df.isna().sum()`](01-Data-Summarization#Missingness-I)
4. observations (rows) and variables (columns)... [`df.shape`](01-Data-Summarization#Variables-and-Observations) and [`df.columns`](01-Data-Summarization#Variables-and-Observations)
5. numeric versus non-numeric... [`df.describe()`](01-Data-Summarization#Types-I) and [`df.value_counts()`](01-Data-Summarization#Types-I)
6. removing missing data... with [`df.dropna()`](01-Data-Summarization#Missingness-II) and [`del df['col']`](01-Data-Summarization#Missingness-II)
7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01-Data-Summarization#Grouping-and-Aggregation)

**LEC Extensions**

2. [function/method arguments](01-Data-Summarization#functionmethod-arguments) (like `encoding` and `inplace`)
    1. ~function side-effects~
3. [boolean values and coercion](01-Data-Summarization#Boolean-Values-and-Coercion)
5. 
    1. [`.dtypes` and `.astype()`](01-Data-Summarization#pandas-column-data-types)
    2. ~[What are `pandas DataFrame objects`?](01-Data-Summarization#what-are-pandas-dataframe-objects)~
    3. statistic calculation functions

**LEC New Topics**

1. sorting and (0-based) indexing
2. subsetting and boolean selection

**Out of Scope**

1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as how to handle missing values using more advanced techniques that don't just "ignore" or "remove" them
4. ...further "data wrangling topics" such as "joining" and "merging"; "pivoting", "wide to long", and "tidy" data formats; etc.

# TUT/HW Topics

## import

> **TUT/HW Topics**
> 
> 1. importing libraries... like [`pandas`](01-Data-Summarization#import)

```python
import pandas as pd # aliasing usage is `pd.read_csv()`
import pandas # original name usaged is `pandas.read_csv()`
from pandas import read_csv # funtion only import usage is `read_csv()`
```

## read_csv

> **TUT/HW Topics**
> 
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

See the [Function/Method Arguments](01-Data-Summarization#functionmethod-arguments) section below for further details!
 

## Missingness I

> **TUT/HW Topics**
> 
> 3. counting missing values... with [`df.isna.sum()`](01-Data-Summarization#Missingness-I)

In Python we generally use the `pandas` library to count missing values.
Specifically, we use the `.isna()` (or equivalently `isnull()` since it is just an **alias** for `.isna()`) followed by `.sum()`.
 
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

For more details regarding "boolean values and coercion", see [Boolean Values and Coercion](01-Data-Summarization#Boolean-Values-and-Coercion).

## Variables and Observations

> **TUT/HW Topics**
> 
> 4. observations (rows) and variables (columns)... [`df.shape`](01-Data-Summarization#variables-and-observations) and [`df.columns`](01-Data-Summarization#Variables-and-Observations)

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

> Missing values in datasets need to be handled carefully during analysis because they can affect the results. Different statistical analyses and tools have their own ways of dealing with missing values, either by ignoring, removing them, or filling them in with estimated values. These techniques are beyond the scope of STA130 so we will not introduce or consider them here.

## Types I

> **TUT/HW Topics**
> 
> 5. numeric versus non-numeric... [`df.describe()`](01-Data-Summarization#Types-I) and [`df.value_counts()`](01-Data-Summarization#Types-I)

The `.describe()` **method** provides descriptive statistics that summarize **numerical data** in terms of its location (or position) and scale (or spread). Its provides mean, standard deviation, median (50th percentile), quartiles (25th and 75th percentile), and minimum and maximum values.

> The statistic calculations are based on the non-missing values in the data set, and the number of such non-missing values used to calculate the statistics is given by the "count" value returned from `.describe()`.

The `[column_name].value_counts()` **method** counts the number of each unique value in a column (named `column_name`). This **method** is used for **categorical data** to understand the distribution of categories within a feature. It does not count missing values by default, but it can include them in the counts by using `dropna=False`.

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

## Missingness II

> **TUT/HW Topics**
> 
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

> The `del df['col']` expression is a somewhat unusual looking line of `python` code which is the result of the `python dict type` structure underlying `pandas DataFrame objects` as is described in the the [Dictionary](01-Data-Summarization#dictionaries) section below.


## Grouping and Aggregation

> **TUT/HW Topics**
> 
> 7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01-Data-Summarization#Grouping-and-Aggregation)

Grouping and aggregation are powerful concepts in data analysis, particularly with pandas in Python. They allow you to organize data into groups and then perform operations on those groups to extract insights.

**Grouping** refers to the process of organizing data into groups based on some criteria. In pandas, this is done using the `.groupby()` **method**. When you group data, you’re essentially splitting the DataFrame into smaller chunks based on unique values of a specified key column or columns. For example, `df.groupby("col1")` will create a group for each unique value in `"col1"`.

**Aggregation** refers to computing summaries of each of the groups once they're separated. Some examples of aggregation functions are the `.sum()`, `.mean()`, `.min()`, `.max()`, and `.count()` **methods**. When you use `df.groupby("col1")["col2"].describe()` you're doing all of these at once (as well as `np.quantile([25,50,75])`).

> After `df.groupby("col1")` groups the data by unique values in `"col1"`, the subsequent `["col2"]` selects the `"col2"` column from the data, and then for each group the concluding `.describe()` computes the summary statistics for `"col2"` within each group. Namely, the count, mean, standard deviation, minimum, 25% (first quartile), 50% (median), 75% (third quartile), and maximum values for `"col2"` within each group. 

Missing values in the grouping column (`"col1"`) will result in a separate group if there are any, while the `.describe()` **method** automatically excludes missing values when calculating descriptive statistics for `"col2"`.


# LEC Extensions


## Function/Method Arguments 

> **LEC Extensions**
>
> 2. [function/method arguments](01-Data-Summarization#Function-Arguments) (like `encoding` and `inplace`)

The `pandas.read_csv` `python` (`pandas`) **function** is used to read a CSV (Comma Separated Values) file as a `pandas DataFrame object` (that can be assigned into a `python` variable). Running the following in a `jupyter notebook cell` will show you that the `pandas.read_csv` **function** can be controlled with a huge number of **arguments**.

```python
import pandas as pd
pd.read_csv? # add ? to the end of a function to see the so-called 
# *signature* of the function... that is, all the possible *arguments* of a function
```

> In statistics, the term **parameter** refers to a characterization about a **population** (as will be discussed later in the course); but, in programming, the term **parameters** refers to the kinds of input that can be used to control the behavior of a **function**; whereas, the actual values given to the **parameters** of a **function** are called the **arguments** of the **function**. We will therefore use the term **arguments** to refer to the inputs of a **function**; but, technically, the **arguments** are the values assigned to the **parameters** of a **function** when it is called.

In `python`, **function arguments** are named and can be optional if they have default values. The `filepath_or_buffer` **argument** of the `pd.read_csv` **function** is required (and not optional so it does not have default value). The `encoding` **argument** is optional, and has a default value of `None`, which means that the function uses the system's default character encoding system when reading the file. A common character encoding system is [UTF-8](https://en.wikipedia.org/wiki/UTF-8#:~:text=UTF%2D8%20is%20the%20dominant,8%20encodings%20on%20the%20web), and you could force `pd.read_csv` to expect this encoding by including the **argument** `encoding="utf-8"` into the `pd.read_csv` **function** when it is called. Another (sometimes useful) alternative `encoding="ISO-8859-1"` is demonstrated below.

```python
trickily_encoded_file = "https://raw.githubusercontent.com/pointOfive/STA130_F23/main/Data/amazonbooks.csv"
# remember, "https://github.com/pointOfive/STA130_F23/blob/main/Data/amazonbooks.csv" won't work 
# because that's not actually a link to a real CSV file [as you can see if you go to that github page]...

pd.read_csv(trickily_encoded_file, encoding='UTF-8') # fails
#pd.read_csv(trickily_encoded_file) # fails, because it defaults to UTF-8
#pd.read_csv(trickily_encoded_file, encoding="ISO-8859-1")# works!
```

There are likely many `pandas` **arguments** that you will find useful and helpful. For `pd.read_csv` some **arguments** address some relatively common special cases are 

- `sep` for different file types
- `skiprows` and `names` to control column names 
- and see more [here](https://note.nkmk.me/en/python-pandas-read-csv-tsv/)<br>(because ``pd.read_csv?` will probably be more confusing that helpful the first few times you look at it...)

Moving beyond `pd.read_csv`, another `pandas` **argument** that is worth considering is the `inplace` **argument**, which works as follows (and will be [referenced again below](01-Data-Summarization#pandas-column-data-types) to further contrast and clarify the nature of `inplace`).

```python
df = pd.read_csv(tricky_file, encoding="ISO-8859-1")
# df = df.dropna() # instead of this, just use
df.dropna(inplace=True) # where the so-called "side-effect" of this *method* 
# is to transform the `df` object into an updated form without missing values

# We typically think of functions are "returning values or object" 
# as in the case of `df = df.dropna()`; but, `df.dropna(inplace=True)`
# demonstrates that functions can operate in terms of "side-effects" on objects as well...
```

> Technically, `pd.read_csv` is called a **function** while `df.dropna(...)` is called a **method**. The reason for the difference is that the `.dropna(...)` **method** is a "function" that belongs to the `df` `pandas DataFrame object`. You can think of a **method** like `.dropna(...)` as a "function" who's first (default) **argument** is the `df` `pandas DataFrame object`.
>
> - We have already used the term **method** above (without explicitly defining it) in sections<br>
>   [4. Variables and Observations](01-Data-Summarization#Variables-and-Observations)<br>
>   [5. Types I](01-Data-Summarization#Types-I)<br>
>   [6. Missingness II](01-Data-Summarization#Missingness-II)<br>
>   [7. Grouping and Aggregation](01-Data-Summarization#Grouping-and-Aggregation)


## Boolean Values and Coercion

> **LEC Extensions**
>
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

> For more details regarding "counting missing values", see [Missingness I](01-Data-Summarization#Missingness-I) and [Missingness II](01-Data-Summarization#Missingness-II).


## `Pandas` column data `types`

> **LEC Extensions**
> 
> 5. 
>    1. [`.dtypes` and `.astype()`](01-Data-Summarization#pandas-column-data-types)

As demonstrated below

- the `.dtypes` **attribute** defines the `type` of data that is stored in a `pandas DataFrameObject` column
- while `.astype()` **method** is used to convert data types to specific formats more specifically suite the nature of the data column

The `.dtypes` **attribute** of a `pandas DataFrame object` provides the data type of each column. This is useful for identifying whether columns are numerical (e.g., `int64`, `float64`) or categorical (e.g., `object`, `category`).

```python
import pandas as pd

# Sample DataFrame
data = {
    'age': [25, 32, 47, 51],
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'income': [50000, 60000, 70000, 80000],
    'has_pet': ['yes', 'no', 'no', 'yes']
}
# Here, `age` and `income` are numerical (integers), while `name` and `has_pet` are given the `type` of `object` 
#   (which in the case of `has_pet` could be interpreted as **categorical** data; whereas
#    `name` is probably better interpreted as an identifier rather than a "cateogory")

df = pd.DataFrame(data)
df.dtypes
```

The `.astype()` **method** is used to convert the data type of a column to another type. For instance, you might want to convert a column from `object` to `category` (a more memory-efficient way to store **categorical data**) or convert a **numerical** column to `float64` if you need to include decimal points.

```python
# Convert the type of 'has_pet' to "categorical" and the type of 'income' to "float"
df['has_pet'] = df['has_pet'].astype('category') # `has_pet` is now of type `category`
df['income'] = df['income'].astype('float64') # `income` has been converted from `int64` to `float64`, allowing for decimal points.
df.dtypes
```

Something that you might like to do here is use `inplace`, e.g., `df['has_pet'].astype('category', inplace=True)`, but **this will not work**(!) because the `.astype()` does not have an `inplace` parameter because it returns a new `pandas DataFrame` or `Series object` with the converted data type. So, the typical usage is to reassign the result back to the original column, as shown in the examples above.

> For methods that do support `inplace`, such as `drop()`, `fillna()`, or `replace()`, the `inplace=True` parameter modifies the original DataFrame without creating a new one. Since `.astype()` doesn't support `inplace`, you need to explicitly assign the result to the column you want to change.


## What are `pandas DataFrame objects`?

> **LEC Extensions**
>
> 5. 
>     2. [dictionary `dict()` objects](01-Data-Summarization#what-are-pandas-dataframe-objects)
  
You've already encountered `python dict` (dictionary) `object types` in the [Missingness I](01-Data-Summarization#Missingness-I), [boolean values and coercion](01-Data-Summarization#Boolean-Values-and-Coercion), and [`Pandas` column data `types`](01-Data-Summarization#pandas-column-data-types) sections where they were used to defined `pandas DataFrame objects`; and, dictionaries were again used in the [Variables and Observations](01-Data-Summarization#Variables-and-Observations) to rename the columns of `pandas DataFrame objects`. 


Certainly! Let's dive into how dictionaries (`dict` objects) are used in Python, particularly in the context of creating and manipulating pandas DataFrames.

### What is a Dictionary in Python?

A dictionary in Python is a collection of key-value pairs. Each key is unique, and it maps to a value, which can be any data type (e.g., integer, string, list, another dictionary, etc.).

**Basic Example of a Dictionary:**

```python
person = {
    'name': 'Alice',
    'age': 25,
    'city': 'New York'
}
```

- `'name'`, `'age'`, and `'city'` are the keys.
- `'Alice'`, `25`, and `'New York'` are the corresponding values.

### Using a Dictionary to Create a DataFrame

When working with pandas, you can use a dictionary to create a DataFrame. In this context, the keys of the dictionary represent the column names, and the values represent the data for those columns.

**Example Using the Data from Before:**

Let's create a DataFrame using a dictionary where:
- The keys are `'age'`, `'name'`, `'income'`, and `'has_pet'`.
- The values are lists containing the data for each column.

```python
import pandas as pd

# Dictionary representing the data
data = {
    'age': [25, 32, 47, 51],
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'income': [50000, 60000, 70000, 80000],
    'has_pet': ['yes', 'no', 'no', 'yes']
}

# Create a DataFrame from the dictionary
df = pd.DataFrame(data)

# Display the DataFrame
print(df)
```

**Output:**

```
   age     name  income has_pet
0   25    Alice   50000     yes
1   32      Bob   60000      no
2   47  Charlie   70000      no
3   51    David   80000     yes
```

- Here, `data` is a dictionary where each key represents a column name, and each value is a list containing the data for that column.
- `pd.DataFrame(data)` converts the dictionary into a DataFrame.

### Why Use Dictionaries to Create DataFrames?

Using dictionaries to create DataFrames is common because it provides a clear and flexible way to organize data. Some benefits include:

1. **Clarity**: It's easy to see which column corresponds to which data.
2. **Flexibility**: You can easily adjust the data by modifying the dictionary (e.g., adding new columns or rows).
3. **Readability**: The code is easy to read and understand, making it clear how the DataFrame is structured.

### Adding New Columns Using a Dictionary

You can also add new columns to an existing DataFrame by using a dictionary:

**Example: Adding a New Column**

```python
# New column data
new_data = {
    'city': ['New York', 'Los Angeles', 'Chicago', 'Houston']
}

# Add the new column to the DataFrame
df['city'] = new_data['city']

# Display the updated DataFrame
print(df)
```

**Output:**

```
   age     name  income has_pet         city
0   25    Alice   50000     yes     New York
1   32      Bob   60000      no  Los Angeles
2   47  Charlie   70000      no      Chicago
3   51    David   80000     yes      Houston
```

### Summary

- A dictionary in Python is a collection of key-value pairs.
- In the context of pandas, dictionaries are commonly used to create DataFrames, where the keys are column names and the values are lists of data for those columns.
- This approach is flexible, easy to understand, and allows for straightforward data manipulation.

If you need further clarification or more examples, feel free to ask!

The explanation given there is worth repeating

```python
# The `{'a': 'A', 'b': 'B'}` is a "dictionary" `dict()` data type object.
# In dictionary parlance, the lowercase letters in the above example are "keys" 
# and the uppercase letters are the "values" which correspond the to the "keys"

# In this case, the code specifies the columns to be renamed (the keys)
# and what their new names should be (the values).
```

and 


> 6. removing missing data... with [`df.dropna()`](01-Data-Summarization#Missingness-II) and [`del df['col']`](01-Data-Summarization#Missingness-II)

> 3. 

