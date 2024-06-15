# 00 Tools

1. [GitHub](https://github.com/pointOfive/STA130_ChatGPT/blob/main/README.md)
2. [UofT Jupyterhub](https://datatools.utoronto.ca) (or[google collab](https://colab.research.google.com/)) `.ipynb` notebook files
3. [Copilot](https://copilot.microsoft.com/) (or [ChatGPT](https://chat.openai.com/))
4. Course [textbook](https://github.com/pointOfive/STA130_ChatGPT/wiki/)

# 01 Data Wrangling
**Topics**
1. importing libraries... like [`pandas`](01.1)
2. loading data... with [`pd.read_csv()`](01.2)
3. counting missing values... with [`df.isna.sum()`](01.3)
    1. [boolean values and coercion](01.3)
4. continuous versus categorical... [`df.describe()` and `df.value_counts()`](01.4)

The `df.describe()` method provides descriptive statistics that summarize the central tendency, dispersion, and shape of a dataset’s distribution, excluding NaN values. It’s typically used for numerical data to get an overview of the mean, median, quartiles, and other statistical measures.

The `df.value_counts()` method returns a Series containing counts of unique values. This method is used for categorical data to understand the distribution of categories within a feature. It does not count NaN values by default, but you can include them by setting dropna=False.


**Out of scope**
1. Material covered in future weeks
2. 
# 02 Coding
