# Doubt-Clearing: K-Means Clustering, Elbow Method & Transfer Learning

## TL;DR

This session was primarily a doubt-clearing class, but with few questions the instructor spent ~15–20 minutes on unsupervised **clustering** (K-means and the elbow method) using a mall customer segmentation example, then covered **transfer learning** and **pre-trained models** for deep learning when labeled data is scarce. The practical thread emphasized that clustering has no single “correct” K—you choose K using the elbow method plus visualization and domain judgment. Logistics included an upcoming RAG assignment, a deep learning session on Saturday, and a bootcamp target end date of June 28, 2026.

## Topics Covered

- Session format: doubt-clearing via Q&A; optional ML topics if no questions
- ML taxonomy recap: predictive (classification, regression) vs non-predictive / unsupervised (clustering, association)
- Customer segmentation use case: annual income vs spending score (no labels)
- Clustering overview; focus on **K-means** (not hierarchical, DBSCAN, Mean Shift in depth)
- Choosing **K**: elbow method and **WCSS** (within-cluster sum of squares)
- K-means algorithm walkthrough on 2D plots (random centroids → assign → recompute centroids → repeat until stable)
- Code demo: `mall.csv` (annual income, spending score), elbow plot (K = 1–15), cluster visualizations for K = 5, 6, 7
- Business interpretation of five clusters and prioritization for marketing
- Predicting cluster membership for new customers
- Clustering applications: cross-selling
- **Pre-trained models** and **transfer learning** (COVID X-ray example)
- Course logistics: assignments, Saturday ML/DL content, bootcamp schedule, instructor personal update

## Key Concepts Explained

### Unsupervised learning & clustering

- What it is: Learning patterns from data **without** class labels—grouping similar records (e.g., customers with similar income and spending behavior).
- Why it matters / when to use it: Use when the problem is segmentation, exploration, or grouping (e.g., customer segmentation) and you do not have predefined categories.
- Caveats: There is no objectively “right” number of groups; outcomes depend on K and business context. The instructor stressed that in clustering, **nothing is strictly right or wrong** in the way supervised classification can be evaluated.

### K-means clustering

- What it is: Partitions data into **K** clusters by minimizing distance from each point to its cluster **centroid**; **K** = number of clusters.
- Why it matters / when to use it: Common, practical choice for segmentation on numeric features; works in high dimensions even when you cannot plot them (e.g., 20 features).
- Caveats: You must choose K first (often via elbow method). Initial centroids are random, so runs can differ; algorithm iterates until assignments/centroids stabilize. Instructor called K-means one of the best / most important clustering algorithms for this curriculum.

### Elbow method & WCSS

- What it is: **WCSS** = within-cluster sum of squares (total squared distance from points to their cluster centroids). Plot WCSS vs **K**; the “elbow” is where adding more clusters yields diminishing reduction in WCSS.
- Why it matters / when to use it: Heuristic to pick a reasonable K before or alongside visual inspection.
- Caveats: With more clusters, WCSS always decreases (down to ~0 if each point is its own cluster). The elbow point is subjective; instructor said to treat the elbow K as a hint and explore **±2** values (e.g., if elbow suggests 5, try 3–7 and compare visualizations).

### Customer segmentation (mall example)

- What it is: Two features—**annual income** and **spending score**—used to discover customer groups for prioritization (e.g., high income + high spend = top tier for cross-sell).
- Why it matters / when to use it: Marketing and product strategy when labels are not provided but similarity groups are useful.
- Caveats: Five clusters looked best visually and for business storytelling, but six or seven are defensible; domain knowledge should drive the final K.

### Transfer learning & pre-trained models

- What it is: **Pre-trained models** are networks already trained on large, diverse datasets (e.g., thousands of image classes). **Transfer learning** reuses that “knowledge” (early layers / representations) and adds task-specific layers for a new problem with limited data.
- Why it matters / when to use it: When your dataset is too small to train a deep model from scratch (e.g., ~100 COVID vs non-COVID X-rays → ~60–70% accuracy alone); stacking a pre-trained backbone can reach ~80–85% in the instructor’s example.
- Caveats: Pre-trained weights may not include your exact domain (e.g., no COVID in ImageNet), but generic visual features still help. Deeper treatment and projects deferred to the Saturday deep learning session.

### Cross-selling

- What it is: Recommending products to customers similar to those who already bought (e.g., same policy sold to similar profiles).
- Why it matters / when to use it: Clustering finds “similar in nature” customers (age, location, salary, etc.) to target pitches.
- Caveats: Clustering supports similarity discovery; business rules and compliance still apply in real deployments.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| K-means | Main clustering algorithm taught; random initial centroids, iterative reassignment |
| Elbow method | WCSS vs K plot to suggest optimal cluster count |
| WCSS | Within-cluster sum of squares; decreases as K increases |
| `mall.csv` | Dataset with at least **Annual Income** and **Spending Score** (two columns used for demo) |
| Python clustering code | Loop K from 1 to 15, fit K-means per K, collect WCSS, plot elbow; then fit K-means with chosen K and visualize |
| Pre-trained CNN architectures | ResNet, MobileNet, Inception v3/v4/v5, Xception, GoogleNet, AlexNet, VGG mentioned; instructor has used several in practice |
| Discord | Channel for doubts and course codes |
| Poll | Used during class for elbow-point guess on elbow plot |

## Code & Examples

### Elbow method (approximate from transcript)

The instructor described code that builds a list of WCSS values for K = 1 through 15, then plots them.

```python
wcss = []
for i in range(1, 16):  # K from 1 to 15
    # Fit K-means with i clusters; compute/store WCSS for this K
    # Append WCSS to wcss
    pass
# Plot wcss vs K to find the elbow
```

On the shown plot, the elbow was around **K = 5**; students were told to still try **3, 4, 5, 6, 7** and pick the visualization that fits the problem best.

### K-means workflow (algorithm steps)

```text
1. Choose K (e.g., via elbow method + domain sense).
2. Place K random centroids.
3. Assign each point to the nearest centroid.
4. Recompute centroid of each cluster (mean of assigned points).
5. Reassign points to nearest new centroids.
6. Repeat steps 4–5 until centroids (and assignments) no longer change.
```

### Mall customer demo

- **Features:** `Annual Income`, `Spending Score` only (for 2D plots).
- **K = 5** chosen after comparing plots for 5 vs 6 vs 7 and mapping clusters to business segments (e.g., low income + high spend, high income + high spend = “king” tier).
- **New customer:** Subsequent code predicts which cluster a new observation belongs to (e.g., “cluster 2”) for targeting.

Exact library calls (`sklearn.cluster.KMeans`, etc.) were not spelled out in the transcript; the logic above matches what was demonstrated.

## Q&A / Discussion Points

- **Why not K = 7?** Instructor: seven is fine; unsupervised clustering has no single correct answer—five, six, or seven are all acceptable if justified. Goal is actionable segments (e.g., where to nudge / cross-sell new customers), not a unique ground-truth K.
- **No doubts during session:** Class wrapped early; students encouraged to post doubts on Discord or bring topics next time.
- **ML course videos / codes:** Instructor will share on **Saturday’s** class; deep learning and time series material is ready; ML video access still being figured out.
- **Next assignment timing:** RAG / generative AI assignment expected after about **two weeks** (instructor referenced week 5 / week 6 progression in transcript—confirm on Discord).
- **Bootcamp end date:** Target completion **June 28, 2026** (~8 weeks left), with possible break in May or June; topics still to cover include APIs, fine-tuning, RAG/framework sessions on scheduled dates (16th–17th, 23rd–24th mentioned in passing).

## Action Items & Assignments

- [ ] Review clustering notebook/material (instructor will keep resources in **week 4** folder).
- [ ] Attend **Saturday** class for deep learning content (transfer learning in more depth, practical projects).
- [ ] Expect **RAG / generative AI assignment** in roughly two weeks.
- [ ] Post doubts on **Discord** if you have them before the next session.
- [ ] Next sessions: bring doubts or suggest topics; otherwise instructor may lecture on optional ML topics.

## Open Questions / Unclear Spots

- Exact filenames, repo paths, and full Python source for elbow/K-means were referenced (`small.csv` / sample CSV) but not fully reproduced in the transcript.
- Assignment week wording was ambiguous (“week five will start at the end of week six”)—confirm exact due date on Discord or course portal.
- Instructor mentioned **Inception v5** among model names; common public versions are often v3/v4—may have been speech recognition error.
- Hierarchical clustering, DBSCAN, and Mean Shift were named but not taught in this session.
- Fine-tuning was distinguished from pre-training (“pretrained, not fine-tuning” for this segment); fine-tuning scheduled later in the bootcamp (16th–17th mentioned).
