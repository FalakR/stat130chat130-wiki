# 00 Tools

1. [GitHub](https://github.com/pointOfive/STA130_ChatGPT/blob/main/README.md)
2. [UofT Jupyterhub](https://datatools.utoronto.ca) (or[google collab](https://colab.research.google.com/)) `.ipynb` notebook files
3. [Copilot](https://copilot.microsoft.com/) (or [ChatGPT](https://chat.openai.com/))
4. Course [wiki-textbook](https://github.com/pointOfive/STA130_ChatGPT/wiki/)

# 01 Data Summarization

1. importing libraries... like [`pandas`](01.1)
2. loading data... with [`pd.read_csv()`](01.2)
    1. parameters and arguments 
3. counting missing values... with [`df.isna.sum()`](01.3)
    1. [boolean values and coercion](01.3)
4. observations (rows) and variables (columns)... [`df.shape`](01.5) and [`df.columns`](01.5)
    1. function side-effects 
5. numeric versus non-numeric... [`df.describe()`](01.4) and [`df.value_counts()`](01.4)
    1. `.dtypes` and `.astype()` 
    2. statistic calculation functions
6. removing missing data... with [`df.dropna()`](01.6) and [`del df['col']`](01.6)
    1. dictionary `dict()` objects
7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01.7)
8. Additional LEC topics
    1. sorting and (0-based) indexing
    2. subsetting and boolean selection


**Out of scope**
1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as how to handle missing values using more advanced methods than just "ignoring" or "removing" them
4. ...further "data wrangling topics" such as "joining" and "merging"; "pivoting", "wide to long", and "tidy" data formats; etc.

# 02 Coding

1. first data types... [`tuple`](02.1), [`list`](02.1), [`dict`](02.1)
    1. `type()` not "types of data" like quantitative or qualitative
    2. `str` (and `sentence.split()`) versus `int` versus `float` versus `bool`
    3. operator overloading polymorphism with `+` and `.sum()`
2. another key data type... [`np.array`](02.2)
    1. `from scipy import stats`, `stats.multinomial`, and probability
3. loops... [`for i in range(n)`](02.3) and [`print()`](02.3)
    1. `for x in lst` and `for word in sentence.split()` and 
4. more loops... such as [`for i,x in enumerate(a_list)`](02.4)
    1. ~`for key,val in dictionary.items()` and `dictionary.keys()` and `dictionary.values()`~
5. logical flow control... with [`if`](02.5)/[`else`](02.5)/[`elif`](02.5)
6. Additional LEC topics
    1. conditional probability Pr(Y=y|X=x)
        1. ~text manipulation with `.apply(lambda x: ...)`, `.replace()`, `re`~

**Out of scope**
1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as modular code design (with `def` based functions or `classes`)
4. ...such as dictionary iteration (which has been removed from the above material)
5. ...such as text manipulation with `.apply(lambda x: ...)`, `.replace()`, `re` (which are introduced but are generally out of scope for STA130)

# 03 Data Visualization

0. ["types of data"](03.0)... continuous, discrete, nominal and ordinal categorical, and binary
1. [bar plots](03.1) and the [mode](03.1)
    1. plotting... plotly, seaborn, matplotlib, pandas
2. [histograms](03.2)
    1. kernel density estimation "violin plots" 
3. [box plots](03.3), [range](03.3), [IQR](03.3) and [outliers](03.3)
    1. legends, annotations, figure panels
4. [skew](03.4) and [multimodality](03.4) 
    1. [mean versus median](03.4) and [normality and standard deviations](03.4)
    2. log transformations
5. Additional LEC topics
    1. samples versus populations (distributions) / statistics versus parameters?
    2. normal distributions
    3. more `from scipy import stats` (re: `stats.multinomial`) with `stats.norm` and `stats.poisson`

**Out of scope**
1. Material covered in future weeks
2. Anything not substantively addressed above...
3. ...such as expectation, moments, integration, heavy tailed distributions...
4. ...such as kernel functions for kernel density estimation
5. ...bokeh, shiny, d3, ...


