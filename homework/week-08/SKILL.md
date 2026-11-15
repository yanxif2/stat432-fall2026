---
name: lda-qda-report
description: Compare ordinary LDA and QDA and produce a one-page statistical report only when explicitly invoked with a dataset, response, and predictors.
---

# LDA/QDA report

Keep this file outside automatically loaded skill folders. Apply it only when the user explicitly supplies this file and requests its use.

Use the dataset, response, predictors, and categorical coding supplied in the invocation. Compare only ordinary LDA and QDA, with empirical class priors and no regularization, feature selection, PCA, or model combinations.

1. Report a random seed. Reserve a stratified 30% test set before comparing models. Use the remaining 70% for model selection.
2. Compare LDA and QDA using the same shuffled, stratified five-fold cross-validation splits within the training data. Use misclassification rate. Estimate means and covariances within each fold; fit any necessary preprocessing only on that fold's training portion. Report fitting failures rather than silently changing the models.
3. Review the analysis using training and cross-validation results only. Select the model with the smaller mean cross-validation error; break an exact tie in favor of LDA. Refit it on all training observations and evaluate it once on the reserved test set. Do not revise the skill or model choice based on test results.
4. Produce a readable report of at most one printed page. Include a short methods paragraph, a table of both models' cross-validation errors, a labeled figure comparing those errors, the selected model, and its test misclassification count and rate. State training/test sizes, seed, fitted class counts or priors, and the covariance assumptions. Briefly explain the findings and limitations, including uncertainty from one split. Verify that the report fits one page.
