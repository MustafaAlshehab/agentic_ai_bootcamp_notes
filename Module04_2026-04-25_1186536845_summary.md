# Supervised Learning: Regression, Feature Scaling, and Classification Intro

## TL;DR

This session grounded AI/ML in supervised vs. unsupervised learning, then focused on **regression** (simple and multiple linear regression, best-fit line intuition, and a Colab walkthrough on the **50 Startups** dataset). The instructor covered **train/test split**, **feature scaling** (standardization vs. normalization, and why `fit_transform` differs from `transform` on test data), and **regression metrics** (MAE and related). Classification was introduced conceptually (workflow and accuracy); **KNN, decision trees, and random forest** were deferred to the next class.

## Topics Covered

- AI vs. ML: AI as a broad goal; ML, computer vision, TTS/STT, automation as paths to AI
- Natural vs. artificial intelligence (brief framing)
- ML types: supervised, unsupervised, reinforcement (RL noted as central to generative AI today; not covered in depth)
- Supervised learning: known target column (Y); unsupervised: no labeled target or open-ended goals
- Bank fraud example: labeled fraud (supervised) vs. unlabeled transactions (unsupervised)
- Customer segmentation / mall analytics as unsupervised
- Supervised subtypes: **classification** vs. **regression**
- Industry examples: churn, fraud, sentiment, medical imaging (COVID from X-ray); regression examples sparser in practice
- **Time series** as related to but distinct from regression
- **Binary** vs. **multi-class** classification
- Math refresh: Euclidean distance between two points; plotting lines from \(y = 2x + 3\)
- **Simple linear regression**: one feature, best-fit line, house price by area
- **Multiple linear regression**: many features; higher dimensions not plottable in 2D
- Hands-on: **50 Startups** dataset — SLR, comparing features, MLR
- **Correlation** for feature choice
- **Feature scaling**: standardization and normalization; height/weight motivation
- `fit_transform` on train vs. `transform` only on test
- When to use standardization vs. normalization (theory vs. industry practice)
- Classification pipeline (X/Y, train/test, scaling, multiple models, accuracy)
- Break; session ended before classification algorithms and full practicals

## Key Concepts Explained

### AI and machine learning

- **What it is:** AI is the broad goal of achieving intelligence artificially; ML is one major approach (along with CV, speech, automation, etc.).
- **Why it matters:** Bootcamp framing connects “predictive AI” to ML; ML is the superset that includes predictive analytics.
- **Emphasis:** ML and AI are tightly related in how the course is taught.

### Supervised vs. unsupervised learning

- **What it is:** Supervised = you know which column to predict (Y exists). Unsupervised = no clear predictor or task path (e.g., no fraud labels, open-ended “find high-value customers”).
- **When to use:** Fraud with an indicator column → supervised; same features without labels → unsupervised; segmentation without a target column → unsupervised.
- **Emphasis:** Unsupervised is not “prediction” in the same sense; you may not know what to optimize.

### Classification vs. regression

- **What it is:** Classification predicts a **categorical** target (fraud/not, churn/active, sentiment). Regression predicts a **numeric** target (temperature, house price, profit).
- **When to use:** Discrete classes → classification; continuous numbers → regression.
- **Emphasis:** Classification use cases dominate in industry per instructor; regression examples are fewer except via **time series** (separate algorithms/domain).

### Binary vs. multi-class classification

- **What it is:** Two classes (e.g., positive/negative) vs. three or more (e.g., positive/negative/neutral).
- **Why it matters:** Problem setup and metrics differ when there are more than two labels.

### Reinforcement learning (mentioned only)

- **What it is:** Third ML paradigm; historically fewer market use cases, now hot in generative AI.
- **Caveat:** Not covered in this session; supervised focus today.

### Best-fit line (simple linear regression)

- **What it is:** Among infinitely many lines through scattered points, the **best-fit** line minimizes total distance (error) from points to the line; then use the line to predict Y at new X (e.g., price at 1,000 sq ft).
- **Why it matters:** Core geometric intuition before `LinearRegression` in sklearn.
- **Emphasis:** Compare candidate lines by summed distances; closer line wins hypothetically, then calculated in code.

### Simple vs. multiple linear regression

- **What it is:** SLR: one feature, \(y = mx + c\), plottable in 2D. MLR: many features, \(y = m_1 x_1 + m_2 x_2 + \cdots + c\); not realistically visualized beyond 2D/3D.
- **Why it matters:** Real house prices (and startup profit) depend on many factors, not area alone.
- **Emphasis:** **Simple linear regression “doesn’t exist” in real life** as a sole-feature model—instructor used it for teaching and visualization only.

### Correlation

- **What it is:** Relationship between two variables: positive (both increase/decrease together), negative (inverse), or weak/none.
- **When to use:** Pick X features that correlate strongly with Y (e.g., R&D spend vs. profit higher than marketing spend in the demo).
- **Caveat:** Instructor proceeded with marketing spend first without correlation; later showed R&D had higher correlation (~0.74 vs. higher for R&D).

### Train / test split

- **What it is:** Hold out a test set; train on `X_train`, `y_train`; evaluate predictions on `X_test` against `y_test`.
- **Why it matters:** Test data must remain unseen by the model during training.
- **Emphasis:** Model never sees `y_test` during fit; predict on `X_test` only.

### Reshape for sklearn

- **What it is:** Feature matrix must be **2D** (e.g., `reshape(-1, 1)`); 1D arrays cause errors like “Expected 2D array, got 1D.”
- **Why it matters:** Library API requirement, not optional polish.

### Feature scaling

- **What it is:** Rescale columns so magnitudes don’t bias models (e.g., height 180 vs. weight 90 treated as “bigger number = more important”).
- **Techniques:** **Standardization:** \(x' = (x - \mu) / \sigma\). **Normalization (min-max):** \(x' = (x - x_{\min}) / (x_{\max} - x_{\min})\).
- **Why it matters:** Models should treat features on comparable scales; computers only see numbers, not units.
- **Emphasis:** Dividing each column by its own max is **one** approach but **not** the statistically “approved” pair—use StandardScaler or MinMaxScaler. Instructor defers detail to **EDA lecture**.

### `fit_transform` vs. `transform`

- **What it is:** On **training** data, fit computes \(\mu\) and \(\sigma\) (or min/max) and transforms. On **test** data, only **transform** using training statistics—never refit \(\mu/\sigma\) on test alone.
- **Why it matters:** Test simulates unseen production data; leakage from test statistics would be invalid.
- **Emphasis:** Testing data is unseen to the model but scaling must still use **training** mean and standard deviation.

### Standardization vs. normalization (when to use)

- **Theory:** Gaussian-like data → standardization; non-Gaussian/random → normalization (thumb rule from textbooks/ChatGPT).
- **Practice:** Instructor recommends picking one (often **StandardScaler**) without expensive A/B trials; time better spent on hyperparameter tuning.
- **Emphasis:** Industry often “blindly” uses standardization; comparing both is costly.

### Regression evaluation metrics

- **What it is:** **MAE** (mean absolute error), **MSE**, **RMSE**, **R²**, adjusted R²—compare predicted vs. actual on test set.
- **Why it matters:** Lower error (e.g., MAE 8,000 vs. 10,000) means better model; same logic as accuracy in classification but with continuous errors.
- **Example:** Two predictions vs. actuals; average absolute gaps → MAE.

### Classification workflow (intro)

- **What it is:** Define X and Y (e.g., churn column), train/test split, feature scaling, fit several models on training data, predict on `X_test`, compare accuracy (e.g., 3/5 correct → 60% vs. 4/5 → 80%).
- **Why it matters:** Same pipeline skeleton as regression; metric is often **accuracy** (and others later).
- **Caveat:** Algorithms (KNN, trees, forest) and full notebook practical **scheduled for next session**.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Python | Notebook practicals |
| pandas (`pd`) | `read_csv`, `df.info()`, `df.describe()` |
| NumPy | Imports alongside pandas |
| scikit-learn | `LinearRegression`, `train_test_split`, `StandardScaler`, `MinMaxScaler` (normalization) |
| Google Colab | Upload dataset and regression notebook |
| SharePoint | Assignment answers; notebook/file distribution via Codecademy team |
| ChatGPT / Groq | Explaining `describe()` output; correlation code snippets |
| 50 Startups dataset | CSV: R&D spend, administration, marketing spend, state, profit |
| `df.info()` | dtypes: floats for numeric, `object` for categorical state |
| `df.describe()` | count, mean, std, min, percentiles, max |
| Correlation (pandas) | e.g., profit vs. marketing spend ~0.74; R&D higher |
| `StandardScaler` | `fit_transform` on train, `transform` on test |
| Regression metrics | MAE, MSE, RMSE, R², adjusted R² (evaluation block in notebook) |

## Code & Examples

### Euclidean distance (points A=(3,2), B=(8,6))

\[
d = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2} = \sqrt{(3-8)^2 + (2-6)^2} = \sqrt{41} \approx 6.4
\]

### Plotting \(y = 2x + 3\)

Substitute values for \(x\), plot \((x,y)\): e.g. (0,3), (1,5), (2,7), (-1,1), (-2,-1).

### SLR and MLR formulas

```text
# Simple linear regression (one feature)
y = m * x + c

# Multiple linear regression
y = m1*x1 + m2*x2 + m3*x3 + ... + c
# e.g. x1=area, x2=location, x3=interest rate, x4=bedrooms, ...
```

### 50 Startups regression pipeline (approximate from walkthrough)

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression
# ... metrics imports for MAE, MSE, RMSE, R² as in notebook

df = pd.read_csv("<path>/50_Startups.csv")  # or 50 startups filename from class

# Target and one feature (SLR example: marketing spend)
X = df[["Marketing Spend"]].values.reshape(-1, 1)  # 2D required
y = df["Profit"].values

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=..., random_state=...)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)  # use train mu/sigma only

model = LinearRegression()
model.fit(X_train_scaled, y_train)
y_pred = model.predict(X_test_scaled)

# Compare y_pred vs y_test; compute MAE, MSE, RMSE, R²
# Repeat with other single columns (e.g. Administration) and compare MAE
# MLR: all spend columns (+ encode state if used) with Profit as y
```

### Correlation check (conceptual)

```python
# Instructor used LLM-assisted snippet; typical approach:
df["Profit"].corr(df["Marketing Spend"])
df["Profit"].corr(df["R&D Spend"])
```

### Feature scaling formulas

```text
Standardization:  x' = (x - mu) / sigma
Normalization:    x' = (x - x_min) / (x_max - x_min)
```

### Classification accuracy toy example

```text
# Actual test labels (5 records): active, churn, churn, churn, churn
# Model 1 preds: active, active, active, churn, churn  -> 3/5 correct -> 60%
# Model 2 preds: active, churn, active, churn, churn   -> 4/5 correct -> 80% -> preferred
```

## Q&A / Discussion Points

- **Regression recap:** Predicting a **numerical** column (temperature, price, profit).
- **Why reshape?** Sklearn expects 2D feature arrays; 1D raises “Expected 2D, got 1D” before or during `fit`.
- **Same scaling technique for height and weight?** Yes—both columns scaled; only two standard approaches (standardization vs. normalization) via `StandardScaler` / `MinMaxScaler`.
- **`fit_transform` on train but `transform` on test?** \(\mu\) and \(\sigma\) computed from **training** only; test rows scaled with those same parameters to mimic production.
- **Normalization vs. standardization:** Theory ties to Gaussian vs. non-Gaussian; practically instructor uses either, often StandardScaler, without rerunning full pipelines to compare.
- **Where to download files?** Dataset link shared in class; Colab notebook to be on SharePoint via Codecademy—upload CSV to Colab, not Excel-only for the notebook itself.
- **Reshape (after break):** Re-explained for Rohan—libraries require 2D input.

## Action Items & Assignments

- [ ] Check **assignment answers** on SharePoint; validate yourself; raise wrong answers in **doubt-clearing class**
- [ ] Download **50 Startups** dataset from the link shared in session
- [ ] Upload data to **Google Colab** and run the **regression notebook** line by line
- [ ] Review **EDA lecture** for feature scaling (standardization/normalization) depth
- [ ] Attend **next class**: KNN, decision trees, random forest + classification practicals
- [ ] Instructor to share **course materials** (parallel self-study) around the next session to connect concepts
- [ ] Optional: use doubt-clearing for **code walkthrough** if notebook is unclear

## Open Questions / Unclear Spots

- Exact **train/test split** parameters (`test_size`, `random_state`) and full evaluation code block were referenced but not fully dictated in the transcript—follow the shared notebook.
- **State** column (categorical: New York, California, Florida) was described in `info()`; how it enters **multiple linear regression** (encoding) was not fully walked through in the live portion—likely in notebook tail.
- Session **ended early** (~9 minutes left): **KNN, decision trees, random forest** and classification practicals explicitly moved to **tomorrow**.
- Poll/feedback aside: no technical open items from that thread.
