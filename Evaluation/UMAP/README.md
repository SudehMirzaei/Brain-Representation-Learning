# UMAP Analysis of ResNet50 Embeddings

## 1. Overview

This figure shows a **2D UMAP projection of the embeddings extracted from ResNet50** for the four brain MRI classes:

- **glioma**
- **meningioma**
- **notumor**
- **pituitary**

The purpose of UMAP in this analysis is not to perform classification itself, but to visualize the structure of the high-dimensional feature space learned by ResNet50. Each point represents one MRI sample, while its position in the two-dimensional plot represents the corresponding high-dimensional embedding after dimensionality reduction.

> **Important:** UMAP Dimension 1 and Dimension 2 do not have a direct biological or semantic interpretation. Their numerical values should not be interpreted as specific MRI features.

---

## 2. Overall Structure of the Embedding Space

The plot indicates that the ResNet50 embeddings contain meaningful information about the different brain MRI categories.

The most noticeable pattern is the relatively strong separation of the **notumor** class from a large portion of the tumor classes. The green samples occupy a broad region on the right side of the projection, while many glioma, meningioma, and pituitary samples are concentrated toward the left and lower parts of the embedding space.

This suggests that ResNet50 has learned visual representations that can distinguish **normal brain MRI scans from many tumor-containing scans**.

However, the tumor classes are not completely separated from each other. In particular, there is substantial overlap between **glioma and meningioma**, especially in the central and upper-left regions.

---

## 3. Analysis of Each Class

### 3.1 Notumor

The **notumor** class, shown in green, demonstrates the clearest large-scale separation in the embedding space.

Most green points are concentrated toward the right side of the plot, although there are several smaller groups and some points closer to the central region.

This pattern suggests that the model has learned a representation that captures characteristics associated with the absence of a visible tumor.

The relatively distinct position of this class is consistent with the idea that normal brain structure provides a different visual representation from tumor-containing MRI scans.

However, the presence of some green points in regions occupied by tumor classes indicates that the representation is not perfectly separated. These samples could represent visually difficult cases, variations in acquisition, or simply local structures that UMAP places close together.

---

### 3.2 Pituitary

The **pituitary** class, shown in red, has several relatively compact clusters in the lower part of the projection.

A particularly visible concentration appears around the lower-left and lower-central regions. There is also a red cluster around the middle-left area.

This relatively compact organization suggests that ResNet50 has learned features that are useful for identifying pituitary tumor cases.

Compared with glioma and meningioma, the pituitary class appears to have several regions that are more distinctly organized.

Nevertheless, some red points are located inside or near regions dominated by other classes. Therefore, the class is not completely isolated.

---

### 3.3 Glioma

The **glioma** class, shown in blue, is considerably more dispersed.

Blue points appear in several different parts of the embedding space, including the central, lower-left, and upper-left regions.

There is substantial overlap between glioma and meningioma samples. This means that, in the learned feature space, some images from these two tumor categories have similar visual representations.

This does not necessarily mean that ResNet50 failed to learn useful features. Rather, it suggests that distinguishing these two tumor types may require more subtle features than those needed to separate tumor from non-tumor cases.

The dispersion of glioma samples may also reflect intra-class variability, such as differences in tumor appearance, location, size, contrast, and MRI acquisition characteristics.

---

### 3.4 Meningioma

The **meningioma** class, shown in orange, is also widely distributed across the embedding space.

There is a noticeable concentration in the upper-left region, but many orange points extend toward the central and lower regions.

The substantial overlap between orange and blue points indicates that **meningioma and glioma are not cleanly separated in the learned representation space**.

This is an important observation because both classes may contain visually similar patterns in MRI images. A classifier can still achieve high classification performance even when a 2D UMAP projection shows overlap, because the original embeddings contain many more dimensions than the two dimensions visualized here.

---

## 4. Class Overlap

One of the most important observations in this figure is the amount of **inter-class overlap**.

### Stronger separation

The clearest separation appears to be between:

- **notumor vs. tumor classes**
- **pituitary vs. some regions of the other tumor classes**

### Stronger overlap

The largest overlap appears to involve:

- **glioma ↔ meningioma**
- **glioma ↔ meningioma ↔ pituitary** in parts of the central region

This suggests that the learned representation contains class-discriminative information, but the boundaries between different tumor categories are more complex than the boundary between normal and abnormal images.

---

## 5. What Does This Say About ResNet50 Representation Learning?

The visualization provides evidence that the ResNet50 embedding space is **not random**.

If the model had learned representations with little relationship to the MRI classes, we would expect the four categories to be extensively mixed throughout the projection.

Instead, we observe structured regions and clusters associated with particular classes.

Therefore, the UMAP visualization supports the following interpretation:

> **ResNet50 has learned a meaningful visual representation of brain MRI images in which samples from different diagnostic categories tend to occupy different regions of the embedding space, although substantial overlap remains between some tumor classes.**

This is especially important for a representation-learning analysis because the goal is not only to report classification accuracy, but also to understand whether the learned feature space contains meaningful organization.

---

## 6. Relationship to Classification Performance

A high classification accuracy does not require perfectly separated clusters in a 2D UMAP plot.

The original ResNet50 embeddings are high-dimensional, whereas UMAP compresses them into only two dimensions. During this compression, some class separation can be lost.

Therefore:

**UMAP overlap ≠ classification failure**

For example, two classes may overlap visually in the UMAP plot while still being separable using a classifier operating on the original high-dimensional embeddings.

This distinction is particularly important when interpreting this figure.

The UMAP should therefore be treated as a **qualitative visualization of representation structure**, rather than as a direct measurement of classification performance.

---

## 7. Important Interpretation of the UMAP Axes

The axes labeled **UMAP Dimension 1** and **UMAP Dimension 2** should not be interpreted as:

- tumor size
- tumor location
- MRI intensity
- anatomical coordinates
- disease severity
- or any specific biological variable

UMAP creates new dimensions that are optimized to preserve neighborhood relationships from the original feature space.

Consequently, the important information in this figure is primarily:

- which samples form neighborhoods,
- which classes form clusters,
- which classes overlap,
- and which classes occupy distinct regions.

---

## 8. Local Structure vs. Global Structure

UMAP is particularly useful for examining **local neighborhood structure**.

For example, a compact group of red points suggests that many pituitary samples have similar representations in the original embedding space.

However, the absolute distance between two clusters in the UMAP plot should not automatically be interpreted as a precise measure of similarity in the original high-dimensional space.

Therefore, conclusions such as:

> "Cluster A is twice as far from Cluster B"

should not be made from this visualization alone.

The strongest conclusions concern the presence of clusters, local neighborhoods, and overlap patterns.

---

## 9. Potential Reasons for Overlap

The overlap between tumor classes may have several possible explanations.

### Biological and anatomical variability

Tumors can vary in:

- size,
- location,
- shape,
- texture,
- intensity,
- and surrounding tissue appearance.

### Imaging variability

MRI images may differ because of:

- acquisition conditions,
- image quality,
- preprocessing,
- contrast,
- scanner characteristics,
- and patient-specific variation.

### Representation limitations

ResNet50 was originally developed for natural image recognition. Although transfer learning can provide strong visual features, its representation may not optimally capture all subtle medical imaging characteristics.

### Dimensionality reduction

Some separation present in the original embedding space may disappear after projecting the embeddings to two dimensions.

---

## 10. Main Findings

The figure can be summarized as follows:

| Observation | Interpretation |
|---|---|
| Notumor forms a relatively distinct region | Strong representation of normal vs. abnormal structure |
| Pituitary forms several compact regions | Useful class-specific representation |
| Glioma is relatively dispersed | High intra-class variability |
| Meningioma is also widely distributed | Significant variation within the class |
| Glioma and meningioma overlap substantially | Their visual representations are relatively similar |
| Some samples of all classes overlap | Embedding space is not perfectly class-separated |
| Clear large-scale structure exists | ResNet50 learned meaningful representations |

---

## 11. Implications for the Project

This UMAP result is useful for the **Representation Learning / Embedding Analysis** stage of the project.

It suggests that the ResNet50 backbone is learning features that organize MRI images according to their diagnostic category.

In particular, the separation of the `notumor` samples suggests that the learned representation captures a meaningful distinction between normal and tumor-containing MRI scans.

At the same time, the overlap between glioma and meningioma indicates that the embedding space contains more challenging decision boundaries.

This observation is valuable because it reveals information that a single accuracy score cannot provide.

---

## 12. Recommended Quantitative Analysis

UMAP should ideally be complemented with quantitative analysis rather than used as the only evidence for representation quality.

Useful next steps include:

### 12.1 Silhouette Score

Calculate the silhouette score using the original embeddings to quantify how well samples are separated according to their class labels.

### 12.2 k-NN Classification on Embeddings

Train a simple k-nearest-neighbor classifier directly on the extracted embeddings.

A strong k-NN performance would provide additional evidence that the embedding space itself contains class-discriminative information.

### 12.3 Linear Probe

Freeze the ResNet50 backbone and train a simple linear classifier on the embeddings.

This tests whether the learned representation is linearly separable.

### 12.4 PCA Comparison

Compare UMAP with PCA to determine whether the observed structure is also visible through a linear dimensionality-reduction method.

### 12.5 Layer-wise Embedding Analysis

Extract embeddings from different ResNet50 stages and compare their UMAP projections.

For example:

- early convolutional layers → low-level visual features
- middle layers → texture and anatomical patterns
- deeper layers → high-level semantic features

This can show how class structure emerges as information passes through the network.

---

## 13. Important Limitation

UMAP is a visualization technique and should not be considered proof that the model has learned medically meaningful features.

The visualization demonstrates **statistical structure in the learned embeddings**, but it does not by itself establish that the model is using clinically relevant anatomical or pathological information.

To investigate this further, UMAP should be combined with explainability methods such as:

- Grad-CAM
- Grad-CAM++
- Integrated Gradients
- Occlusion analysis
- Attention visualization

These methods can help determine whether the features represented in the embedding space correspond to relevant anatomical or pathological regions.

---

## 14. Final Conclusion

The UMAP projection demonstrates that the ResNet50 embedding space contains meaningful structure associated with the four brain MRI categories.

The **notumor class shows the strongest large-scale separation**, indicating that the learned representation captures important differences between normal and tumor-containing MRI scans. The **pituitary class also forms several relatively compact regions**, while **glioma and meningioma exhibit substantial overlap and intra-class variability**.

Overall, the visualization provides qualitative evidence that ResNet50 has learned a useful representation of the MRI data, but the representation is not perfectly class-separated.

The most important conclusion is therefore:

> **ResNet50 learns a structured and class-informative embedding space, with strong separation of normal tissue from many tumor cases but considerable overlap among different tumor types.**

This result motivates further quantitative embedding analysis and explainability experiments to determine whether the learned representations correspond to meaningful medical imaging patterns.

