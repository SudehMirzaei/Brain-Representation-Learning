# Representation Learning

## Overview

Representation Learning is the central concept behind this project. The objective is not simply to train a neural network that can classify brain MRI images into different tumor categories. Instead, the project investigates how a deep neural network learns to represent brain MRI images internally.

A traditional machine learning system often depends on manually designed features. For example:

```

MRI
│
▼
Hand-crafted features
│
├── Texture
├── Shape
├── Intensity
└── Edge information
│
▼
Classifier
│
▼
Prediction

```

Deep neural networks approach the problem differently. Instead of explicitly defining the features, the network learns useful representations directly from the data.

```

MRI
│
▼
Deep Neural Network
│
▼
Learned Representation
│
▼
Classifier
│
▼
Prediction

```

This process is called **Representation Learning**.

---

## 1. What Is Representation Learning?

Representation Learning is the process through which a machine learning model automatically learns a useful representation of raw input data. For an image, the raw input consists primarily of pixel values. A neural network transforms these raw values through multiple layers into increasingly abstract features. Conceptually:

```

Raw Image
↓
Low-Level Representation
↓
Mid-Level Representation
↓
High-Level Representation
↓
Semantic Representation
↓
Prediction

```

The model therefore does not operate only on the original pixels. It gradually constructs an internal representation that is more useful for the task.

---

## 2. Representation Learning in Brain MRI

In this project, the input is a brain MRI image. The model receives the image as a tensor:

```

224 × 224 × 3

```

and transforms it through the ResNet50 architecture. Conceptually:

```

Brain MRI
│
▼
Convolutional Layers
│
▼
Visual Features
│
▼
Deep Features
│
▼
2048-D Representation
│
▼
Classification

```

The learned representation contains information that the model considers useful for distinguishing:

- Glioma
- Meningioma
- Pituitary
- No Tumor

---

## 3. Why Representation Learning Matters

The final classification output contains very limited information. For example:

```

Prediction: Glioma

```

This tells us the model's final decision, but it does not tell us how the model internally represented the MRI. Representation Learning allows us to investigate the intermediate information used to reach that decision.

The research question therefore changes from:

> "Can the model classify brain tumors?"

to:

> "What visual representation does the model learn from brain MRI images in order to classify them?"

This distinction is fundamental to the purpose of this project.

---

## 4. CNNs as Representation Learning Systems

Convolutional Neural Networks naturally perform hierarchical representation learning. Different stages of a CNN tend to operate at different levels of abstraction. A simplified representation is:

```

Input
│
▼
Early Layers
│
├── Edges
├── Lines
└── Simple textures
│
▼
Middle Layers
│
├── Shapes
├── Patterns
└── Local structures
│
▼
Deep Layers
│
├── Complex structures
├── Anatomical patterns
└── Pathological patterns
│
▼
Final Representation
│
└── Task-relevant features

```

These descriptions are conceptual. The network is not explicitly told:

> "Layer 1 should learn edges."

Instead, these representations emerge from optimization during training.

---

## 5. Hierarchical Feature Learning

One of the important characteristics of deep neural networks is hierarchical feature learning. The network begins with relatively simple transformations and progressively combines them into more complex representations. For example:

```

Pixels
↓
Edges
↓
Textures
↓
Shapes
↓
Structures
↓
Complex visual patterns
↓
Task-relevant representation

```

In brain MRI analysis, these representations may contain information related to visual properties such as:

- boundaries
- intensity patterns
- textures
- anatomical structures
- structural abnormalities
- tumor-related visual patterns

However, these concepts should not automatically be assigned to specific layers without empirical analysis. The purpose of this project is precisely to investigate what representations emerge.

---

## 6. ResNet50 as the Representation Learner

The main baseline architecture in this project is ResNet50. The network consists of multiple residual blocks organized into several stages:

```

Input
│
▼
Initial Convolution
│
▼
Layer 1
│
▼
Layer 2
│
▼
Layer 3
│
▼
Layer 4
│
▼
Global Average Pooling
│
▼
2048-D Representation
│
▼
Classification Head

```

The classification head is placed after the learned representation. Therefore, the backbone can be considered the primary representation learning component.

---

## 7. Representation vs Classification

It is important to distinguish between the learned representation and the final classifier. The architecture can be viewed as:

```

ResNet50
│
┌─────────┴─────────┐
│                   │
Backbone        Classifier
│                   │
▼                   ▼
Representation   Prediction
2048-D           4 Classes

```

The backbone answers:

> How should this MRI be represented?

The classifier answers:

> Which class does this representation correspond to?

This separation is particularly important for representation analysis.

---

## 8. The 2048-Dimensional Representation

After the final convolutional stage and global average pooling, ResNet50 produces a vector with 2048 dimensions. For one MRI:

```

MRI
│
▼
ResNet50 Backbone
│
▼
[ f₁, f₂, f₃, ..., f₂₀₄₈ ]

```

This vector is the learned representation of that image. For a batch of 32 images:

```

[32, 2048]

```

For the complete test set containing 1600 images:

```

[1600, 2048]

```

Each row corresponds to one MRI image. Each row is therefore one point in the learned representation space.

---

## 9. Embeddings as Learned Representations

The 2048-dimensional vectors are commonly called embeddings. The relationship can be represented as:

```

Representation Learning
│
▼
Learned Representation
│
▼
2048-D Feature Vector
│
▼
Embedding

```

Embedding Analysis is therefore one way of studying the representation learned by the network. The project uses these embeddings to investigate the structure of the learned feature space.

---

## 10. Representation Space

Once every MRI has been converted into an embedding, the entire dataset can be viewed as a collection of points in a high-dimensional space.

```

MRI 1 → Embedding 1
MRI 2 → Embedding 2
MRI 3 → Embedding 3
...
MRI N → Embedding N

```

Conceptually:

```

Representation Space
● ● ●   ● ● ●   ●
▲ ▲ ▲   ▲ ▲ ▲   ▲ ▲
■ ■ ■   ■ ■ ■   ■
○ ○ ○   ○ ○ ○   ○ ○ ○

```

The geometry of this space can reveal relationships between samples. Images that are represented similarly may appear close together, while images represented differently may appear farther apart.

---

## 11. Learning Class-Discriminative Representations

Because the model is trained for classification, the optimization process encourages the learned representation to contain information useful for distinguishing the target classes. Ideally:

```

Same class
↓
Similar representation

Different classes
↓
More distinguishable representations

```

For example:

```

Glioma      ● ● ●   ● ● ●   ●
Meningioma  ▲ ▲ ▲   ▲ ▲ ▲   ▲ ▲
Pituitary   ■ ■ ■   ■
No Tumor    ○ ○ ○   ○

```

This would indicate a relatively class-discriminative representation. However, perfect separation is not expected or required. Some overlap may naturally occur between visually similar samples.

---

## 12. Representation Learning and Similarity

Representation learning changes how similarity between images can be defined. In raw image space, two MRI images may have different pixel values because of:

- rotation
- intensity differences
- acquisition differences
- anatomical variation
- image positioning

Yet the model may still learn to represent them similarly. Therefore:

```

Pixel Similarity ≠ Representation Similarity

```

Representation similarity is based on the features learned by the neural network. This is one of the reasons representation analysis is useful.

---

## 13. Global Average Pooling

ResNet50 uses global average pooling before producing the 2048-dimensional representation. The final convolutional feature maps have spatial dimensions. Global average pooling summarizes these feature maps into a fixed-length vector. Conceptually:

```

Feature Maps
│
▼
Global Average Pooling
│
▼
2048 Values

```

This transforms the spatial feature representation into a compact vector suitable for classification and embedding analysis.

---

## 14. Representation Learning During Training

The representation is not manually designed. It is learned through optimization. The training process is approximately:

```

MRI
│
▼
ResNet50
│
▼
Representation
│
▼
Prediction
│
▼
Loss
│
▼
Backpropagation
│
▼
Update Network Parameters
│
└───────────────┐
▼
Improved Representation

```

At each training step, the model adjusts its parameters to reduce the classification loss. As training progresses, the internal representation is expected to become increasingly useful for the classification task.

---

## 15. Transfer Learning and Representation Learning

The ResNet50 backbone used in this project is initialized with ImageNet-pretrained weights. This means the network begins with visual representations learned from a large natural-image dataset. The process is:

```

ImageNet Pretraining
│
▼
General Visual Representation
│
▼
Brain MRI Fine-Tuning
│
▼
Brain MRI Representation

```

This provides the model with an initial set of visual features. During training on brain MRI data, these features can be adapted toward the target domain.

---

## 16. Domain Adaptation of Representations

The representation learned from natural images is not necessarily optimal for brain MRI. The model must adapt its internal features to the new domain. Conceptually:

```

General Visual Features
│
▼
Domain-Specific Learning
│
▼
Brain MRI Features

```

This raises an important research question:

> How does a pretrained visual representation change when adapted to brain MRI classification?

This can later be investigated by comparing representations from different stages of the network.

---

## 17. Layer-Wise Representation Analysis

Representation learning is not limited to the final 2048-dimensional embedding. Different layers can be analyzed:

```

Input
│
▼
Layer 1
│
▼
Layer 2
│
▼
Layer 3
│
▼
Layer 4
│
▼
2048-D Embedding

```

The goal is to investigate how the representation evolves through the network. For example:

### Early representation
May capture relatively low-level visual properties such as:
- edges
- gradients
- local textures
- simple patterns

### Intermediate representation
May capture:
- shapes
- local structures
- more complex patterns

### Deep representation
May become increasingly specialized for the classification task.

These interpretations are hypotheses that should be validated through visualization and analysis.

---

## 18. Representation Evolution

The central idea of layer-wise analysis is to observe how the input changes throughout the network.

```

Raw MRI
↓
Low-Level Features
↓
Mid-Level Features
↓
High-Level Features
↓
Task-Specific Representation

```

This can be thought of as a transformation from:

```

Pixels

```

to:

```

Meaningful task-related representation

```

The exact nature of this transformation is one of the main objects of investigation in this project.

---

## 19. Representation Learning and Explainability

Representation Learning and Explainable AI are closely related but answer different questions.

**Representation Learning** asks:
> What internal features does the model learn?

**Explainability** asks:
> Why did the model make a particular prediction?

Together:

```

Model
│
┌────────┴────────┐
│                 │
▼                 ▼
Representation    Prediction
│                 │
▼                 ▼
Embedding         XAI
│                 │
▼                 ▼
Global structure  Local evidence

```

This combination forms the foundation of the project's explainability analysis.

---

## 20. Representation Analysis and XAI

Embedding analysis provides a global view of the learned representation. For example:

```

Many MRI images
↓
2048-D embeddings
↓
UMAP
↓
Class clusters

```

XAI provides a local view. For example:

```

One MRI
↓
Grad-CAM
↓
Important regions

```

The two perspectives can be combined:

```

Global Representation + Local Explanation
↓
More Complete Interpretation

```

---

## 21. Representation Learning and Model Behavior

A useful representation should ideally support good model behavior. Therefore, representation analysis should be connected to:

- classification accuracy
- confusion matrix
- prediction confidence
- misclassification
- class similarity
- XAI results

For example:

```

Representation Overlap
↓
Ambiguous Sample
↓
Low Confidence
↓
Misclassification

```

This is not guaranteed, but it is an important hypothesis to investigate.

---

## 22. Representation Learning and Model Reliability

High classification accuracy does not automatically imply that the learned representation is trustworthy. A model may potentially exploit:

- dataset-specific artifacts
- acquisition characteristics
- image backgrounds
- spurious correlations
- non-pathological visual cues

Therefore, representation analysis should be considered part of a broader reliability investigation. The important question becomes:

> What information is actually encoded in the representation used by the model?

This is especially important in medical imaging.

---

## 23. Representation Learning in This Project

The complete representation-learning pipeline is:

```

Brain MRI Dataset
│
▼
Preprocessing
│
▼
ResNet50
│
▼
Hierarchical Feature Learning
│
▼
Deep Representation
│
▼
2048-D Embedding
│
├───────────────┐
│               │
▼               ▼
Classification  Representation Analysis
│               │
▼               ▼
Prediction      PCA / t-SNE / UMAP
│
▼
Feature Space
│
▼
Interpret Structure

```

---

## 24. Main Research Questions

Representation Learning in this project is designed around several questions.

### RQ1 — What does the network learn?
What visual representations emerge inside ResNet50 when it is trained on brain MRI images?

### RQ2 — How does representation evolve?
How does the representation change from early convolutional layers to deep layers?

### RQ3 — Is the representation class-discriminative?
Do different tumor classes become distinguishable in the learned representation space?

### RQ4 — Which classes are similar?
Which tumor categories have the greatest overlap in the learned representation?

### RQ5 — Where do errors occur?
Are classification errors associated with ambiguous regions of the representation space?

### RQ6 — What information supports predictions?
Can the learned representation be connected to spatial evidence identified through explainability methods?

---

## 25. Representation Learning vs Feature Engineering

**Traditional feature engineering:**

```

MRI
│
▼
Human-designed features
│
▼
Classifier

```

**Deep representation learning:**

```

MRI
│
▼
Neural Network
│
▼
Learned Features
│
▼
Classifier

```

The major advantage is that the model learns the representation jointly with the target task. The major challenge is that these learned representations are often difficult to interpret. This project addresses that challenge by combining representation analysis with explainability.

---

## 26. Limitations

Representation learning should not be interpreted as equivalent to human understanding. A learned feature is not necessarily:

```

Human concept

```

The network may encode information in a distributed and highly nonlinear manner. Additionally, the learned representation can be affected by:

- dataset size
- dataset composition
- preprocessing
- augmentation
- class imbalance
- model architecture
- pretrained initialization
- training strategy
- image acquisition conditions

Therefore, representation analysis should be treated as an empirical investigation rather than a direct translation of neural features into human concepts.

---

## 27. Overall Concept

The central idea of this project can be summarized as:

```

Brain MRI
│
▼
Deep Neural Network
│
▼
Learned Representation
│
▼
2048-D Embedding
│
┌──────────┴──────────┐
│                     │
▼                     ▼
Representation        Classification
Analysis
│                     │
▼                     ▼
PCA / UMAP            Prediction
t-SNE
│
▼
XAI
│
▼
Feature Space

```

The model is therefore studied at multiple levels:

1. **Input level** — the original MRI image.
2. **Feature level** — intermediate neural representations.
3. **Embedding level** — the final 2048-dimensional representation.
4. **Prediction level** — the classification decision.
5. **Explanation level** — the image regions contributing to the decision.

---

## 28. Final Perspective

The purpose of Representation Learning in this project is to move beyond treating a neural network as a simple mapping:

```

MRI → Tumor Class

```

Instead, the project investigates the complete transformation:

```

MRI
↓
Visual Features
↓
Hierarchical Representations
↓
Deep Representation
↓
2048-D Embedding
↓
Classification

```

The central research objective is therefore:

> To understand what representations a deep neural network learns from brain MRI images and how those representations support its classification decisions.

This perspective connects deep learning, representation learning, embedding analysis, and explainable AI. Ultimately, the project is not only concerned with whether the model is accurate. It is concerned with understanding:

- What the model learns
- How it represents the input
- How those representations are organized
- How they contribute to the final decision

