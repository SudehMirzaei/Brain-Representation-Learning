Consistency with Metrics:
This observation perfectly explains the lower recall for Glioma observed in the classification report. When embeddings overlap, the classifier is forced to make ambiguous decisions, increasing misclassification rates.

---

## Connecting Embedding Space to Classification Performance

| Embedding Property | Affected Metric | Observed Impact |
|--------------------|-----------------|-----------------|
| Isolated No Tumor cluster | Recall (No Tumor) | ~99.5% |
| Compact Pituitary subclusters | F1 (Pituitary) | High |
| Overlapping Glioma–Meningioma | Recall (Glioma) | Lowered |

The UMAP visualization provides the geometric explanation for the numerical results in the classification report. Performance is high where embeddings are well-separated and degrades precisely where class overlap occurs.

---

## Conclusion

The learned embedding space separates normal brain MRIs almost perfectly from pathological cases. Pituitary tumors also form compact clusters, though with evidence of meaningful intra-class substructure. In contrast, glioma and meningioma exhibit substantial overlap, indicating that the learned representations for these tumor types remain partially entangled. This observation is consistent with the lower recall obtained for glioma during classification.

