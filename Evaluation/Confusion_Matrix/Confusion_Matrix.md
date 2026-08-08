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


