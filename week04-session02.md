# Classification, Ensemble Learning & Customer Churn Practical

## TL;DR
This session deep-dived into supervised classification: KNN, decision trees, bagging/random forest, and boosting (including XGBoost), with emphasis on when foundational models fall short and ensembles usually win. The second half was a hands-on telecom customer churn workflow in Google Colab—encoding, train/test split, scaling, and comparing classifiers—with a short intro to hyperparameter tuning and neural-network basics. Unsupervised learning, transfer learning, and fuller deep learning were deferred to Thursday’s doubt-clearing session because next class starts NLP.

## Topics Covered
- Recap: classification vs regression, known class labels
- K-Nearest Neighbors (KNN): Euclidean distance, voting, odd *K* for binary classification
- Taxonomy of classification algorithms (basic vs advanced)
- Decision trees: intuitive job-offer analogy, root/leaf nodes, `criterion` (Gini vs entropy)
- Ensemble learning: bagging vs boosting
- Random forest: why “random,” why “forest,” bootstrap of features/rows, `n_estimators`
- Boosting pipeline and XGBoost performance expectations
- Break; shared notebook/data via Google Drive (later SharePoint)
- Customer churn dataset: schema, tenure assumption, churn rate (~26.5%)
- Feature encoding: label encoding vs one-hot vs dummy encoding
- End-to-end ML pipeline in Colab: clean → X/y → encode → split → scale → models → accuracy
- Hyperparameter optimization: `RandomizedSearchCV` vs `GridSearchCV`
- Brief deep learning: neurons, input/hidden/output layers, epochs
- Curriculum alignment, Thursday carryover topics, interview/agent Q&A

## Key Concepts Explained

### Classification
- What it is: Predicting which **known category** (class) a record belongs to—e.g., fraud vs not fraud, churn vs active.
- Why it matters: Core supervised task before NLP/generative work; many business problems are classification.
- Instructor emphasized: Classes are fixed upfront; you cannot predict labels outside the training set’s categories.

### K-Nearest Neighbors (KNN)
- What it is: For a new point, find the *K* closest training points (by distance, typically **Euclidean**) and classify by **majority vote** among their labels.
- Why it matters: Simple, interpretable baseline; illustrates distance-based reasoning in feature space.
- Caveats: With many features you cannot visualize plots, but distance is computed per axis; for **binary** classification use **odd** *K* (3, 5, 7…) to avoid ties; foundational but rarely the best production model.

### Euclidean Distance
- What it is: \(\sqrt{(x_1-x_2)^2 + (y_1-y_2)^2}\) between two points—recapped with example (3,4) vs (8,6) → √29 ≈ 5.38.
- Why it matters: Distance metric underlying KNN (and neighbor search in general).

### Decision Tree
- What it is: A tree of **if/else splits** on features; top node = **root**, splits end in **leaf** decisions; built automatically from data (e.g., churn active/inactive).
- Why it matters: Mirrors human hierarchical decisions; strong teaching model and building block for forests.
- Caveats: Split choice math (which feature/threshold first) was **not** covered in depth—refer to shared ML videos; in scikit-learn `DecisionTreeClassifier`, `criterion` defaults to **`gini`**, can set **`entropy`**.

### Ensemble Learning
- What it is: **Hybrid** approach combining multiple models instead of a single standalone learner.
- Why it matters: Standalone KNN, decision tree, SVM, Naive Bayes often underperform; ensembles commonly win on real tabular data.
- Two branches: **bagging** and **boosting**.

### Bagging
- What it is: Train several models (“**bags**”) on train data; each predicts on test rows; **majority vote** on predictions (e.g., 3 say churn, 1 says active → churn).
- Why it matters: Voting reduces variance; usually beats individual weak models.
- Instructor tied this to customer churn example: 800 train / 200 test, multiple model types each producing `y_pred`, then vote per test record.

### Random Forest
- What it is: Bagging where **every bag is a decision tree** (“**forest**”); **randomness** = each tree sees a **random subset of rows and features** (bootstrap), so trees differ even if same algorithm.
- Why it matters: Often much better than a single decision tree; can surface rare informative rows or features (e.g., `age`) in some trees; hundreds of trees typical (`n_estimators` default **100**, instructor used **1000** in demo).
- Caveats: Rows can repeat across bags; each tree does **not** see full data; prediction on new records runs through all trees then votes; internal per-tree predictions are hard to inspect; “RF beats single DT” is ~99.99% true but not absolute.

### Boosting
- What it is: Sequential bags; **misclassified** training samples are emphasized/passed into the next learner with added randomness so later models focus on hard cases.
- Why it matters: Often top tier on tabular data; XGBoost dominated many Kaggle-era competitions (~2018–2020).
- Thumb rule stated: worst ≈ KNN/DT/SVM → better ≈ bagging/RF → often best ≈ boosting (XGBoost, etc.)—but **always try multiple models** on real projects.

### Feature Encoding
- What it is: Converting categoricals to numbers models can use.
- **Label encoding**: map categories to integers (OK for binary M/F); **not** recommended for >2 unordered categories (model may treat 3 > 2 > 1 falsely).
- **One-hot encoding**: one binary column per category value.
- **Dummy encoding**: one-hot then **drop one column** (N categories → N−1 columns) to avoid redundancy/multicollinearity.
- Covered in instructor’s “ADA” YouTube series; `pandas.get_dummies` used in notebook.

### Train/Test Split & `random_state`
- What it is: Split features/labels into train and test; pass either `train_size` **or** `test_size` to `train_test_split`, not both (e.g., `test_size=0.2` ≡ 80/20).
- `random_state=42` fixes the split for reproducibility.

### Feature Scaling (StandardScaler)
- What it is: `fit_transform` on **training** data only; `transform` on **test** (and inference) to avoid leakage.
- Why it matters: Required for distance-based models like KNN; applied before KNN in the practical.

### Hyperparameter Optimization
- What it is: Searching combinations of model parameters (e.g., RF `n_estimators`, `max_depth`, `criterion`, `min_samples_split`) to beat defaults.
- **RandomizedSearchCV**: sample `n_iter` random combos from a grid (faster).
- **GridSearchCV**: exhaust all combinations in the defined grid (slower).
- Workflow suggested: broad randomized search → narrow ranges → fine grid; very **expensive** (60 combos × 10s ≈ 10 minutes in example).
- Instructor noted RF with tuning could reach ~**85–87%** accuracy on this telecom churn problem vs ~80% out of the box.

### Deep Learning (Brief)
- What it is: Stacks of **neurons**—**input layer**, **hidden layers**, **output layer**; training passes data through the network repeatedly (epochs) before inference.
- Use cases mentioned: image/voice classification, segmentation, detection, translation/summarization (now often grouped under generative AI).
- Example: X-ray → COVID vs non-COVID at output layer.
- More detail promised in transformers/NLP sessions and Thursday doubt-clearing.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| scikit-learn (`sklearn`) | `KNeighborsClassifier`, `DecisionTreeClassifier`, `RandomForestClassifier`, `train_test_split`, `StandardScaler`, `accuracy_score`, `RandomizedSearchCV`, `GridSearchCV` |
| pandas | `read_csv`, `df.info`, `head`, `fillna`, `get_dummies` |
| NumPy, Matplotlib | Standard imports in notebook |
| Google Colab | Runtime for class practicals |
| XGBoost, CatBoost, AdaBoost | Boosting family; AdaBoost hit **81%** in notebook |
| Gradient Boosting / Extreme Gradient Boosting | Mentioned in algorithm list |
| Naive Bayes, SVM, Logistic Regression | Listed as basic classifiers |
| Google Drive / SharePoint | Class files shared temporarily on Drive; Codecademy team uploads to SharePoint |
| Udemy (instructor DL & time series courses) | Free coupon codes promised for DL and time series |
| YouTube “ADA” series (instructor name) | Feature encoding & 6hr churn EDA on same dataset |
| Groq, ChatGPT | Suggested for EDA help on dataset |
| Excel | Instructor used for quick churn-rate chart |

## Code & Examples

### Euclidean distance (conceptual)
```python
# Distance between (3, 4) and (8, 6)
import math
math.sqrt((3 - 8)**2 + (4 - 6)**2)  # sqrt(29) ≈ 5.38
```

### KNN classifier (as walked through)
```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score

knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)
y_pred_knn = knn.predict(X_test)
accuracy_score(y_test, y_pred_knn)  # ~76% in session
```

### Decision tree & random forest (same pattern)
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier

dt = DecisionTreeClassifier()  # default params; ~72% accuracy
dt.fit(X_train, y_train)
y_pred_dt = dt.predict(X_test)

rf = RandomForestClassifier(n_estimators=1000)  # ~80% accuracy; slower
rf.fit(X_train, y_train)
y_pred_rf = rf.predict(X_test)
```

### Preprocessing pipeline (approximate order from transcript)
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

df = pd.read_csv("churn.csv")  # filename as shared in class

# Fix total_charges dtype and nulls
df["TotalCharges"] = pd.to_numeric(df["TotalCharges"], errors="coerce")
df["TotalCharges"] = df["TotalCharges"].fillna(...)  # 11 nulls imputed

# Features / target
X = df.drop(columns=["customerID", "Churn"])
y = df["Churn"]

# One-hot encode before split (instructor did encode then split)
X_encoded = pd.get_dummies(X)

X_train, X_test, y_train, y_test = train_test_split(
    X_encoded, y, test_size=0.2, random_state=42
)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Hyperparameter search sketch
```python
from sklearn.model_selection import RandomizedSearchCV, GridSearchCV

param_grid = {
    "criterion": ["gini", "entropy"],
    "max_depth": [50, 100, 200],
    "n_estimators": [100, 500, 1000, 1500, 2000],
    "min_samples_split": [2, 3, 4],
    # ... other RF params often left default
}
# 2×2×3×5 = 60 combinations in instructor example

random_search = RandomizedSearchCV(
    RandomForestClassifier(), param_grid, n_iter=20, ...
)
# GridSearchCV runs all combinations in param_grid
```

### Tenure validation (EDA logic)
```text
total_charges ≈ monthly_charges × tenure (months)
# e.g. 29.75 × 10 ≈ 297 vs recorded total charges — close match
```

*Note: Exact `fillna` strategy, column names, and hyperparameter dict in the notebook may differ slightly; follow the shared Colab file.*

## Q&A / Discussion Points
- **Why are all random forest trees decision trees, and won’t they predict the same?** No—each tree trains on different random subsets of rows and features, so structure and root splits differ.
- **New record (e.g., 1001st customer) inference?** Goes through preprocessing/scaling, every tree predicts, majority vote on churn vs active; you don’t see individual tree outputs easily.
- **Google Drive download issues (AJ):** Instructor may add AI-generated notebook descriptions; not a blocker—use AI to explain notebook goals if needed.
- **Curriculum week 5 subtopics out of sync?** Typos in curriculum; instructor covers listed items over time—ignore mismatched subtopic list.
- **Implement classification/regression in agents?** No—these are conceptual foundations, not used directly in generative/agentic builds.
- **ML/DL in AI engineering interviews?** Depends on JD; GenAI-only roles may ask less, but instructor expects baseline ML/DL knowledge for AI engineering roles and may drill candidates who lack it.
- **Overfitting during training?** Yes—models should be checked for overfitting in training process (acknowledged, not demoed in depth).
- **When do projects/practicals ramp up?** Week 6 onward more project-focused practicals expected.
- **Thursday vs teach now (unsupervised/transfer learning)?** Class voted to defer; instructor will cover Thursday.

## Action Items & Assignments
- [ ] Download class notebook and dataset from shared **Google Drive** link; set up on **Google Colab**
- [ ] Watch instructor **ML videos** (to be shared post-class; ML content is part of AI series—not a separate course yet)
- [ ] Use **Udemy coupon** for instructor’s **deep learning** and **time series** courses (DL + time series promised today; full ML access by ~next day)
- [ ] Review **ADA** YouTube videos for feature encoding and optional **6-hour** churn EDA on the same dataset
- [ ] Attend **Thursday doubt-clearing** for: unsupervised learning (high level), transfer learning, pre-trained models, token usage, model selection strategies
- [ ] Before **next Saturday**, watch Thursday recording if you miss live—then join **NLP / word embeddings** session
- [ ] Prepare for **top ML/DL interview questions** instructor may share near end of bootcamp
- [ ] On real projects: **hit-and-trial** multiple algorithms; don’t assume XGBoost always wins

## Open Questions / Unclear Spots
- Recording briefly stopped/restarted mid-session (~hyperparameter combinatorics)—verify full video if that segment matters.
- Exact **`fillna`** rule for 11 `TotalCharges` nulls was not fully spoken in transcript.
- **Bayesian optimization** mentioned as optional third HPO approach—not covered.
- **Unsupervised/clustering** content skipped today; only promised Thursday high-level theory.
- **Transfer learning & pre-trained models** deferred; “token usage” listed for Thursday without detail in this session.
- [unclear: "ADA" video series] — instructor’s YouTube channel name spelling; search instructor name + “ADA” as stated in class.
