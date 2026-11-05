# Multi-View Multi-Granularity Multi-Label Benchmarks
This collection extends five public multi-view benchmarks with coarse-grained label annotations. The underlying image datasets are Corel5K [1], Pascal07 [2], ESPGame [3], IAPRTC12 [4], and MIRFlickr [5]. For each benchmark, we use the six precomputed feature views as provided and retain the original fine-grained annotations.

## Multi-Granularity Label Construction
1. **Compute label affinities.** Estimate label occurrence frequencies and co-occurrence probabilities from the binary annotations at the current granularity. Combine Jaccard similarity, nonnegative normalized pointwise mutual information, and the average of the two directional conditional probabilities to compute pairwise label affinities.
2. **Cluster labels.** Adaptively determine the number of clusters based on the size of the current label set. Seed each cluster with one of the most frequent labels. Assign the remaining labels based on their average affinity to the current members of each cluster, minus a penalty based on cluster size and sample coverage. Larger clusters incur a higher penalty. Sample coverage is the proportion of samples that have at least one label in the resulting cluster.
3. **Generate coarser labels.** Use the label-to-cluster assignments from Step 2 to construct the label distribution matrix for the instances. To obtain a coarser granularity, recompute label affinities from the newly generated coarse-grained labels and repeat the clustering procedure.

## Dataset Format
Each dataset is provided as a MATLAB `.mat` file containing the multi-view feature matrices `X`, the multi-granularity multi-label distribution matrices `Y`, and the cross-granularity relation matrices `H`.
1. `X` is a `1 × 6` cell array of feature matrices, with one row per sample in each matrix. For all five datasets, the view dimensions in the order stored in `X` are **100, 1,000, 512, 4,096, 4,096, and 4,096**.
2. `Y` consists of binary label matrices. The rows follow the sample order in `X`, and the columns correspond to labels. An entry is `1` if the corresponding label is present and `0` otherwise. For example, `Y_L4` stores the original labels at the finest granularity, while `Y_L3` and `Y_L2` store progressively coarser labels.
3. `H` consists of binary relation matrices. Rows index finer labels, and columns index coarser labels. An entry is `1` if the corresponding finer and coarser labels are related and `0` otherwise. For example, `H34` and `H23` map L4 labels to L3 labels and L3 labels to L2 labels, respectively. `H23` is included only in datasets with L2 labels.

**Details of the dataset are summarized as follows:**
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
