# 00 Tools

1. [GitHub](https://github.com/pointOfive/STA130_ChatGPT/blob/main/README.md)
2. [UofT Jupyterhub](https://datatools.utoronto.ca) (or[google collab](https://colab.research.google.com/)) `.ipynb` notebook files
3. [Copilot](https://copilot.microsoft.com/) (or [ChatGPT](https://chat.openai.com/))
4. Course [wiki-textbook](https://github.com/pointOfive/STA130_ChatGPT/wiki/)

# 01 Data Summarization
**Topics**
1. importing libraries... like [`pandas`](01.1)
2. loading data... with [`pd.read_csv()`](01.2)
3. counting missing values... with [`df.isna.sum()`](01.3)
    1. [boolean values and coercion](01.3)
4. continuous versus categorical... [`df.describe()`](01.4) and [`df.value_counts()`](01.4)
5. observations (rows) and variables (columns)... [`df.shape`](01.5) and [`df.columns`](01.5)
6. removing missing data... with [`df.dropna()`](01.6) and [`del df['col']`](01.6)
7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01.7)

**Out of scope**
1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as how to handle missing values using more advanced methods than just "ignoring" or "removing" them
4. ...further "data wrangling topics" such as "joining" and "merging"; "pivoting", "wide to long", and "tidy" data formats; etc.

# 02 Coding

1. loops... with [`for`](02.1)
2. logical flow control... with [`if`](02.2)/[`else`](02.2)
3. data types... such as [`list`](02.3), [`tuple`](02.3), [`dict`](02.3), and [`np.array`](02.3)
4. text data... [`str`](02.4)
