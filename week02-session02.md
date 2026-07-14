# NumPy, Pandas, and Visualization Libraries for Data Science & AI

## TL;DR

This session (Week 2) introduces the core Python libraries data scientists and AI engineers use daily: **NumPy** for fast array computation, **Pandas** for tabular data analysis (using a churn-modeling CSV), and a brief overview of **Matplotlib** and **Seaborn** for charts. The instructor explains why the bootcamp’s Python track is intentionally lighter than a software-engineering path, then walks through hands-on Colab examples including indexing, aggregation, filtering, and null handling. Visualization demos lean on AI-assisted code generation (Groq) rather than deep chart-by-chart teaching.

## Topics Covered

- Recap: why the Python curriculum is short for DS/AI vs. software engineering
- Planned libraries: NumPy, Pandas (primary); Matplotlib, Seaborn (visualization, if time permits)
- **NumPy**: definition, dimensions (1D–nD), vectorization vs. lists/for-loops
- **NumPy practical**: install/import, `np.array`, `ndim`, dtypes, indexing & slicing (`[row, col]`, `:`)
- `np.zeros`, `np.ones`, `np.full`, `np.random.rand`
- Exercise: build an odd-sized “plus” pattern matrix with `n // 2`
- Break (~6 minutes)
- **NumPy arithmetic & statistics**: element-wise ops, `max`, `min`, `mean`, `np.median`, `std`, `var`
- **Pandas**: features, `read_csv` / `read_excel`, `head`, `tail`, `info`, `describe`
- Sorting (`sort_values`, `inplace`), row slicing, boolean filtering
- Null handling: `fillna`, `dropna`, `inplace` vs. assignment
- Break; logistics (Colab files on SharePoint via Aparna/Nazia)
- **Matplotlib & Seaborn**: chart types, qualitative vs. quantitative data, when to use which chart
- Quick practical: Groq-generated plots on churn dataset (`value_counts`, bar, pie, histogram, scatter, heatmap, pair plot)
- Wrap-up: assignments announced in doubt-clearing session; break week before next class

## Key Concepts Explained

### Python depth for DS/AI vs. software engineering

- What it is: Two career paths—backend/software needs deep Python (DS&A, months of practice); DS/AI leans on libraries.
- Why it matters: Beginner–intermediate Python plus library fluency is enough to start DS/AI work.
- Emphasis: Curriculum focuses on **Pandas, NumPy**, and related tools, not mastering every Python topic.

### NumPy (Numerical Python)

- What it is: Library for computation on single- and multi-dimensional arrays (1D, 2D, 3D, nD).
- Why it matters: Faster and lower memory than plain lists for large data; core to ML, stats, and scientific computing.
- Caveats: Implemented largely in **C** for speed; default float dtypes in `zeros`/`ones` unless you set `dtype=int` (e.g. `int32`). Instructor deferred a live speed comparison to break or self-practice. [unclear: claim that NumPy does not support Python 3.12—mentioned in chat, instructor unsure.]

### Vectorization

- What it is: Operating on whole arrays at once instead of Python loops.
- Why it matters: Benchmarks shown (presentation): NumPy vectorized ops ~4× faster than list comprehensions / `map` for comparable tasks.
- When to use: Large datasets—prefer NumPy (or Pandas) over lists/tuples.

### Array dimensions (`ndim`)

- What it is: Number of “levels” of nesting; for NumPy, often inferred from bracket structure.
- Why it matters: Distinguishes 1D vectors from 2D matrices (e.g. `[[1,2,3],[4,5,6]]` → `len` 3, `ndim` 2).
- Tip: Nested list `[[10,20,30,40,50]]` has **one** outer element; access inner index with `[0][2]`.

### Indexing and slicing NumPy arrays

- What it is: Same spirit as lists (`a[0][0]`) plus **row, column** form `a[row, col]` and slices `a[0, :]`, `a[:, 1]`, `a[:, 5:7]`.
- Why it matters: Foundation for Pandas row/column logic and ML feature slicing.
- Emphasis: Practice slowly; colon semantics (`5:7` vs `5:6`) mean inclusive start, exclusive end (Python style).

### Factory arrays: `zeros`, `ones`, `full`, `random`

- What it is: Quick creation of shaped arrays filled with 0, 1, a constant, or random values (`np.random.rand`).
- Why it matters: Initialization patterns (e.g. mask then set cross/plus lines to 1).

### Plus-pattern matrix (odd *n*)

- What it is: `n×n` zeros, then set middle row and middle column to 1 using index `n // 2`.
- Why it matters: Combines initialization + indexed assignment; only works for **odd** *n* (symmetric “plus”).
- Pattern: For *n*=5 use index 2; for *n*=7 use 3 → always `n // 2`.

### NumPy arithmetic and aggregations

- What it is: `a + 2`, `a * 2`, `a ** 2` apply element-wise; `a.max()`, `a.min()`, `a.mean()`, `np.median(a)`, std/var available.
- Why it matters: Vectorized math without explicit loops.
- Note: `mean` returns float; median via **`np.median(a)`**, not `a.median()`.

### Pandas DataFrame

- What it is: Fast, flexible open-source tool for tabular data (`import pandas as pd`); primary for traditional ML/DS/analytics.
- Why it matters: Less central for “pure” GenAI/agentic workflows but essential for classical ML and analytics.
- Read: `df = pd.read_csv('path')` or `pd.read_excel` for `.xlsx`.

### Inspecting data: `head`, `tail`, `info`, `describe`

- What it is: `head(n)` / `tail(n)` preview rows; `info()` rows/columns/null counts/dtypes; `describe()` stats on **numeric** columns only.
- Why it matters: Spot bad dtypes (e.g. credit score as `object`), missing values, and distribution summaries (mean, median, percentiles, std).

### Sorting and persistence

- What it is: `df.sort_values('age')` sorts temporarily unless you assign (`new_df = ...`) or use `inplace=True`.
- Why it matters: Common beginner mistake—assuming sort mutates original `df`.

### Filtering (Excel/SQL-style “where”)

- What it is: Boolean indexing, e.g. `new_df = df[df['age'] >= 50]`.
- Why it matters: Subset populations; combine with `len` and ratios for percentages (example: ~14% age ≥ 50 in churn data).

### Missing values: `fillna` vs `dropna`

- What it is: `fillna(value)` imputes; `dropna()` removes rows with nulls; column-specific: `new_df['age'].fillna(10)`.
- Why it matters: Same `inplace=True` or assign-to-new-frame rule as sorting.
- Emphasis: Usually impute **per column** with a strategy, not one `fillna` on entire frame blindly.

### Visualization libraries and chart choice

- What it is: **Matplotlib** and **Seaborn** are the main Python plotting libs (also mentioned: Plotly).
- Why it matters: **Categorical** → bar/pie; **numerical** distributions → histogram/box plot; **two numeric** → scatter (correlation); **many columns** → pair plot; **correlation matrix** → heatmap.
- Seaborn: often more visually appealing; Matplotlib also fine—preference-based.

### AI-assisted plotting (Groq)

- What it is: Attach CSV to Groq; ask for sample Matplotlib/Seaborn code for bar, pie, histogram, scatter, heatmap, pair plot, etc.
- Why it matters: Rapid practice plots; follow-up prompts can ask Groq to **interpret** a chart.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| NumPy (`np`) | Numerical arrays; `pip install numpy`; preinstalled on Colab |
| Pandas (`pd`) | DataFrames; `pip install pandas`; `read_csv`, `read_excel` |
| Matplotlib (`plt`) | `import matplotlib.pyplot as plt` |
| Seaborn (`sns`) | Higher-level, prettier plots on top of Matplotlib |
| Google Colab | Default environment; upload CSV via Files panel |
| `pip install` | Standard install when libs missing |
| Churn modeling CSV | Class dataset (customer churn); instructor shared via temporary Google Drive |
| Groq | AI code generation for chart examples from attached file |
| SharePoint / Discord | Official notebooks/files via Aparna/Nazia; tag in doubts channel |
| YouTube: “Satyajit EDA” | ~5 hr EDA video recommended (not part of agentic AI curriculum) |
| Plotly | Mentioned as alternative viz library |

## Code & Examples

### Install and import

```python
# pip install numpy pandas  # often "already satisfied" on Colab

import numpy as np
import pandas as pd
```

### NumPy: create arrays and dimensions

```python
a = np.array([10, 20, 30, 40, 50])
print(len(a))      # 5
print(a.ndim)      # 1

b = np.array([[10, 20, 30, 40, 50]])
print(len(b))      # 1
print(b.ndim)      # 2

c = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(len(c))      # 3
print(c.ndim)      # 2

# Explicit float dtype
a = np.array([10, 20, 30, 40, 50], dtype=float)
```

### NumPy: indexing and slicing

```python
A = np.array([[1, 2, 3, 4, 5, 6, 7],
              [8, 9, 10, 11, 12, 13, 14]])

print(A[1, 4:6])    # [12, 13]
print(A[0, 3])      # 4  (row 0, col 3)
print(A[1, 0])      # 8
print(A[0, :])      # first row
print(A[:, 1])      # column 1 → [2, 9]
print(A[:, 5:7])    # cols 5,6 → [6, 7, 13, 14]
```

### NumPy: zeros, ones, full, random

```python
np.zeros((3, 3))                    # float zeros by default
np.zeros((3, 3), dtype=int)         # or dtype=np.int32
np.ones((3, 3), dtype=int)
np.full((3, 3), 50)
np.random.rand(1, 4)                # random values (instructor demo)
```

### Plus pattern (odd n)

```python
def plus_matrix(n):
    a = np.zeros((n, n), dtype=int)
    mid = n // 2
    a[mid, :] = 1      # middle row
    a[:, mid] = 1      # middle column
    return a

# n=3 → 010 / 111 / 010 style; n=5 → larger plus
```

### NumPy: arithmetic and stats

```python
a = np.array([10, 20, 30, 40])
print(a + 2)
print(a * 2)
print(a ** 2)
print(a.max(), a.min(), a.mean())
print(np.median(a))
# std, var also available on arrays
```

### Pandas: load and explore churn data

```python
import pandas as pd
import numpy as np

df = pd.read_csv('/content/Churn_Modelling.csv')  # path from Colab upload

df.head()           # default 5 rows
df.head(10)
df.tail()
df.info()           # dtypes, non-null counts
df.describe()       # numeric columns only
```

### Pandas: sort, slice rows, filter

```python
# Temporary sort
df.sort_values('age')

# Persist sort
new_df = df.sort_values('age')
# or: df.sort_values('age', inplace=True)
# descending: ascending=False

# Rows 2–4
df[2:5]

# Age >= 50
new_df = df[df['age'] >= 50]
pct = len(new_df) / len(df) * 100
```

### Pandas: null imputation

```python
# Fill null ages with 10 (local unless inplace/assign)
df['age'].fillna(10, inplace=True)
# or: new_df = df.copy(); new_df['age'].fillna(10)

df.dropna()   # drops rows with any null (demo removed 6 of 10000 rows)
```

### Visualization imports + AI workflow (approximate)

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('Churn_Modelling.csv')

# Example patterns from Groq-generated code (column names from dataset):
sns.countplot(data=df, x='Geography')
df['Gender'].value_counts().plot(kind='bar')
# pie charts, histograms (age, credit score, balance), scatter, heatmap, pairplot
```

*Exact Groq-generated cells were copy-pasted live; reproduce by attaching the same CSV and requesting bar, pie, histogram, scatter, line, heatmap, and pair plots.*

## Q&A / Discussion Points

- **Why NumPy if we have lists?** Large data and speed (vectorization / C implementation); detailed timing demo deferred to break or self-practice.
- **Bianca — slicing and “outer brackets”:** Clarified `A[0, 4:]` style access for partial rows; encouraged re-running examples in notebook.
- **NumPy median:** `np.median(a)` works; `a.median()` on array was initially doubted then corrected.
- **Sorting — does it stick?** No, unless `inplace=True` or assigned to a variable (`new_df`).
- **`fillna` on original frame?** Changes are local unless `inplace=True` or saved to new DataFrame.
- **Seaborn vs Matplotlib:** Overlapping chart types; Seaborn often prettier; use either.
- **SharePoint / class files:** Tag **Nazia** or **Aparna** on Discord; Colab also on instructor Google Drive (Week 2 folder).
- **Participant drop / timing:** Possible daylight-saving confusion; batch timing unchanged per instructor.
- **Connection drops:** Instructor acknowledged; no fix offered.

## Action Items & Assignments

- [ ] Download **Churn_Modelling.csv** from class Google Drive link and upload to Colab
- [ ] Bookmark instructor’s temporary Google Drive used during sessions
- [ ] Practice NumPy speed comparison (lists vs NumPy) during break or on your own
- [ ] Work through shared **Week 2** Colab/notebook on SharePoint (via Aparna/Nazia after class)
- [ ] Complete **Python assignments** during break week; assignment list and expectations covered in **doubt-clearing session** (recording available if you miss live)
- [ ] Optional: watch instructor’s **~5 hour EDA** video on YouTube (search instructor name + “EDA”) after finishing Python assignments
- [ ] Optional: run Groq-generated Matplotlib/Seaborn exercises on churn data; ask Groq to interpret plots in a separate chat
- [ ] Optional: session **poll** (comfort feedback)—vote during or after class
- [ ] Practice `sort_values` / `fillna` with `inplace=True` vs assignment on your own copy of `df`

## Open Questions / Unclear Spots

- [unclear: exact SharePoint URL for course materials—instructor directed students to Nazia/Aparna]
- [unclear: full assignment question list—promised only in upcoming doubt-clearing class]
- [unclear: NumPy support on Python 3.12—raised in chat, not confirmed]
- [unclear: planned “complex example” comparing loop vs NumPy at 10:00 break—may not have been shown after session interruptions]
- Instructor started a **concatenation** exercise on NumPy arrays but skipped when it did not work as expected
- **Null imputation** demo used a manually corrupted “version 2” CSV created live; students may need the shared v2 file or recreate nulls
- `np.random.rand(1, 4)` described as “between one to four” in speech—standard `rand` returns uniform floats in [0, 1), not integers 1–4 (verify intent if reproducing)
