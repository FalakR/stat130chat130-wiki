
# import

1. importing libraries... like [`pandas`](01-Data-Summarization

2. loading data... with [`pd.read_csv()`](01-Data-Summarization#read_csv)
3. counting missing values... with [`df.isna.sum()`](01-Data-Summarization#Missingness-I)
4. observations (rows) and variables (columns)... [`df.shape`](01-Data-Summarization#Variables-and-Observations) and [`df.columns`](01-Data-Summarization#Variables-and-Observations)
5. numeric versus non-numeric... [`df.describe()`](01-Data-Summarization#Types-I) and [`df.value_counts()`](01-Data-Summarization#Types-I)
6. removing missing data... with [`df.dropna()`](01-Data-Summarization#Missingness-II) and [`del df['col']`](01-Data-Summarization#Missingness-II)
7. grouping and aggregation.... with [`df.groupby("col1")["col2"].describe()`](01-Data-Summarization#Grouping-and-Aggregation)

**LEC Extensions**

2. parameters and arguments
3. [boolean values and coercion](01-Data-Summarization#Boolean-Values-and-Coercion)

```python
import pandas as pd # aliasing usage is `pd.read_csv()`
import pandas # original name usaged is `pandas.read_csv()`
from pandas import read_csv # funtion only import usage is `read_csv()`
```