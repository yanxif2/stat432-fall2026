---
name: cluster-report
description: Compare at least three clustering algorithms on a user-supplied dataset, select one, and write a one-page report on the selected clustering. Only use when explicitly invoked as /cluster-report.
disable-model-invocation: true
---

Intent: find the most convincing grouping of the observations, choose one clustering method and number of clusters using only the features, and explain the groups on one printed page.

## Inputs (ask if any are missing)
Dataset path, feature columns, ID column, optional known-label column, random seed, output file, and any file to append. Default to Python (scikit-learn), seed 432. State every default used. Assume nothing about the data beyond what is given.

## Analysis rules
1. **Check the data.** Report n, p, and missing values. Stop and ask if features are non-numeric or missing without instructions. Note when p is much larger than n or n is small.
2. **Keep labels out.** Never use the known labels for preprocessing, tuning, choosing the number of clusters, or choosing the method. Use them only after the final choice, to interpret it.
3. **Preprocess.** Check whether features are already on a common scale. Standardize them if not, and say which. If p is large, reduce with PCA and keep enough components for about 80-90% of the variance or up to the scree elbow. Feature filtering must be unsupervised, such as top variance. State every choice.
4. **Fit candidates.** Use at least three different algorithms: K-means (n_init >= 20), agglomerative hierarchical clustering (Ward on Euclidean, plus average linkage on correlation distance), and spectral clustering (nearest-neighbor affinity, tune n_neighbors on a small grid). A Gaussian mixture may be added. Dimension reduction alone is not a clustering algorithm. Try K = 2 to min(8, n/5).
5. **Pick K and method.** State this rule before looking at results. For each method and K, compute the average silhouette, Calinski-Harabasz, and Davies-Bouldin indices. Compute stability as the mean adjusted Rand index between the full-data clustering and clusterings of 50 random 80% subsamples, over shared observations. Reject solutions with a cluster under 5% of n. Prefer high silhouette and stability. If scores are within about 0.02, choose the smaller K and the simpler method (K-means, then hierarchical, then spectral).
6. **Describe the final clustering.** Refit the selected method. Report cluster sizes and the silhouette of each cluster. Find the features that most separate the clusters (for example, the largest absolute standardized mean differences). If labels exist, cross-tabulate clusters against labels, give the adjusted Rand index, and list misassigned IDs.

## Report (at most ONE printed letter page)
The whole report must fit on one printed page, including all text, figures, and tables. Present clustering results from ONLY the selected method. Other methods may be named, with their key selection scores, in one sentence or one compact row each. Do not show their cluster assignments, plots, or cross-tabs.

Write a Quarto .qmd file (embed-resources: true, html-math-method: mathml) with code chunks set to echo: false. Use these sections, about 250 words of prose in total:

- **Data and preprocessing** (2 sentences): n, p, scaling, dimension reduction, seed.
- **Methods and selection** (3-4 sentences): the algorithms tried, the selection rule, and why the chosen method and K won. Cite the deciding numbers.
- **Figure** (about 6.5 x 3 in, font >= 9 pt, colorblind-safe): two panels for the selected method only. Left, the first two principal components colored by cluster, with marker shape by known label if one exists. Right, a silhouette plot or a cluster-ordered heatmap of the top separating features. Label axes in plain words, including PC variance percentages.
- **Table**: cluster sizes, and the cluster-by-label cross-tab with ARI if labels exist. Keep it small.
- **Interpretation** (3-4 sentences): what distinguishes the groups, how well they match the known labels, and which observations do not fit.
- **Limitations** (2-3 bullets): for example, small n, sensitivity to preprocessing and distance, uncertainty in K, and that agreement with labels is not proof the clusters are real.

After rendering, print to a letter-size PDF and count the pages. If the report runs over one page, cut prose before evidence and shrink the figure before dropping it. If asked to append a file, add it verbatim after a page break as an appendix. The appendix does not count toward the page limit.

## Behavior
Base every claim on computed numbers. Do not overstate small differences between methods. Write short, plain sentences, with a heading for each section. Put the selected method and its key evidence first. Print package versions and the seed in a small line. At the end, tell the user the file path, the selected method and K, the key scores, and anything you could not do.
