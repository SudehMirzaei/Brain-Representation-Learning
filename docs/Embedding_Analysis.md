# Embedding Analysis

## Overview

Embedding Analysis is one of the central components of this project. The goal of this analysis is to investigate how the ResNet50 model represents brain MRI images internally, rather than evaluating the model only through its final classification predictions.

A classification model produces a final output such as:
```

MRI → Glioma

```
However, this final prediction does not reveal how the model arrived at that decision. Internally, the image passes through multiple neural network layers and is gradually transformed from raw pixels into increasingly abstract representations.

In this project, the final representation produced by the ResNet50 backbone is a 2048-dimensional feature vector, commonly referred to as an **embedding**. Embedding Analysis studies these learned representations and asks:

> How does the model organize different brain MRI images in its learned representation space?

---

## 1. What Is an Embedding?

An embedding is a numerical representation of an input produced by a neural network. For a brain MRI image, the process can be represented as:

```

Brain MRI
│
▼
ResNet50
│
▼
Deep Feature Representation
│
▼
2048-dimensional Embedding
│
▼
Classification Head
│
▼
Predicted Class

```

Instead of representing an MRI only as its original pixels, the network converts it into a vector containing 2048 learned features. Conceptually:

```

MRI Image
↓
[ f₁, f₂, f₃, ..., f₂₀₄₈ ]

```

Each MRI therefore becomes a point in a 2048-dimensional feature space.

---

## 2. Embedding in This Project

The ResNet50 architecture used in this project consists of two main conceptual components:

```

ResNet50
│
┌─────────┴─────────┐
│                   │
Backbone        Classifier
│                   │
▼                   ▼
2048-D Vector    4 Classes

```

The backbone is responsible for learning visual representations. The classifier uses those representations to predict one of the four brain MRI classes:

- Glioma
- Meningioma
- Pituitary
- No Tumor

The embedding is extracted immediately before the classification head. Therefore:

```

MRI
↓
ResNet50 Backbone
↓
2048-D Embedding
↓
Classifier
↓
Prediction

```

The embedding is consequently a representation of what the network has learned about the input image before making its final decision.

---

## 3. Why Analyze Embeddings?

Classification metrics such as:

- Accuracy
- Precision
- Recall
- F1-score

tell us *how well* the model performs. However, they do not directly tell us *how* the model internally organizes the data. For example, two models may achieve similar accuracy while learning substantially different representations.

Embedding Analysis provides another perspective. Instead of asking only:

> Did the model classify the MRI correctly?

we can ask:

> How are MRI images organized in the representation space learned by the model?

This allows us to investigate whether the learned representation contains meaningful structure related to the different tumor classes.

---

## 4. Representation Space

Every MRI image is transformed into a point in a 2048-dimensional space. For example:

```

MRI A → [0.21, -0.14, 0.83, ..., 0.41]
MRI B → [0.19, -0.11, 0.79, ..., 0.39]
MRI C → [-0.72, 0.42, 0.15, ..., -0.21]

```

If two MRI images produce similar embeddings, this suggests that the model represents them similarly. If their embeddings are very different, the model considers them different in its learned representation space. Therefore, the geometry of the embedding space becomes informative. Conceptually:

```

Image Space
│
▼
Neural Network
│
▼
Representation Space
│
├── Similar MRI representations
├── Different MRI representations
└── Class-specific structure

```

---

## 5. What Does the 2048-Dimensional Vector Mean?

The 2048 dimensions should not be interpreted as 2048 independent human-readable concepts. For example, we cannot automatically assume:

```

Feature 1 = tumor size
Feature 2 = tumor location
Feature 3 = brain texture

```

The representation learned by a deep neural network is generally distributed. Information about:

- shape
- texture
- edges
- anatomical structures
- pathological patterns
- spatial relationships
- visual similarities

may be distributed across many dimensions. Therefore, this project does not attempt to assign a specific semantic meaning to individual embedding dimensions. Instead, the analysis focuses on the overall structure of the learned representation space.

---

## 6. From High-Dimensional Representation to Visualization

A 2048-dimensional representation cannot be directly visualized. Therefore, dimensionality-reduction methods are used to project the embeddings into a lower-dimensional space. The general process is:

```

2048-D Embeddings
│
├── PCA
├── t-SNE
└── UMAP
│
▼
2-D Space
│
▼
Visualization

```

The resulting 2D visualization allows us to inspect relationships between MRI samples. Each point represents one MRI image.

---

## 7. What Are We Looking For?

The main objective is to determine whether the learned representation contains class-related structure. For example, ideally we might observe:

```

Glioma      ● ● ●   ● ● ●   ● ● ●   ●
Meningioma  ▲ ▲ ▲   ▲ ▲ ▲   ▲ ▲ ▲   ▲
Pituitary   ■ ■ ■   ■ ■ ■
No Tumor    ○ ○ ○   ○ ○ ○   ○ ○ ○   ○

```

This type of structure would suggest that images from the same class tend to have similar learned representations. However, real data may produce overlapping regions:

```

● ● ● ▲ ▲ ●
● ● ▲ ▲ ▲ ●
● ■ ■ ▲ ■ ■
■

```

Such overlap can indicate that the model represents some classes as visually or semantically similar.

---

## 8. Class Separation

One of the important questions in Embedding Analysis is:

> Are different tumor classes separated in the learned representation space?

For example:

```

Glioma ↔ Meningioma
Glioma ↔ Pituitary
Glioma ↔ No Tumor
Meningioma ↔ Pituitary
Meningioma ↔ No Tumor
Pituitary ↔ No Tumor

```

If samples from two classes strongly overlap, this may indicate that the model has difficulty finding a representation that clearly distinguishes those classes. Conversely, strong separation suggests that the learned features contain discriminative information useful for classification.

---

## 9. Intra-Class Similarity

Embedding Analysis can also examine intra-class similarity. Intra-class similarity asks:

> How similar are the representations of MRI images belonging to the same class?

For example:

```

Glioma
│
├── MRI 1
├── MRI 2
├── MRI 3
└── MRI 4

```

If their embeddings are relatively close, the representation is coherent for that class. A highly coherent class might appear as a compact region in the embedding space.

---

## 10. Inter-Class Similarity

Inter-class similarity asks:

> How similar are the representations of MRI images belonging to different classes?

For example:

```

Glioma ↔ Meningioma
Glioma ↔ Pituitary
Glioma ↔ No Tumor

```

A useful discriminative representation should generally make samples from different classes more distinguishable. Conceptually:

```

High intra-class similarity + Low inter-class similarity
↓
Better class-discriminative representation

```

This does not mean that all classes must be perfectly separated. Some biological and visual similarities between tumor types are expected.

---

## 11. Relationship Between Embeddings and Classification

The embedding is directly connected to the classification decision. The process is:

```

MRI
↓
2048-D Representation
↓
Classification Head
↓
Prediction

```

The classifier does not operate directly on the raw MRI. It operates on the learned representation. Therefore, classification performance depends heavily on the quality of this representation. If the representation contains useful class-discriminative information, the classifier can use it to distinguish the four classes.

---

## 12. Embeddings and Misclassification

Embedding Analysis becomes especially interesting when combined with model errors. Suppose the model frequently confuses:

```

Glioma → Meningioma

```

We can investigate whether the corresponding embeddings also overlap. Conceptually:

```

Embedding Space

Glioma      Meningioma
● ● ●       ▲ ▲ ▲
● ● ●       ▲ ▲ ▲
● ● ▲ ▲     ● ▲

```

If misclassified glioma images appear close to the meningioma region, the representation analysis provides a possible explanation for the classification error. This creates a connection between:

```

Representation
↓
Class Overlap
↓
Prediction Difficulty
↓
Classification Error

```

---

## 13. Correct and Incorrect Samples

Another useful analysis is to distinguish correctly classified and incorrectly classified samples within the embedding space. Conceptually:

```

Embedding Space
│
├── Correct predictions
└── Incorrect predictions

```

If incorrect predictions concentrate near the boundaries between clusters, this may indicate that those samples have ambiguous representations. For example:

```

Class A          Class B
● ● ● ●         ▲ ▲ ▲
● ● ● ●         ▲ ▲ ▲
● ● ● × × ×     ▲ ▲ ▲
↑ ambiguous region

```

This analysis can help identify where the representation is less discriminative.

---

## 14. Embedding Analysis and UMAP

UMAP is particularly useful for visualizing the structure of the learned representation. The workflow is:

```

MRI Dataset
↓
Trained ResNet50
↓
2048-D Embeddings
↓
UMAP
↓
2-D Representation
↓
Visualization

```

The resulting plot allows us to inspect:

- class clusters
- cluster overlap
- local neighborhoods
- ambiguous samples
- potential outliers
- representation boundaries

However, UMAP is only a projection of the original feature space. Therefore, the 2D visualization should not be treated as a perfect representation of the original 2048-dimensional geometry.

---

## 15. PCA, t-SNE, and UMAP

Different dimensionality-reduction methods provide different perspectives.

### PCA
PCA is a linear method. It identifies directions of maximum variance in the embedding space.

```

2048-D
↓
Linear projection
↓
2-D

```

PCA provides a relatively simple global overview.

### t-SNE
t-SNE is a nonlinear method that focuses strongly on preserving local neighborhood relationships. It is useful for investigating local clusters.

### UMAP
UMAP is also nonlinear and can provide useful information about both local neighborhoods and broader structure.

For this reason, this project can use all three methods as complementary views rather than relying on a single visualization.

---

## 16. Embedding Analysis Is Not the Same as XAI

Embedding Analysis and Explainability answer different questions.

### Embedding Analysis
Asks:

> How does the model represent the input internally?

It focuses on the structure of the learned feature space.

```

MRI
↓
Embedding
↓
Representation Space

```

### XAI
Asks:

> Which parts of the input contributed to the model's decision?

For example, Grad-CAM produces a spatial importance map.

```

MRI
↓
Grad-CAM
↓
Important Image Regions

```

Therefore:

```

Embedding Analysis + XAI
↓
More complete model interpretation

```

---

## 17. Global vs Local Interpretation

Embedding Analysis provides a relatively global perspective. It examines many samples simultaneously.

```

100s or 1000s of MRI images
↓
Representation Space
↓
Global Structure

```

XAI methods such as Grad-CAM provide a local perspective. They examine why the model made a particular prediction for an individual image.

```

One MRI
↓
Model
↓
Prediction
↓
Important regions

```

Combining these perspectives is an important part of this project.

---

## 18. Embedding Analysis as Representation Learning Analysis

The broader research goal of this project is **Explainable Brain Representation Learning**. Therefore, embeddings are not simply an additional visualization. They are evidence of what the network has learned internally. The analysis can be viewed as:

```

Input MRI
↓
Learned Representation
↓
Embedding
↓
Representation Geometry
↓
Class Structure
↓
Classification Behavior

```

This allows the project to move beyond the question:

> "How accurate is the model?"

toward:

> "What kind of representation does the model learn from brain MRI images?"

---

## 19. Important Limitations

Embedding Analysis must be interpreted carefully. A visually clean cluster does not automatically prove that the model has learned medically meaningful or anatomically correct features. The representation can also be influenced by:

- dataset bias
- image acquisition differences
- preprocessing
- class imbalance
- artifacts
- scanner-specific characteristics
- background information
- model architecture
- training procedure

Similarly, a UMAP or t-SNE visualization is not sufficient evidence that the model has learned clinically meaningful representations. Therefore, Embedding Analysis should be combined with other analyses.

---

## 20. Connection to Explainable Brain Representation Learning

The ultimate purpose of this analysis is to connect three levels of understanding:

```

Brain MRI
│
▼
Neural Representation
│
┌──────────┴──────────┐
│                     │
▼                     ▼
Embedding Space       XAI Maps
│                     │
▼                     ▼
Global Structure      Local Evidence
│                     │
└──────────┬──────────┘
▼
Model Interpretation

```

Embedding Analysis tells us about the organization of learned representations. XAI tells us about the spatial evidence used for individual predictions. Together, they provide a more complete understanding of the model.

---

## 21. Research Questions

The Embedding Analysis component of this project is designed to investigate several research questions:

### RQ1 — Representation Structure
Does ResNet50 learn a structured representation of brain MRI images?

### RQ2 — Class Separation
Do different tumor classes occupy distinguishable regions of the learned representation space?

### RQ3 — Class Similarity
Which tumor classes have the greatest overlap in the learned representation?

### RQ4 — Classification Errors
Are misclassified MRI images located near regions of representation overlap?

### RQ5 — Representation Quality
Does the structure of the embedding space correspond to the observed classification performance?

### RQ6 — Explainability
Can the learned representation structure be connected to spatial evidence identified by XAI methods?

---

## 22. Overall Analysis Pipeline

The complete Embedding Analysis pipeline is:

```

Brain MRI Dataset
│
▼
ResNet50
│
▼
Train Classification Model
│
▼
Extract 2048-D Embeddings
│
▼
Representation Space
│
├───────────┐
│           │
▼           ▼
PCA     t-SNE / UMAP
│           │
└─────┬─────┘
▼
2-D Visualization
│
▼
Analyze Representation
│
┌──────┼──────┐
▼      ▼      ▼
Clusters  Overlap  Errors
│      │      │
└──────┼──────┘
▼
Compare with Classification
│
▼
Connect to XAI

```

---

## 23. Final Perspective

Embedding Analysis provides a way to look inside the learned representation of the ResNet50 model. Instead of treating the model as a black box that simply maps:

```

MRI → Class

```

we investigate the intermediate process:

```

MRI
↓
Visual Features
↓
Deep Representation
↓
2048-D Embedding
↓
Representation Space
↓
Classification

```

The central idea is therefore:

> A model should not only be evaluated by what it predicts, but also by understanding how it represents the data that it predicts from.

For this project, Embedding Analysis is consequently an important bridge between deep learning, representation learning, and explainable AI. It provides the foundation for later analyses involving:

- PCA
- t-SNE
- UMAP
- embedding similarity
- class separation
- correct/incorrect representation analysis
- confidence analysis
- Feature Map visualization
- Grad-CAM
- Grad-CAM++
- Integrated Gradients

Together, these analyses form a broader framework for studying how deep neural networks learn and represent information from brain MRI images.

