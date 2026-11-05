# Multi-View Multi-Granularity Multi-Label Benchmarks

This collection extends five public multi-view benchmarks with coarse-grained label annotations. The underlying image datasets are Corel5K [1], Pascal07 [2], ESPGame [3], IAPRTC12 [4], and MIRFlickr [5]. For each benchmark, we use the six precomputed feature views as provided and retain the original fine-grained annotations. The same six-view feature setup is documented by Yan et al. [6].

## Multi-Granularity Labels Construction

1. **Compute label affinities.** At the current granularity, estimate label occurrence and co-occurrence probabilities from the binary annotations. Combine Jaccard similarity, nonnegative normalized pointwise mutual information, and the mean of the two directional conditional probabilities to compute pairwise label affinities.
2. **Group labels.** Set the number of groups based on the size of the current label set. Seed each group with a different label, selecting the most frequent labels as seeds. Assign the remaining labels in decreasing order of frequency. For each candidate group, score the assignment by subtracting a penalty for group size and sample coverage from the average affinity between the label and the current group members. The penalty is evaluated after adding the candidate label. Sample coverage is the fraction of samples with at least one label in the resulting group.
3. **Generate coarser labels.** Each group defines one coarse-grained label. A sample receives that label if any label in the group is present. To generate another granularity, recompute the affinities from the newly generated annotations and repeat the grouping procedure.

## Dataset Format

Each dataset is provided as a MATLAB `.mat` file containing the feature views and binary label matrices. `X` is a `1 × 6` cell array of feature matrices, each with one row per sample. The view dimensions, in the order stored in `X`, are **100, 1,000, 512, 4,096, 4,096, and 4,096** for all five datasets.

The label matrices are stored as `uint8` arrays with values of `0` or `1`. Rows follow the same sample order as in `X`, and columns correspond to labels. A value of `1` indicates that the label is present. `Y_L4` stores the original fine-grained labels, and `Y_L3` and `Y_L2` store progressively coarser labels. A dash in the table indicates that the corresponding matrix is absent.

| Dataset | Samples | Views | `Y_L2` labels | `Y_L3` labels | `Y_L4` labels |
| --- | ---: | ---: | ---: | ---: | ---: |
| Corel5K | 4,999 | 6 | 12 | 56 | 260 |
| Pascal07 | 9,963 | 6 | — | 8 | 20 |
| ESPGame | 20,770 | 6 | 12 | 57 | 268 |
| IAPRTC12 | 19,627 | 6 | 12 | 60 | 291 |
| MIRFlickr | 25,000 | 6 | — | 11 | 38 |

## References

[1] P. Duygulu, K. Barnard, J. F. G. de Freitas, and D. A. Forsyth. “Object Recognition as Machine Translation: Learning a Lexicon for a Fixed Image Vocabulary.” *ECCV*, 2002. [Paper](https://ora.ox.ac.uk/objects/uuid%3A92e538e8-10b7-461a-9f8e-db525ab8fdf4)

[2] M. Everingham, L. Van Gool, C. K. I. Williams, J. Winn, and A. Zisserman. “The PASCAL Visual Object Classes (VOC) Challenge.” *International Journal of Computer Vision*, 2010. [Paper](https://www.robots.ox.ac.uk/~vgg/publications/2010/Everingham10/)

[3] L. von Ahn and L. Dabbish. “Labeling Images with a Computer Game.” *CHI*, 2004. [Paper](https://doi.org/10.1145/985692.985733)

[4] M. Grubinger, P. Clough, H. Müller, and T. Deselaers. “The IAPR TC-12 Benchmark: A New Evaluation Resource for Visual Information Systems.” *LREC Workshop*, 2006. [Paper](https://www-i6.informatik.rwth-aachen.de/publications/download/34/Grubinger-LREC-2006.pdf)

[5] M. J. Huiskes and M. S. Lew. “The MIR Flickr Retrieval Evaluation.” *ACM MIR*, 2008. [Paper](https://doi.org/10.1145/1460096.1460104)

[6] X. Yan, J. Yin, and J. Wen. “Incomplete Multi-View Multi-label Learning via Disentangled Representation and Label Semantic Embedding.” *CVPR*, 2025, pp. 30722–30731. [Paper](https://openaccess.thecvf.com/content/CVPR2025/papers/Yan_Incomplete_Multi-View_Multi-label_Learning_via_Disentangled_Representation_and_Label_Semantic_CVPR_2025_paper.pdf)
