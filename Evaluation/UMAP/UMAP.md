# UMAP Analysis of Learned ResNet50 Brain MRI Representations

## 1. Overview

This figure shows a **2-dimensional UMAP projection of the feature representations learned by a ResNet50 model** for brain MRI images.

The visualization does **not** show the original MRI images. Instead, it shows how the trained neural network represents those images internally in its learned feature space.

Each point represents **one MRI image**. Images with similar learned representations tend to appear near one another, while images with different representations tend to appear farther apart.

The four colors correspond to the four MRI classes:

| Color | Class |
|---|---|
| 🔵 Blue | Glioma |
| 🟠 Orange | Meningioma |
| 🟢 Green | No Tumor |
| 🔴 Red | Pituitary |

The main purpose of this visualization is to investigate whether the ResNet50 embedding space naturally organizes MRI images according to their diagnostic class.

---

## 2. What Exactly Is This Figure Showing?

The figure visualizes the **learned feature space of ResNet50**.

The overall pipeline can be represented as:

```text
Brain MRI
    │
    ▼
ResNet50
    │
    ├── Early layers → low-level visual features
    │
    ├── Middle layers → patterns and structures
    │
    └── Deep layers → high-level representations
    │
    ▼
2048-dimensional embedding
    │
    ▼
UMAP
    │
    ▼
2-dimensional representation
    │
    ▼
UMAP plot
```

In this project, each MRI image is transformed by ResNet50 into a **2048-dimensional feature vector**.

UMAP then reduces those 2048 dimensions to only two dimensions so that the representation space can be visualized.

Therefore, this figure answers the question:

> **How does ResNet50 organize brain MRI images in its learned feature space?**

It does **not** directly answer:

> How do the original MRI images look?

---

## 3. What Is an Embedding?

An **embedding** is a numerical representation of an input.

Instead of representing an MRI only as a matrix of pixel intensities, ResNet50 transforms the image into a high-dimensional feature vector.

Conceptually:

```text
MRI image
    ↓
ResNet50
    ↓
[feature₁, feature₂, feature₃, ..., feature₂₀₄₈]
```

In this project, the embedding has **2048 dimensions**.

The individual dimensions should not necessarily be interpreted as simple human-readable concepts such as:

- tumor size
- tumor location
- tumor shape

Instead, the 2048 values collectively represent patterns learned by the neural network.

These representations are useful because the network learns to encode visual information that helps distinguish the different MRI categories.

---

## 4. Why Are the Embeddings 2048-Dimensional?

ResNet50 contains a deep convolutional architecture.

After the convolutional feature-extraction layers and global average pooling, the network produces a feature vector with **2048 values**.

Conceptually:

```text
Input MRI
   ↓
Convolutional layers
   ↓
Deep feature maps
   ↓
Global Average Pooling
   ↓
2048-dimensional embedding
```

The final 2048-dimensional representation contains information extracted from the image by the network.

This representation is much more compact than the original image while still containing high-level information learned by the model.

---

## 5. Why Do We Need UMAP?

A 2048-dimensional vector cannot be directly visualized on a normal 2D plot.

UMAP (**Uniform Manifold Approximation and Projection**) is a dimensionality-reduction technique that maps high-dimensional data into a lower-dimensional space.

Here:

```text
2048 dimensions
       ↓
      UMAP
       ↓
2 dimensions
```

The result is represented by:

- **UMAP Dimension 1**
- **UMAP Dimension 2**

These two dimensions are artificial coordinates created by UMAP.

They are **not medical measurements**.

For example, it would be incorrect to interpret:

> UMAP Dimension 1 = tumor size

or:

> UMAP Dimension 2 = tumor severity

The axes simply provide coordinates that allow the high-dimensional relationships between samples to be visualized.

---

## 6. What Does One Dot Represent?

Every dot in the figure represents **one MRI sample**.

The process for one image is:

```text
One MRI image
      ↓
ResNet50
      ↓
2048-dimensional embedding
      ↓
UMAP
      ↓
One point in the 2D plot
```

The **color** of the point represents the true diagnostic class.

Therefore:

- 🔵 Blue → Glioma
- 🟠 Orange → Meningioma
- 🟢 Green → No Tumor
- 🔴 Red → Pituitary

---

## 7. What Does Distance Between Points Mean?

A major purpose of UMAP is to visualize relationships between samples.

If two points are close together, their learned representations are relatively similar in the structure captured by UMAP.

If two points are far apart, their representations are more different.

However, UMAP primarily emphasizes **local neighborhood structure**.

Therefore:

> Local relationships and clusters are generally more informative than interpreting exact global distances between arbitrary points.

It is safer to say:

> "These samples form a similar local neighborhood."

than:

> "These two samples are exactly twice as similar as those two samples."

---

# 8. Overall Structure of the Figure

The figure shows several important patterns.

The most visually obvious observation is that the **no-tumor class is relatively well separated from many tumor samples**.

The tumor classes, particularly **glioma and meningioma**, show substantially more overlap.

The **pituitary** class also appears in several different regions rather than forming one single compact cluster.

Therefore, the overall embedding can be described as:

> **Structured, partially class-separable, but not perfectly separated.**

---

# 9. Analysis of the No-Tumor Class

The **green points** represent the no-tumor class.

This is the most visually separated class in the figure.

A large proportion of the green embeddings occupy the **right-hand and upper regions** of the UMAP space.

Several dense green structures can be seen approximately around:

```text
UMAP Dimension 1 ≈ 0 to 4.5
UMAP Dimension 2 ≈ 10.5 to 12.8
```

There are also several smaller green groups distributed throughout this region.

### Interpretation

The model appears to have learned representations that distinguish many normal brain MRI images from tumor-containing images.

This suggests that the learned feature space captures characteristics associated with normal brain anatomy.

The relatively strong separation of the green class is one of the clearest observations in this UMAP.

### Important limitation

The no-tumor class is **not completely isolated**.

Some green points occur in regions containing blue, orange, or red points.

Therefore, the learned representation is not perfectly class-separated.

---

# 10. Why Might No-Tumor Images Be More Separated?

One possible interpretation is that the presence or absence of tumor-related visual patterns creates a relatively strong difference in the learned representations.

Conceptually:

```text
Normal anatomy
      ↓
Distinct feature patterns
      ↓
Separate region of embedding space
```

while tumor images may share more visual characteristics with each other:

```text
Glioma ─────┐
            ├── shared visual characteristics
Meningioma ─┤
            │
Pituitary ──┘
```

This is only a representation-level interpretation.

The UMAP alone cannot establish which exact anatomical or pathological characteristics caused the separation.

---

# 11. Analysis of the Pituitary Class

The **red points** represent pituitary tumor images.

One of the interesting properties of the red class is that it does **not form one single compact cluster**.

Instead, several red-dominated regions are visible.

For example, there is a prominent red structure around:

```text
UMAP Dimension 2 ≈ 6.2–7.5
```

and another red-dominated region around:

```text
UMAP Dimension 2 ≈ 10.0–10.6
```

There are also red samples distributed into central regions of the plot.

### Interpretation

This suggests that the pituitary class is **heterogeneous in the learned feature space**.

In other words, ResNet50 does not represent every pituitary MRI as exactly the same type of feature pattern.

Instead, the class appears to contain several subgroups.

Possible explanations include:

- anatomical variation
- differences in tumor appearance
- differences in tumor size
- differences in tumor location
- MRI acquisition differences
- image-quality differences
- orientation differences
- preprocessing variation

However, these are **possible explanations**, not conclusions that can be proven from UMAP alone.

---

# 12. Multiple Clusters Do Not Necessarily Mean Poor Performance

A class forming several clusters is not automatically evidence that the model is performing poorly.

A single diagnostic class can contain many visually different examples.

For example:

```text
Pituitary
   │
   ├── subgroup A
   ├── subgroup B
   └── subgroup C
```

The network may learn different representations for these visually different samples while still correctly assigning them to the same class.

Therefore:

> **One class does not necessarily correspond to one geometric cluster.**

This is an important representation-learning observation.

---

# 13. Analysis of the Glioma Class

The **blue points** represent glioma.

The glioma class is distributed primarily throughout the **left and central regions** of the embedding space.

There are relatively dense blue regions around:

```text
UMAP Dimension 1 ≈ -2.8 to -0.5
UMAP Dimension 2 ≈ 7.5 to 9.2
```

and another substantial blue region around:

```text
UMAP Dimension 1 ≈ -2.3 to -0.7
UMAP Dimension 2 ≈ 11.0 to 12.5
```

However, the blue points also overlap substantially with orange points.

### Interpretation

Glioma does not form one perfectly isolated region.

Instead, many glioma samples occupy regions shared with meningioma samples.

This indicates that the learned representations of some glioma and meningioma images are relatively similar.

---

# 14. Analysis of the Meningioma Class

The **orange points** represent meningioma.

Meningioma samples are distributed widely throughout the left and central portions of the embedding space.

There is a noticeable orange concentration in the upper-left region:

```text
UMAP Dimension 1 ≈ -1.7 to -0.8
UMAP Dimension 2 ≈ 12.0 to 13.0
```

There are also many orange samples around:

```text
UMAP Dimension 1 ≈ -2.5 to 0.5
UMAP Dimension 2 ≈ 8.0 to 11.0
```

### Main observation

Meningioma has considerable overlap with glioma.

This is one of the most important characteristics of the figure.

The network has not transformed these two classes into two completely independent geometric regions.

Instead, their representations remain partially intertwined.

---

# 15. Glioma vs. Meningioma

The overlap between blue and orange points is particularly important.

In an idealized perfectly separated embedding:

```text
Glioma                  Meningioma

● ● ● ●                 ○ ○ ○ ○
● ● ● ●                 ○ ○ ○ ○
● ● ● ●                 ○ ○ ○ ○
```

we would see clearly separated regions.

In the actual UMAP:

```text
Glioma + Meningioma

🔵 🟠 🔵 🟠 🟠
🟠 🔵 🟠 🔵 🟠
🔵 🟠 🔵 🟠 🔵
🟠 🔵 🟠 🔵 🟠
```

the two classes frequently occupy similar regions.

### Interpretation

The ResNet50 embedding contains useful information for distinguishing the classes, but the representation is **not completely disentangled**.

Some glioma and meningioma images appear to have similar learned feature representations.

This may be related to visual similarity between some tumor images or to limitations of the learned representation.

---

# 16. Central Overlap Region

A large portion of the central-left area contains a mixture of:

- glioma
- meningioma
- some pituitary samples

This region is especially important for representation analysis.

Approximately:

```text
UMAP Dimension 1 ≈ -2.5 to -0.5
UMAP Dimension 2 ≈ 8.0 to 10.5
```

contains substantial mixing.

### Interpretation

The model considers many samples in this region to have similar learned representations.

However, this does **not** mean that the images are clinically identical.

It only means that their learned feature vectors have similar local relationships in the embedding space.

---

# 17. What Does Class Overlap Mean?

Suppose a blue glioma point appears next to an orange meningioma point.

This means:

> Their learned representations are similar enough to appear in the same local region of the UMAP projection.

It does **not** necessarily mean:

- the images are identical;
- the diagnoses are clinically similar;
- the model misclassified one of them;
- the patients have similar clinical characteristics.

UMAP is showing **representation similarity**, not clinical similarity.

---

# 18. What Does Class Separation Mean?

When two classes occupy different regions, it suggests that their learned representations have different structures.

The strongest example in this figure is:

```text
No Tumor
     ↕
Many tumor samples
```

The green no-tumor samples are concentrated largely in a different region from many tumor samples.

This suggests that the model has learned features that distinguish normal from many abnormal images.

---

# 19. Intra-Class Variation

Another important property of the figure is **intra-class variation**.

Intra-class variation means:

> How different are samples belonging to the same class from each other?

For example, if all pituitary samples formed one tiny cluster, the class would have low representation variation.

Instead, the red samples occupy several regions.

Therefore, the pituitary class appears to have relatively high intra-class variation.

Similar behavior can also be observed for other classes.

This is important because medical image datasets often contain substantial variation even within the same diagnosis.

---

# 20. Inter-Class Variation

**Inter-class variation** refers to differences between classes.

The figure suggests that inter-class variation is not uniform.

For example:

```text
No Tumor ↔ Tumor
       ↑
   relatively strong separation

Glioma ↔ Meningioma
       ↑
   substantial overlap
```

Therefore, the model appears to find some diagnostic distinctions easier than others.

---

# 21. UMAP Axes Do Not Have Medical Meaning

The x-axis:

```text
UMAP Dimension 1
```

and y-axis:

```text
UMAP Dimension 2
```

are not medical variables.

Do **not** interpret them as:

```text
UMAP 1 = tumor severity
UMAP 2 = tumor size
```

or:

```text
higher UMAP 2 = more abnormal
```

There is no basis for such interpretations.

The axes are constructed by UMAP to preserve aspects of the relationships present in the original high-dimensional embedding space.

---

# 22. UMAP Is a Projection, Not the Original Feature Space

The original feature representation is:

```text
2048 dimensions
```

The visualization is:

```text
2 dimensions
```

Therefore:

```text
2048D feature space
        ↓
     UMAP
        ↓
      2D plot
```

Information is inevitably lost during dimensionality reduction.

The plot should therefore be treated as a **visual approximation** of the original embedding geometry.

It should not be treated as the complete representation of the model's internal feature space.

---

# 23. Why Does the Plot Look Complicated?

A common expectation is:

```text
Class A → one cluster
Class B → one cluster
Class C → one cluster
Class D → one cluster
```

But deep neural networks do not necessarily learn such simple geometry.

Instead, the representation can contain:

- multiple subclusters
- overlapping classes
- local neighborhoods
- outliers
- continuous transitions
- heterogeneous structures

The current figure is therefore more realistic than a perfectly separated four-cluster visualization.

---

# 24. What Does This Say About the ResNet50 Representation?

Overall, the visualization suggests that the ResNet50 has learned a **meaningful and structured representation**.

The points are not randomly distributed.

Instead, there are clear class-related structures.

The strongest evidence is:

1. The no-tumor class occupies a relatively distinct region.
2. Pituitary samples form several recognizable subregions.
3. Glioma and meningioma form substantial but overlapping structures.
4. Multiple local neighborhoods appear within individual classes.

This indicates that the learned embedding contains meaningful information about the MRI categories.

---

# 25. What We Can Reasonably Conclude

Based on the UMAP visualization, we can reasonably state:

### 1. The embedding contains diagnostic structure

The learned representations are organized according to class to a meaningful degree.

### 2. No-tumor samples are relatively well separated

The green class is concentrated mainly in the right and upper regions.

### 3. Glioma and meningioma overlap substantially

The blue and orange classes are not completely separated.

### 4. Pituitary representations are heterogeneous

The red class forms several different local structures.

### 5. Class separation is incomplete

Multiple classes occupy some of the same regions.

### 6. The model has learned both inter-class and intra-class structure

Different classes have different distributions, while individual classes themselves contain multiple subgroups.

---

# 26. What We Cannot Conclude From UMAP Alone

The visualization does **not** prove:

- why a specific MRI was misclassified;
- whether a cluster represents a particular tumor grade;
- whether a cluster corresponds to tumor size;
- whether a cluster corresponds to patient demographics;
- whether a cluster has clinical significance;
- whether the model is clinically reliable;
- whether the representation is medically meaningful;
- whether one class is intrinsically harder to diagnose.

Those questions require additional experiments and metadata.

---

# 27. UMAP Is Not a Classification Metric

UMAP should not be used as a replacement for classification metrics.

For example:

> "The clusters overlap, therefore the accuracy is low."

is not necessarily true.

Similarly:

> "The clusters look separated, therefore the classifier has perfect accuracy."

is also incorrect.

A model can achieve high classification performance even when its 2D UMAP projection contains overlap.

The appropriate classification evaluation should include metrics such as:

- **Accuracy**
- **Precision**
- **Recall**
- **F1-score**
- **Confusion matrix**
- **ROC-AUC**, where appropriate

UMAP provides a **complementary representation-level analysis**.

---

# 28. Relationship Between UMAP and Classification Errors

One of the most useful next steps is to combine the UMAP with the model's predictions.

Instead of coloring points only by true class, we can encode both:

```text
Color  → True class
Marker → Correct / Incorrect prediction
```

For example:

```text
🔵 = Glioma
🟠 = Meningioma

✓ = Correct
✗ = Incorrect
```

This would allow us to determine whether misclassified samples concentrate in particular regions of the embedding.

For example, if many incorrectly classified glioma images occur inside a dense meningioma region, this would provide evidence that those errors are associated with a region of representation overlap.

---

# 29. UMAP + Confusion Matrix

The confusion matrix tells us:

> **Which classes are being confused?**

UMAP can then tell us:

> **Where do those confused samples live in feature space?**

The analysis can therefore proceed as:

```text
Confusion Matrix
       ↓
Identify major confusion
       ↓
Example: Glioma ↔ Meningioma
       ↓
Inspect overlapping blue/orange UMAP regions
       ↓
Analyze representative MRI samples
```

This creates a much stronger representation-level error analysis.

---

# 30. UMAP + Grad-CAM / Grad-CAM++

Another powerful analysis is to combine UMAP with explainability.

The workflow could be:

```text
UMAP
  ↓
Identify an interesting cluster
  ↓
Select representative MRI samples
  ↓
Run Grad-CAM / Grad-CAM++
  ↓
Inspect regions highlighted by the model
```

This allows us to connect:

```text
Where the image is represented
```

with:

```text
What visual regions the model uses
```

This is especially valuable for **Explainable and Trustworthy AI**.

---

# 31. Nearest-Neighbor Analysis

Another useful experiment is to find the nearest neighbors of selected MRI images in the original 2048-dimensional embedding space.

For example:

```text
Query MRI
    ↓
2048D embedding
    ↓
Nearest-neighbor search
    ↓
Most similar MRI representations
```

This can answer:

> **Which MRI samples does the model consider most similar to this sample?**

This is particularly useful for ambiguous or misclassified cases.

---

# 32. Cluster-Level Analysis

Because several classes contain multiple subgroups, it may be useful to perform clustering within each class.

For example:

```text
Pituitary
    ↓
Sub-clustering
    ↓
Cluster A
Cluster B
Cluster C
```

Then examine representative images from each subgroup.

The goal would be to determine whether these subclusters correspond to meaningful imaging differences.

This could potentially reveal hidden structure in the dataset.

---

# 33. Technical Consideration: UMAP Parameters

The exact appearance of a UMAP plot depends on parameters such as:

- `n_neighbors`
- `min_dist`
- `metric`
- `random_state`

For example:

```python
umap.UMAP(
    n_neighbors=15,
    min_dist=0.1,
    metric="euclidean",
    random_state=42
)
```

Different parameter values can produce different visual arrangements.

Therefore, when this figure is included in a research repository or paper, the exact UMAP configuration should be documented.

The example values above are illustrative and should **not** be assumed to be the parameters used for this figure.

---

# 34. Important Point About Reproducibility

UMAP can produce different layouts depending on initialization and configuration.

For reproducible analysis, it is useful to specify:

```python
random_state=42
```

or another fixed seed.

The following should ideally be documented:

```text
Embedding dimension
UMAP version
n_neighbors
min_dist
metric
random_state
Number of samples
```

This makes the visualization easier to reproduce.

---

# 35. Important Point About Global Distance

UMAP is particularly useful for understanding local neighborhoods.

Therefore, the following interpretation should be avoided:

> "The green cluster is exactly three times farther from the red cluster than from the blue cluster."

That kind of precise global distance interpretation is not justified.

A better interpretation is:

> "The green samples form local neighborhoods that are largely separated from many tumor samples."

---

# 36. Representation-Level Interpretation

The UMAP provides evidence that ResNet50 has learned a **non-random representation space**.

The network appears to organize the MRI images according to meaningful visual patterns.

Conceptually:

```text
Raw MRI images
      ↓
ResNet50 learns visual representations
      ↓
Similar images → similar feature representations
Different images → different feature representations
      ↓
UMAP visualizes these relationships
```

This makes UMAP particularly useful in representation-learning research.

---

# 37. Overall Interpretation of the Figure

The most important conclusion is:

> **The ResNet50 embedding space contains meaningful diagnostic structure, but the classes are not perfectly disentangled.**

More specifically:

```text
No Tumor
    ↓
Relatively strong separation

Glioma
    ↕
Substantial overlap
    ↕
Meningioma

Pituitary
    ↓
Multiple distinct subregions
```

This indicates that the network has learned useful discriminative information while still retaining substantial intra-class and inter-class complexity.

---

# 38. Final Research-Oriented Conclusion

The UMAP projection demonstrates that the ResNet50 model learns a structured 2048-dimensional representation of brain MRI images.

The **no-tumor class exhibits the clearest separation**, with many samples concentrated in the right and upper regions of the embedding space.

In contrast, **glioma and meningioma show substantial overlap**, suggesting that their learned representations share important visual characteristics and are not completely separated in feature space.

The **pituitary class displays multiple local structures**, indicating heterogeneous representation patterns within the same diagnostic category.

Overall, the embedding can be characterized as:

> **Meaningfully structured, partially class-separable, and heterogeneous, but not perfectly disentangled.**

This is an important finding for representation learning because it demonstrates that the model has learned more than a simple arbitrary feature representation.

At the same time, the overlap between tumor classes shows that strong classification performance should not automatically be interpreted as perfect representation separation.

For a more complete analysis, this UMAP should be considered together with:

- **classification metrics**
- **confusion matrix**
- **correct vs. incorrect prediction analysis**
- **nearest-neighbor analysis**
- **Grad-CAM**
- **Grad-CAM++**
- **class-wise embedding analysis**
- **cluster analysis**

Together, these analyses can provide a much deeper understanding of **what the ResNet50 has learned, where its representations are strong, where they overlap, and where its classification decisions may be uncertain or difficult to interpret**.

---

## 39. One-Sentence Summary

> **The UMAP plot visualizes the 2048-dimensional ResNet50 embeddings of brain MRI images in two dimensions, revealing relatively strong separation of no-tumor samples, substantial overlap between glioma and meningioma, and heterogeneous substructure within classes such as pituitary.**
