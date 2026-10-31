# Course source index

Companion to [COURSE_TOOLKIT.md](COURSE_TOOLKIT.md). Inventory of all 23 supplied files; reviewed 2026-10-31. Original references were not modified. Notebook cells are zero-based; PDF pages are physical, one-based pages, which can differ from printed slide numbers.

## Lecture decks

| ID | Original file | Pages | Main coverage |
|---|---|---:|---|
| L1 | [WPI_AnoDet_Lecture01.pdf](../references/WPI_AnoDet_Lecture01.pdf) | 58 | Anomaly types; raw plots and pairplots; Gaussian/tail/MAD thresholds; regression residuals; OLS/Huber/RANSAC/Theil–Sen; confusion matrix/PR/ROC. |
| L2 | [WPI_AnoDet_Lecture02.pdf](../references/WPI_AnoDet_Lecture02.pdf) | 56 | Assignment and UNSW-NB15 description (p. 9); task/split design; logistic regression; SVC, soft margins and kernels; probability/threshold analysis. |
| L3 | [WPI_AnoDet_Lecture03_unsupAD_I.pdf](../references/WPI_AnoDet_Lecture03_unsupAD_I.pdf) | 34 | Mahalanobis/Hotelling-style T²; DBSCAN and k-distance knee; OCSVM/SVDD; sensitivity and evaluation constraints. |
| L4 | [WPI_AnoDet_Lecture04_unsupAD_II.pdf](../references/WPI_AnoDet_Lecture04_unsupAD_II.pdf) | 49 | LOF, isolation forest, PCA theory, t-SNE and UMAP; neighborhood and feature-space interpretation. |
| L5 | [WPI_AnoDet_Lecture05_novelty_detection.pdf](../references/WPI_AnoDet_Lecture05_novelty_detection.pdf) | 52 | Normal-only residual models; PCA reconstruction and windows; latent-distance complement; image-task design; NN foundations and autoencoders. |
| L6 | [WPI_AnoDet_Lecture06_residual_reconstruction_CNN.pdf](../references/WPI_AnoDet_Lecture06_residual_reconstruction_CNN.pdf) | 68 | CNN features and receptive fields; pretrained ResNet152; model development and regularization; case-level error analysis. |

No Lecture 7 deck is present; N07a and N07b supply the regression/reconstruction engine material. Later topics on the course schedule are not evidence of supplied lessons.

## Notebook overview

| ID | Original notebook | Cells | Main techniques |
|---|---|---:|---|
| N01 | [WPI_AnoDet_01b_AD_statistical_solution.ipynb](../references/WPI_AnoDet_01b_AD_statistical_solution.ipynb) | 75 | Univariate and contextual temperature exploration; pairplots, OLS/Huber residuals, mean/std and median/MAD thresholds. |
| N01e | [WPI_AnoDet_01d_AD_statistical_solution_with_evaluation.ipynb](../references/WPI_AnoDet_01d_AD_statistical_solution_with_evaluation.ipynb) | 82 | Statistical workflow plus manual confusion matrices, one/two-sided thresholds and PR-area comparison. |
| N02a | [WPI_AnoDet_02a_AD_supervised_log_reg_classifier (1).ipynb](../references/WPI_AnoDet_02a_AD_supervised_log_reg_classifier%20%281%29.ipynb) | 49 | Time split, supervised probabilities, score histograms, recall-driven cutoff and temporal predictions. |
| N02b | [WPI_AnoDet_02b_exercise_AD_supervised_svm_classifier (1).ipynb](../references/WPI_AnoDet_02b_exercise_AD_supervised_svm_classifier%20%281%29.ipynb) | 7 | Exercise scaffold only: CSV loading and time split; no completed SVM fit. |
| N02c | [WPI_AnoDet_02c_solution_AD_supervised_svm_classifier.ipynb](../references/WPI_AnoDet_02c_solution_AD_supervised_svm_classifier.ipynb) | 57 | Linear SVC solution, decision scores/probabilities, manual PR evaluation and threshold selection. |
| N03a | [WPI_AnoDet_03a_AD_unsupervised_DBSCAN.ipynb](../references/WPI_AnoDet_03a_AD_unsupervised_DBSCAN.ipynb) | 24 | Robust scaling, DBSCAN, kth-neighbor distances, custom knee and estimated epsilon. |
| N03b | [WPI_AnoDet_03b_AD_unsupervised_ocsvm.ipynb](../references/WPI_AnoDet_03b_AD_unsupervised_ocsvm.ipynb) | 74 | Single/two operating states, robust scaling, median-distance gamma, boundary sweeps, score thresholds and evaluation. |
| N04a | [WPI_AnoDet_04a_AD_unsupervised_LOF_example_machine_data.ipynb](../references/WPI_AnoDet_04a_AD_unsupervised_LOF_example_machine_data.ipynb) | 33 | Local density scores, neighborhood-size ensemble, mean/median/dispersion and heuristic thresholds. |
| N04b | [WPI_AnoDet_04b_AD_unsupervised_isolation_forest_example_machine_data.ipynb](../references/WPI_AnoDet_04b_AD_unsupervised_isolation_forest_example_machine_data.ipynb) | 61 | Single/two-state comparison, shuffled-index plots, IF scores, conventional/robust thresholds and PR evaluation. |
| N04d | [WPI_AnoDet_04d_FMNIST_dimension_reduction_visualization.ipynb](../references/WPI_AnoDet_04d_FMNIST_dimension_reduction_visualization.ipynb) | 33 | Balanced sampling, image inspection, pixel scaling, PCA variance, PCA→t-SNE/UMAP, runtime and labeled silhouette. |
| N05a | [WPI_AnoDet_05a_PCA_time_series.ipynb](../references/WPI_AnoDet_05a_PCA_time_series.ipynb) | 83 | Normal prefix, pointwise/window PCA, endpoint alignment, reconstruction scores, percentiles, histogram-tail thresholds and causal smoothing. |
| N05b | [WPI_AnoDet_05b_PCA_FMNIST.ipynb](../references/WPI_AnoDet_05b_PCA_FMNIST.ipynb) | 27 | Normal class versus one/some/all other image classes; inspect image embedding overlay, reconstructions and error histograms. |
| N05c | [WPI_AnoDet_05c_PCA_AE_FMNIST.ipynb](../references/WPI_AnoDet_05c_PCA_AE_FMNIST.ipynb) | 42 | Raw-pixel PCA versus dense AE, same image-task design, reconstructions and PR comparison. |
| N06a | [WPI_AnoDet_06a_PCA_AE_FMNIST_pretrained_feature_extractor.ipynb](../references/WPI_AnoDet_06a_PCA_AE_FMNIST_pretrained_feature_extractor.ipynb) | 59 | ResNet152 pooled features versus pixels; PCA/AE, feature residual displays and score comparisons. t-SNE is discussed but not computed here. |
| N06b | [WPI_AnoDet_06b_cifar10_tricks.ipynb](../references/WPI_AnoDet_06b_cifar10_tricks.ipynb) | 54 | Image inspection, RF/CNN, augmentation inspection, capacity/regularization comparisons, class recall and confidence-based error review. |
| N07a | [WM_AnDet_07a_PCA_AE_NCMAPSS.ipynb](../references/WM_AnDet_07a_PCA_AE_NCMAPSS.ipynb) | 64 | Raw sensor dictionary/traces, early/late comparisons, sampled pairplots, cycle means, normal-only PCA/AE, cycle-score calibration and variable errors. |
| N07b | [WM_AnDet_07b_regression_NCMAPSS.ipynb](../references/WM_AnDet_07b_regression_NCMAPSS.ipynb) | 65 | Shared raw-data exploration, operating-condition→sensor linear/NN regression, signed per-sensor residuals, cycle means/medians and group histograms. |

## Notebook navigation

Section titles below are taken from notebook headings to make original examples easy to find; headings alone do not imply completed implementation. Consult the toolkit for demonstrated/theory/suggested distinctions.

### N01

- Cell 0: Anomaly detection in component temperatures
- Cell 2: Univariate example: isolated bearing temperature
- Cell 3: Read data
- Cell 5: Visualize data
- Cell 7: detect anomalies with a statistical threshold
- Cell 8: Histogram
- Cell 11: Think
- Cell 12: Multivariate example bearing temperature
- Cell 13: Read data
- Cell 15: visualize data
- Cell 19: Visualize all variables
- Cell 21: Think
- Cell 22: Correlations between variables: pair plot
- Cell 25: AD with linear regression
- Cell 27: Plot linear regression
- Cell 29: Improve model: robust linear regression
- Cell 31: plot linear fit
- Cell 33: plot residuals
- Cell 35: Think
- Cell 36: Statistical threshold on residuals
- Cell 38: Plot the original data with the detected anomalies
- Cell 40: Robust anomaly detection
- Cell 43: visualize data
- Cell 45: Calculate statistical threshold
- Cell 47: Visualize the detection results
- Cell 50: Solution: robust thresholding
- Cell 52: Visualize robust thresholding results
- Cell 55: Robust multivariate anomaly detection
- Cell 56: Read data
- Cell 58: Visualize data
- Cell 60: Correlation plots
- Cell 62: Linear regression
- Cell 64: Robust linear regression
- Cell 66: Plot both fits
- Cell 69: Plot residuals
- Cell 72: Anomaly Detection with Robust Thresholding

### N01e

- Cell 0: Anomaly detection in component temperatures
- Cell 2: Univariate example: isolated bearing temperature
- Cell 3: Read data
- Cell 5: Visualize data
- Cell 7: detect anomalies with a statistical threshold
- Cell 8: Histogram
- Cell 11: Think
- Cell 12: Multivariate example bearing temperature
- Cell 13: Read data
- Cell 15: visualize data
- Cell 19: Visualize all variables
- Cell 21: Think
- Cell 22: Correlations between variables: pair plot
- Cell 25: AD with linear regression
- Cell 27: Plot linear regression
- Cell 29: Improve model: robust linear regression
- Cell 31: plot linear fit
- Cell 33: plot residuals
- Cell 35: Think
- Cell 36: Statistical threshold on residuals
- Cell 38: Plot the original data with the detected anomalies
- Cell 40: Robust anomaly detection
- Cell 43: visualize data
- Cell 45: Calculate statistical threshold
- Cell 47: Visualize the detection results
- Cell 50: Solution: robust thresholding
- Cell 52: Visualize robust thresholding results
- Cell 55: Robust multivariate anomaly detection
- Cell 56: Read data
- Cell 58: Visualize data
- Cell 60: Correlation plots
- Cell 62: Linear regression
- Cell 64: Robust linear regression
- Cell 66: Plot both fits
- Cell 69: Plot residuals
- Cell 71: Anomaly Detection with Robust Thresholding
- Cell 73: Confusion Matrix
- Cell 77: Precision recall curve (PRC)
- Cell 79: Evaluation using precision recall curves

### N02a

- Cell 0: Supervised anomaly detection with logistic regression
- Cell 2: Read data
- Cell 4: Visualize the data
- Cell 5: on the time axis
- Cell 7: pair plot
- Cell 10: Classifying with logistic regression
- Cell 12: Train test split
- Cell 14: Train logistic regression model and predict
- Cell 16: Plot predictions
- Cell 18: Think
- Cell 19: Refine the predictions using the probabilities
- Cell 21: Plot the predicted probabilities
- Cell 23: Evaluation: precision recall
- Cell 24: Define anomaly scores as prediction probabilities
- Cell 27: Own precision-recall calculation
- Cell 30: Precision-recall curve
- Cell 32: Select a detection threshold
- Cell 35: Think
- Cell 36: Select a decision criterion
- Cell 40: Precision and recall vs. threshold for the training data
- Cell 42: Presicion and recall vs. threshold for the test data
- Cell 44: Plot as function of time
- Cell 48: Think

### N02b

- Cell 0: Supervised anomaly detection with support vector machines (SVM)
- Cell 2: Read data
- Cell 4: Train-test split

### N02c

- Cell 0: Supervised anomaly detection with support vector machines (SVM)
- Cell 2: Read data
- Cell 4: Visualize the data
- Cell 5: On the time axis
- Cell 7: pair plot
- Cell 10: Train-test split
- Cell 12: Classifying with SVM
- Cell 15: Plot predictions
- Cell 17: Think
- Cell 18: Refine the predictions using the decision function and probabilities
- Cell 20: Plot the SVM decision scores
- Cell 22: Plot the predicted probabilities
- Cell 25: Evaluation: precision recall
- Cell 26: Define anomaly scores as prediction probabilities
- Cell 29: Own precision-recall calculation
- Cell 33: Precision-recall curve
- Cell 35: Select a detection threshold
- Cell 38: Think
- Cell 39: Select a decision criterion
- Cell 43: Precision and recall vs. threshold for the training data
- Cell 45: Presicion and recall vs. threshold for the test data
- Cell 47: Visualize the results
- Cell 48: Plot as function of time
- Cell 51: Plot the decision boundary in variable space
- Cell 53: Extensions

### N03a

- Cell 0: Unsupervised AD with DBSCAN clustering
- Cell 1: Imports
- Cell 3: Download data
- Cell 5: Visualize the data
- Cell 7: AD with DBSCAN
- Cell 8: extract only the variables from the dataframe
- Cell 10: DBSCAN prediction
- Cell 12: plot results
- Cell 13: plot DBSCAN prediction
- Cell 16: Determine hyperparameters
- Cell 20: Repeat DBSCAN with estimated parameters
- Cell 22: Think

### N03b

- Cell 0: Unsupervised AD with One Class Support Vector Machines
- Cell 2: Load data
- Cell 4: Single state data
- Cell 6: Visualize the data
- Cell 8: Extract only the variables from the dataframe
- Cell 10: Rescale data
- Cell 12: Plot decision boundaries for several values of $\gamma$
- Cell 15: Think
- Cell 16: Fit and predict OCSVM with a selected value of $\gamma$
- Cell 18: Plot ocsvm predictions
- Cell 20: Plot ocsvm anomaly scores
- Cell 23: Detection threshold
- Cell 26: Detection threshold: robust
- Cell 28: Plot robust threshold
- Cell 31: Think
- Cell 32: Two state data
- Cell 33: Random shuffling of the rows of the dataframe
- Cell 35: Visualize the data
- Cell 37: Extract only the variables from the dataframe
- Cell 39: Fit and predict with ocsvm
- Cell 41: Plot decision boundaries for several values of $\gamma$
- Cell 43: Fit and predict OCSVM with selected hyperparameters
- Cell 45: Plot ocsvm predictions
- Cell 47: Plot ocsvm anomaly scores
- Cell 50: Detection threshold
- Cell 53: Detection threshold: robust
- Cell 55: Plot robust threshold
- Cell 57: Evaluation
- Cell 59: Confusion matrix
- Cell 62: Precision-recall curve
- Cell 64: Define a set of thresholds
- Cell 66: calculate precision and recall using own function
- Cell 69: calculate precision and recall using sklearn function

### N04a

- Cell 0: Unsupervised anomaly detection with Local Outlier Factor (LOF); Example: synthetic machine data
- Cell 2: Read data
- Cell 4: Visualize the data
- Cell 6: AD with LOF
- Cell 7: extract only the variables from the dataframe
- Cell 9: run LOF with specific hyperparameters
- Cell 11: plot results
- Cell 12: plot lof prediction
- Cell 15: plot LOF scores
- Cell 17: Think
- Cell 18: Ensemble of LOFs
- Cell 20: plot statistics of the scores
- Cell 22: Think
- Cell 23: Detection threshold
- Cell 26: Detection threshold: robust
- Cell 29: Plot robust threshold
- Cell 31: Think

### N04b

- Cell 0: Unsupervised AD with isolation forest; Synthetic machine data example
- Cell 2: Load data
- Cell 4: Single state data
- Cell 6: Random shuffling of the rows of the dataframe
- Cell 9: Extract only the variables from the dataframe
- Cell 11: Fit and predict with iForest
- Cell 13: Plot iForest predictions
- Cell 15: Plot iForest anomaly scores
- Cell 17: Detection threshold
- Cell 20: Detection threshold: robust
- Cell 22: Plot robust threshold
- Cell 25: Two state data
- Cell 26: Random shuffling of the rows of the dataframe
- Cell 28: Visualize the data
- Cell 30: Extract only the variables from the dataframe
- Cell 32: Fit and predict with iForest
- Cell 34: Plot iForest predictions
- Cell 36: Plot iForest anomaly scores
- Cell 38: Detection threshold
- Cell 41: Detection threshold: robust
- Cell 43: Plot robust threshold
- Cell 45: Evaluation
- Cell 47: Confusion matrix
- Cell 50: Precision-recall curve
- Cell 52: Define a set of thresholds
- Cell 54: calculate precision and recall using own function
- Cell 57: calculate precision and recall using sklearn function

### N04d

- Cell 0: Dimensionality reduction for visualization with Fashion-MNIST
- Cell 1: Install and import packages
- Cell 4: Load Fashion-MNIST
- Cell 6: Select a balanced subset
- Cell 8: Inspect example images
- Cell 10: Prepare the raw-pixel feature matrix
- Cell 12: Helper function for two-dimensional plots
- Cell 14: Dimensionality reduction from raw pixels; PCA
- Cell 16: Examine how much variance is retained
- Cell 18: t-SNE
- Cell 21: Optional experiment: effect of perplexity
- Cell 23: UMAP
- Cell 25: alternative: UMAP on pixels
- Cell 27: Optional experiment: local versus global structure
- Cell 29: Compare the three embeddings
- Cell 31: A cautious quantitative comparison; Silhouette score; Interpretation; Important note

### N05a

- Cell 0: Anomaly detection with Principal Component Analysis (PCA)
- Cell 2: Load data
- Cell 4: Visualize the data
- Cell 5: on the time axis
- Cell 7: pair plot
- Cell 10: Split to train and test data
- Cell 12: PCA on single time stamps
- Cell 15: number of input features and principal components
- Cell 17: Plot histogram of reconstruction errors
- Cell 20: Determine detection threshold
- Cell 21: Statistical threshold
- Cell 24: Geometric threshold
- Cell 27: Think
- Cell 28: Threshold on moving mean of reconstruction errors
- Cell 30: Statistical threshold
- Cell 32: Histogram of smoothed reconstruction errors
- Cell 34: Geometric threshold
- Cell 37: Think
- Cell 38: Evaluation: precision recall
- Cell 39: Define anomaly scores as reconstruction errors
- Cell 42: Own precision-recall calculation
- Cell 46: Precision-recall curve
- Cell 49: Plot detected anomalies on the time axis
- Cell 51: PCA on time series
- Cell 53: number of input features and principal components
- Cell 55: Optional: select the number of principal components
- Cell 57: Statistical threshold
- Cell 59: Histogram of reconstruction errors
- Cell 61: Geometric threshold
- Cell 65: Threshold on moving mean of reconstruction errors
- Cell 68: Statistical threshold
- Cell 70: Histograms of reconstruction errors
- Cell 72: Geometric threshold
- Cell 75: Evaluation and model comparison
- Cell 78: Plot detected anomalies on the time axis
- Cell 81: Think

### N05b

- Cell 0: Anomaly detection on images with PCA reconstruction models
- Cell 1: 1. DATASET - Fashion MNIST
- Cell 3: Think
- Cell 4: ; Class names; 
- Cell 5: PCA on the raw pixels
- Cell 7: Train PCA on the normal class
- Cell 9: Test on a testset with anomalies
- Cell 12: Histogram of reconstruction errors
- Cell 16: Evaluation: precision recall curve
- Cell 19: Function to calculate precision and recall
- Cell 21: define threshold values
- Cell 23: plot precision-recall
- Cell 25: Your exercise

### N05c

- Cell 0: Anomaly detection on images with PCA reconstruction models
- Cell 1: 1. DATASET - Fashion MNIST
- Cell 3: Think
- Cell 4: ; Class names; 
- Cell 5: PCA on the raw pixels
- Cell 7: Train PCA on the normal class
- Cell 9: Test on a testset with anomalies
- Cell 12: Histogram of reconstruction errors
- Cell 16: Evaluation: precision recall curve
- Cell 19: Function to calculate precision and recall
- Cell 21: define threshold values
- Cell 23: plot precision-recall
- Cell 25: Think
- Cell 26: Autoencoder (AE) on raw pixels
- Cell 30: Reconstruct using the trained model
- Cell 32: Reconstruction errors
- Cell 34: Visualize original and reconstructed images
- Cell 36: Evaluate: precision recall curve
- Cell 40: Think

### N06a

- Cell 0: Anomaly detection on images with reconstruction models
- Cell 1: 1. DATASET - Fashion MNIST
- Cell 3: Think
- Cell 4: ; Class names; 
- Cell 5: PCA on the raw pixels
- Cell 7: Train PCA on the normal class
- Cell 9: Test on a testset with anomalies
- Cell 12: Histogram of reconstruction errors
- Cell 15: Evaluation: precision recall curve
- Cell 20: Think
- Cell 21: PCA on pretrained features
- Cell 22: Use ResNet representations for the visualisation of the Dataset; Feature Extraction; Dimensionality Reduction
- Cell 23: Define train and test sets
- Cell 26: Define test set with anomalies
- Cell 28: Extract ResNet features for train and test set
- Cell 30: Fit PCA on the extracted features
- Cell 32: Calculate reconstruction errors
- Cell 35: Plot reconstruction maps for normal and abnormal images
- Cell 38: Evaluation: precision recall curve
- Cell 42: investigate the model comparison
- Cell 47: Autoencoder (AE) on pretrained features
- Cell 49: MSE for individual samples
- Cell 51: Reconstruction error heatmaps
- Cell 53: Evaluation: precision recall curve
- Cell 57: Think

### N06b

- Cell 0: Cifar10 classification tricks
- Cell 1: Helping functions
- Cell 4: Data loading
- Cell 6: Random forest
- Cell 9: Baseline CNN model
- Cell 14: Dropout
- Cell 17: Effect of dropout
- Cell 18: Data augmentation
- Cell 20: Visualize the augmentations
- Cell 24: Effect of augmentation
- Cell 25: Increase CNN capacity
- Cell 27: Going deeper
- Cell 29: Both wider and deeper
- Cell 33: Batch Normalization
- Cell 36: Effect of BN
- Cell 37: Weight decay
- Cell 39: Effect of Weight decay
- Cell 40: Error analysis
- Cell 41: Confusion matrix
- Cell 43: Per-class accuracy
- Cell 45: Visualize misclassified examples
- Cell 47: High confidence errors
- Cell 49: Low confidence correct classifications
- Cell 52: 🔧 **YOUR TASK:**

### N07a

- Cell 0: Anomaly detection in N-CMAPSS with PCA and an autoencoder
- Cell 1: 1. Load the dataset
- Cell 2: The N-CMAPSS variables; Operating conditions (inputs); Sensor measurements (responses)
- Cell 6: Development and test engines
- Cell 8: Variable groups
- Cell 11: Explore the raw data
- Cell 13: raw variables over several cycles
- Cell 16: What can we observe?
- Cell 17: compare early with late cycles
- Cell 19: Think
- Cell 20: Pairplot
- Cell 25: 2. Inspect the degradation trajectories
- Cell 28: Think
- Cell 29: 3. Preprocess the data
- Cell 31: Labels for evaluation
- Cell 33: 4. PCA reconstruction model
- Cell 34: Think
- Cell 37: One anomaly score per engine cycle
- Cell 41: Think
- Cell 42: 5. Autoencoder reconstruction model
- Cell 49: Think
- Cell 50: 6. Compare PCA and the autoencoder
- Cell 53: Think
- Cell 55: 7. What does the model tell us?
- Cell 58: Can you do better?
- Cell 59: Optional detection schemes:
- Cell 60: Final Think: reconstruction or regression?
- Cell 61: Plot signed reconstruction errors per variable

### N07b

- Cell 0: Anomaly detection in N-CMAPSS with regressio models
- Cell 1: 1. Load the dataset
- Cell 2: The N-CMAPSS variables; Operating conditions (inputs); Sensor measurements (responses)
- Cell 6: Development and test engines
- Cell 8: Variable groups
- Cell 11: Explore the raw data
- Cell 13: raw variables over several cycles
- Cell 16: What can we observe?
- Cell 17: compare early with late cycles
- Cell 19: Think
- Cell 20: Pairplot
- Cell 25: 2. Inspect the degradation trajectories
- Cell 28: Think
- Cell 29: 3. Preprocess the data
- Cell 31: Labels for evaluation
- Cell 33: Multiple linear Regression
- Cell 35: Fit linear regression
- Cell 37: Predict
- Cell 39: Plot predictions
- Cell 41: plot residuals
- Cell 43: Cycle aggregated residuals
- Cell 45: compare residual histograms of training and test data
- Cell 47: Regression NN
- Cell 48: imports
- Cell 50: prepare inputs and targets (same as above)
- Cell 52: define asmall NN
- Cell 54: train the NN
- Cell 56: plot training summary
- Cell 58: calculate predictions
- Cell 60: calculate residuals
- Cell 62: plot residuals per variable and cycle

## Coverage limits

- Inspection used extracted PDF text and notebook markdown/code. This is not a visual audit of every slide diagram or stored notebook output.
- Original notebooks were not executed; external CSV/HDF5/image resources and pretrained weights were not fetched.
- Notebooks mentioned in slides but absent here include the dedicated robust-regression example, credit-card exercise/solution notebooks, and the pretrained-feature t-SNE solution. Their contents were not inferred.
- Library-specific corrections in the toolkit are interpretation notes to check against installed versions before implementation.
- Persistent project context is linked from `AGENTS.md`; this preserves the toolkit across workspace sessions without claiming permanent model training.
