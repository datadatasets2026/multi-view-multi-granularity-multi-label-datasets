# Multi-View Multi-Granularity Benchmarks

These datasets extend five public multi-view benchmarks (Corel5K, Pascal07, ESPGame, IAPRTC12, and MIRFlickr) with coarser label annotations and mappings between adjacent granularities. The original multi-view features and fine-grained annotations are retained.

Corel5K, ESPGame, and IAPRTC12 have three label granularities. Pascal07 and MIRFlickr have two.

## Dataset Construction

1. **Compute label affinities.** Estimate label occurrence and co-occurrence probabilities from the binary annotations at the current granularity. Combine Jaccard similarity, nonnegative normalized pointwise mutual information, and the average of the two directional conditional probabilities into a pairwise label affinity matrix.

2. **Group labels.** Determine the number of groups from the size of the current label set. Initialize each group with one of the most frequent labels, then assign the remaining labels in decreasing order of occurrence frequency. Each label goes to the group with the highest score, computed as its average affinity to the current group members minus a penalty for excessive group size and sample coverage after adding the label. Sample coverage is the fraction of samples with at least one label in the group.

3. **Generate coarser labels and mappings.** Each group defines one coarse-grained label. A sample is positive for that label if it is positive for any label in the group. Group membership is recorded as a binary fine-to-coarse mapping. To obtain another granularity, recompute label affinities from the newly generated annotations and repeat the procedure.
