# Confusion Matrix Analysis

## ResNet50 Baseline Classification

---

## 1. Overview

The confusion matrix is one of the most important evaluation tools used in the baseline classification stage of the **Brain Representation Learning** project.

While metrics such as accuracy, precision, recall, and F1-score provide an overall numerical summary of model performance, the confusion matrix provides a **class-level analysis** of model behavior.

In this project, the ResNet50 baseline model performs four-class classification of brain MRI images:

- Glioma
- Meningioma
- No Tumor
- Pituitary

The test set contains 400 images from each class:

```text
Glioma       : 400
Meningioma   : 400
No Tumor     : 400
Pituitary    : 400
--------------------
Total        : 1600
```

The confusion matrix obtained from the ResNet50 baseline is:

| Actual \ Predicted | Glioma | Meningioma | No Tumor | Pituitary |
|---|---:|---:|---:|---:|
| **Glioma** | 322 | 48 | 30 | 0 |
| **Meningioma** | 0 | 394 | 2 | 4 |
| **No Tumor** | 0 | 0 | 400 | 0 |
| **Pituitary** | 0 | 0 | 0 | 400 |

Rows represent the **actual class**, while columns represent the **predicted class**.

The diagonal elements represent correct classifications.

The off-diagonal elements represent classification errors.

---

# 2. Reading the Confusion Matrix

The matrix can be interpreted as follows:

```text
                         Predicted Class
                 ┌──────────────────────────────┐
                 │ Glioma │ Mening. │ NoTumor │ Pituitary
─────────────────┼──────────────────────────────┤
Actual Glioma    │  322   │   48    │   30    │    0
Actual Mening.   │   0    │  394    │    2    │    4
Actual NoTumor   │   0    │    0    │  400    │    0
Actual Pituitary │   0    │    0    │    0    │  400
                 └──────────────────────────────┘
```

For example:

```text
322
```

means that 322 actual Glioma images were correctly predicted as Glioma.

Similarly:

```text
48
```

means that 48 actual Glioma images were incorrectly predicted as Meningioma.

---

# 3. Overall Performance

The number of correctly classified samples is obtained from the diagonal:

```text
322 + 394 + 400 + 400 = 1516
```

The total number of test samples is:

```text
1600
```

Therefore, the accuracy represented by this particular confusion matrix is:

```text
Accuracy = 1516 / 1600
         = 0.9475
         = 94.75%
```

Thus, this confusion matrix corresponds to an overall accuracy of approximately:

**94.75%**

### Important Reproducibility Note

This accuracy should only be reported together with metrics generated from the **same evaluation run**.

If another evaluation result in the project reports a different accuracy, such as:

```text
87.38%
```

the two results should not be combined.

Different results may originate from:

- Different model checkpoints
- Different preprocessing
- Different dataset splits
- Different test sets
- Different prediction runs
- Different versions of the trained model

For a reproducible research project, the confusion matrix, classification report, predictions, and checkpoint should always correspond to the same evaluation configuration.

---

# 4. Class-wise Analysis

The diagonal values are:

| Class | Correct | Total | Recall |
|---|---:|---:|---:|
| Glioma | 322 | 400 | 80.5% |
| Meningioma | 394 | 400 | 98.5% |
| No Tumor | 400 | 400 | 100% |
| Pituitary | 400 | 400 | 100% |

This immediately shows that the model does not perform equally well across all classes.

The most difficult class is:

```text
Glioma
```

while the strongest classes are:

```text
No Tumor
Pituitary
```

---

# 5. Glioma Analysis

The Glioma row is:

| Predicted Class | Number |
|---|---:|
| Glioma | 322 |
| Meningioma | 48 |
| No Tumor | 30 |
| Pituitary | 0 |

There are 400 actual Glioma images.

The model correctly classifies:

```text
322 / 400 = 80.5%
```

Therefore, Glioma recall is:

```text
80.5%
```

The number of incorrectly classified Glioma images is:

```text
400 - 322 = 78
```

Thus, 78 Glioma images were misclassified.

This makes Glioma the primary source of classification errors in this confusion matrix.

---

## 5.1 Glioma → Meningioma

The largest individual confusion is:

```text
48 Glioma → Meningioma
```

This represents the dominant error pattern in the matrix.

The model appears to have difficulty separating some Glioma representations from Meningioma representations.

From the perspective of this project, this is particularly important because the main goal is not simply classification accuracy.

The project aims to understand:

> How does a deep neural network construct visual representations of brain MRI images?

The Glioma–Meningioma confusion therefore provides an important target for representation analysis.

Potential explanations include:

- Similar visual patterns
- Similar tumor morphology
- Overlapping high-level feature representations
- Insufficiently discriminative channels
- Ambiguous MRI characteristics
- Dataset-specific similarities

These possibilities can be investigated using the representation and embedding analyses performed later in the project.

---

## 5.2 Glioma → No Tumor

The second major confusion is:

```text
30 Glioma → No Tumor
```

This means that 30 actual Glioma images were classified as No Tumor.

This is particularly important from a medical classification perspective because the model is effectively interpreting a tumor image as a normal image.

However, the confusion matrix alone cannot determine the reason for this behavior.

Possible explanations include:

- Small or subtle tumor regions
- Weak tumor-related features
- Ambiguous visual appearance
- Insufficient spatial information in deep layers
- Attention to irrelevant image regions
- Dataset-specific artifacts
- Similarity between some Glioma images and No Tumor images

These samples should therefore be examined using explainability methods.

---

## 5.3 Glioma → Pituitary

There are:

```text
0 Glioma → Pituitary
```

errors.

This suggests that the learned representation provides a strong separation between Glioma and Pituitary images.

This is an example of a class boundary that appears to be relatively easy for the model.

---

# 6. Meningioma Analysis

The Meningioma row is:

| Predicted Class | Number |
|---|---:|
| Glioma | 0 |
| Meningioma | 394 |
| No Tumor | 2 |
| Pituitary | 4 |

There are 400 actual Meningioma images.

The model correctly classifies:

```text
394 / 400 = 98.5%
```

Therefore:

```text
Meningioma Recall = 98.5%
```

Only six Meningioma images are misclassified:

```text
2 → No Tumor
4 → Pituitary
```

There are no:

```text
Meningioma → Glioma
```

errors in this matrix.

This suggests that Meningioma is represented relatively well by the learned feature space.

However, the six incorrectly classified images remain valuable for further analysis because they may represent samples near decision boundaries.

---

# 7. No Tumor Analysis

The No Tumor row is:

| Predicted Class | Number |
|---|---:|
| Glioma | 0 |
| Meningioma | 0 |
| No Tumor | 400 |
| Pituitary | 0 |

All 400 No Tumor images are correctly classified.

Therefore:

```text
No Tumor Recall = 400 / 400 = 100%
```

There are no observed false negatives for this class.

This indicates a very strong separation between the No Tumor samples and the other classes in this particular test set.

However, perfect classification should be interpreted carefully.

A neural network may potentially exploit dataset-specific characteristics rather than purely learning clinically meaningful anatomy.

Possible factors include:

- Intensity distribution
- Image cropping
- Background structure
- Image resolution
- Acquisition characteristics
- Preprocessing artifacts
- Dataset-specific visual patterns

Therefore, the later representation and explainability analyses are necessary to investigate what the model actually uses.

---

# 8. Pituitary Analysis

The Pituitary row is:

| Predicted Class | Number |
|---|---:|
| Glioma | 0 |
| Meningioma | 0 |
| No Tumor | 0 |
| Pituitary | 400 |

All 400 Pituitary images are correctly classified.

Therefore:

```text
Pituitary Recall = 400 / 400 = 100%
```

There are no observed misclassifications for Pituitary in this matrix.

This suggests that the model has learned highly discriminative features for this class.

A key research question is:

> What features allow the network to separate Pituitary images so effectively?

This question can be investigated through:

- Feature Map Visualization
- Mean Activation Maps
- Top Activated Channels
- Layer-wise Representation Analysis
- Grad-CAM
- Grad-CAM++
- Embedding Analysis

---

# 9. False Negative Analysis

For each class, false negatives correspond to samples belonging to that class but predicted as another class.

## Glioma

```text
400 - 322 = 78
```

False negatives:

```text
78
```

## Meningioma

```text
400 - 394 = 6
```

False negatives:

```text
6
```

## No Tumor

```text
400 - 400 = 0
```

False negatives:

```text
0
```

## Pituitary

```text
400 - 400 = 0
```

False negatives:

```text
0
```

Therefore, Glioma has by far the largest number of false negatives.

---

# 10. False Positive Analysis

False positives can be analyzed column-wise.

---

## 10.1 Predicted Glioma

Total predictions of Glioma:

```text
322 + 0 + 0 + 0 = 322
```

Actual Glioma:

```text
322
```

Therefore:

```text
False Positives = 0
```

---

## 10.2 Predicted Meningioma

Total predictions of Meningioma:

```text
48 + 394 + 0 + 0 = 442
```

Correct Meningioma predictions:

```text
394
```

Therefore:

```text
False Positives = 442 - 394
                = 48
```

All 48 false positives are caused by:

```text
Glioma → Meningioma
```

---

## 10.3 Predicted No Tumor

Total predictions of No Tumor:

```text
30 + 2 + 400 + 0 = 432
```

Correct No Tumor predictions:

```text
400
```

Therefore:

```text
False Positives = 32
```

These consist of:

```text
30 Glioma → No Tumor
2 Meningioma → No Tumor
```

---

## 10.4 Predicted Pituitary

Total predictions of Pituitary:

```text
0 + 4 + 0 + 400 = 404
```

Correct predictions:

```text
400
```

Therefore:

```text
False Positives = 4
```

All four false positives are:

```text
Meningioma → Pituitary
```

---

# 11. Main Confusion Patterns

The major errors can be summarized as:

```text
Glioma
   │
   ├── 48 → Meningioma
   │
   └── 30 → No Tumor


Meningioma
   │
   ├── 2 → No Tumor
   │
   └── 4 → Pituitary
```

The dominant confusion is:

```text
Glioma → Meningioma
```

followed by:

```text
Glioma → No Tumor
```

These two error patterns should receive the greatest attention in the subsequent analysis.

---

# 12. Representation Learning Interpretation

The confusion matrix becomes more informative when interpreted together with the internal representation analysis.

The ResNet50 model progressively transforms the input MRI:

```text
MRI
 ↓
Conv1
 ↓
Layer1
 ↓
Layer2
 ↓
Layer3
 ↓
Layer4
 ↓
Global Average Pooling
 ↓
2048-dimensional Embedding
 ↓
Classifier
```

At each stage, the representation becomes increasingly abstract.

The earlier layers tend to encode lower-level visual information, while deeper layers produce more semantic and task-relevant representations.

The confusion matrix indicates that these representations are not equally separable for all classes.

In particular:

```text
Glioma
   ↕
Meningioma
```

appears to be the most difficult class boundary.

This leads to an important hypothesis:

> Some Glioma and Meningioma images may occupy overlapping regions of the learned representation space.

This hypothesis can be directly tested using the embedding analysis.

---

# 13. Connection to Embedding Analysis

The project extracts a 2048-dimensional representation from the ResNet50 backbone:

```text
Embedding Shape:

(1600, 2048)
```

Each MRI image is therefore represented as a point in a 2048-dimensional feature space.

The confusion matrix tells us **where the classifier fails**.

Embedding analysis helps us investigate **why those failures may occur**.

For example, if Glioma and Meningioma samples overlap strongly in UMAP, this would provide evidence that their learned representations are not completely separable.

Conceptually:

```text
Raw MRI
   ↓
ResNet50
   ↓
2048-D Representation
   ↓
UMAP
   ↓
Class Structure
```

If misclassified Glioma samples appear close to the Meningioma cluster, the UMAP analysis would support the confusion-matrix observation.

---

# 14. Connection to UMAP

The UMAP visualization should be used to investigate three major questions.

### Question 1

Do the four classes form distinct clusters?

### Question 2

Where are the misclassified samples located?

### Question 3

Are the errors concentrated near class boundaries?

A useful analysis is to mark:

```text
Correct predictions
```

and

```text
Incorrect predictions
```

on the same UMAP plot.

If incorrect samples are concentrated around cluster boundaries, this may indicate that the corresponding representations are ambiguous.

If incorrect samples form isolated regions, this may indicate that the model has learned an unexpected representation.

---

# 15. Connection to Feature Maps

The project also analyzes internal feature maps from:

```text
Conv1
Layer1
Layer2
Layer3
Layer4
```

The confusion matrix can guide this analysis.

For example, because Glioma is the most difficult class, we can compare its feature maps against Meningioma feature maps.

A possible analysis pipeline is:

```text
Confusion Matrix
       ↓
Identify Glioma errors
       ↓
Compare Glioma / Meningioma
       ↓
Feature Maps
       ↓
Activation Statistics
       ↓
Top Activated Channels
       ↓
Channel Similarity
       ↓
Layer-wise Representation
```

This allows the confusion matrix to act as a starting point for investigating the internal behavior of the network.

---

# 16. Connection to Explainability

Classification errors should also be analyzed using explainability methods.

For misclassified samples, we can compare:

```text
True Class
      ↓
Predicted Class
      ↓
Grad-CAM
      ↓
Grad-CAM++
```

For example:

```text
True: Glioma
Predicted: Meningioma
```

can be analyzed using Grad-CAM to determine whether the model focused on:

- The tumor region
- Brain anatomy
- Background
- Image borders
- Other irrelevant structures

This is important because a correct prediction does not necessarily imply that the model used the correct evidence.

Likewise, an incorrect prediction may reveal which visual features caused the representation to move toward another class.

---

# 17. Misclassified Samples

The confusion matrix indicates:

```text
Total samples = 1600
Correct       = 1516
Incorrect     = 84
```

Therefore:

```text
1516 + 84 = 1600
```

The 84 misclassified images are particularly valuable for research analysis.

Instead of treating them simply as failures, they should be considered **informative samples for understanding the learned representation**.

A complete misclassification analysis should record:

| Information | Description |
|---|---|
| Image | Original MRI |
| True Label | Ground-truth class |
| Predicted Label | Model prediction |
| Confidence | Prediction probability |
| Embedding | 2048-dimensional representation |
| UMAP Position | 2D embedding location |
| Feature Maps | Internal activations |
| Grad-CAM | Spatial explanation |
| Grad-CAM++ | More detailed attribution |

---

