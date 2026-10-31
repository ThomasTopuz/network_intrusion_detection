# Anomaly detection course toolkit

Prepared from the six lecture PDFs and 17 notebooks supplied in `references/`, reviewed on 2026-10-31. This is the reusable reference for future work in this project. **Exploration and analysis come first; fitting detectors comes later.**

Scope: lecture text and notebook markdown/code were inspected. Notebooks were not executed, external datasets and linked resources were not downloaded, and stored plots were not independently reproduced. This records methods and reasoning, not verified numerical results. No Lecture 7 PDF is supplied; its two engine notebooks are included. Scheduled future lecture topics are not treated as covered methods.

Source abbreviations below are resolved in [COURSE_SOURCES.md](COURSE_SOURCES.md), with original file links, PDF page counts and notebook cell locations. PDF references use physical, one-based PDF pages; notebook cells use zero-based indices. Status means **demonstrated** in supplied code, **theory** in slides, or **suggested** as an exercise/optional approach.

## 1. Default investigation workflow

1. **Define the task.** What is an observation, what is normal, and what decision should an alert support? Separate point, contextual and collective anomalies. Identify supervised, unsupervised, or normal-only novelty detection. Write down which labels are available and when.
2. **Inspect original data before transformation.** Establish shape, feature meanings, units, identifiers, categorical variables, sampling order, and label prevalence. Inspect raw traces or representative samples before averaging or scaling hides structure.
3. **Explore marginals.** Compare histograms, center, spread, quantiles, tails, and class/regime proportions. Look for multiple operating modes and contamination. Keep suspicious records available for investigation.
4. **Explore relationships and context.** Use scatterplots and selected pairplots. Compare regimes, entities, early/late periods and normal/anomalous groups when labels are permitted. Ask whether an apparent anomaly is explained by a change in conditions.
5. **Choose representations deliberately.** Compare raw units and appropriate scaling. Use PCA, t-SNE and UMAP to investigate high-dimensional structure. State what information each representation can lose.
6. **Design splits before estimating transformations.** Define training, validation and final testing, respecting time and entity boundaries. Identify a defensible normal reference if using novelty detection. Fit preprocessing on training data only.
7. **Use simple analysis baselines if needed.** Compare conventional and robust statistics; inspect regression or reconstruction residuals, including individual variables, before increasing model complexity.
8. **Inspect score distributions and individual cases.** Keep continuous scores. Check tails, parameter sensitivity, false positives, false negatives and original observations. Choose thresholds from the intended reference/validation population.
9. **Report evidence and limits.** Distinguish measured behavior from hypotheses, label-assisted exploration from label-free detection, and exploratory comparisons from independent evaluation.

Steps concerning schema audits, missing data and duplicates below are practical project extensions to the lecture workflow, not claimed as dedicated classroom exercises.

## 2. Exploration and preparation methods — primary toolkit

| Method | What we learn or produce | Source / status | Interpretation or implementation rule |
|---|---|---|---|
| Task and anomaly taxonomy | Point versus contextual versus collective anomalies; what counts as normal | L1 pp. 14–18; theory | A normal marginal value can still violate context; isolated-row methods can miss collective behavior. |
| Learning-setting design | Supervised classification, unlabeled outlier detection, normal-only novelty detection | L2 pp. 18–30; L3 p. 6; theory | State how normal examples are identified. Labels needed for evaluation need not be used in fitting. |
| Data loading and structural inspection | CSV DataFrames, HDF5 arrays, feature/label shapes and entity IDs | N01, N03a/b, N07a/b; demonstrated | CSV via pandas; HDF5 via `h5py`. Inspect schema and variable names rather than treating every numeric column as a measurement. |
| Feature dictionary with units and roles | Meaning of each variable; conditions/commands versus responses | N07a/b cells 1–10; demonstrated | Engine conditions `W` differ physically from sensor outputs `Xs`; engine/cycle IDs are metadata. |
| Representative examples and class counts | What a typical observation looks like and how balanced a subset is | N04d cells 5–11; N06b cell 5; demonstrated | Image grids reveal ambiguity that a metric hides. A balanced visualization subset does not preserve population prevalence. |
| Raw time/index plots | Abrupt changes, drifts, unusual periods and regime transitions | N01 cells 6, 16–20; N03a/b; N07a/b cells 11–19; demonstrated | Preserve true order. Row index is not automatically time; shuffled rows cannot establish temporal behavior. |
| Local zoom and synchronized subplots | Compare variables around suspicious intervals | N01 cells 17–20; demonstrated | Share the time axis and retain original units. A local excursion may follow an external condition. |
| Early/late and entity comparisons | Separate degradation hypotheses from operating-condition and unit differences | N07a/b cells 17–28; demonstrated | Different operating ranges can explain a shift; early/late comparisons alone do not identify degradation. |
| Histograms and overlaid distributions | Shape, tails, multimodality and group overlap | N01; N04a; N05b/c; N07b cell 46; demonstrated | Use common bins and comparable normalization. `density=True` produces density, not raw counts. |
| Conventional descriptive statistics | Mean, standard deviation, variance and quantiles | L1; N01; demonstrated | Outliers can move both mean and standard deviation, hiding themselves. Document sample versus population standard deviation convention. |
| Robust descriptive statistics | Median, MAD, IQR and other quantile spans | L1 pp. 50–56; N01; N04a; demonstrated | `MAD = median(abs(x - median(x)))`; Gaussian-equivalent spread is `1.4826 * MAD`. IQR means Q75−Q25. |
| Scatterplots and pairplots | Correlations, nonlinear relationships, clusters, dependence violations | L1 pp. 29–35; N01 cells 22–24; N07a/b cells 20–23; demonstrated | Custom matplotlib grids use diagonal histograms; seaborn supports `hue` and `corner=True`. Correlation is not causation. |
| Selected and sampled pairplots | Readable exploration of large/high-dimensional data | N07a/b cells 21, 23; demonstrated | Select meaningful variables and sample reproducibly (up to 2,000 rows in the example); preserve rare cases in a separate view. |
| Group-colored comparisons | Compare early/late, normal/anomalous or operating-state geometry | N07a/b cell 23; N03b/N04b single versus two states; demonstrated | Labels can explain structure during exploration, but are excluded from features in an unsupervised experiment. |
| Standardization | Center and scale numerical variables using training mean/std | N05a cells 13–14, 52; demonstrated | Necessary when units would dominate variance/distances; near-constant features need special handling. |
| Robust scaling | Center on median and scale by an interquartile range | N03a/b, N04a/b; demonstrated | Reduces influence of extreme observations on scale estimates; does not remove them or make all shapes Gaussian. |
| Min–max and pixel scaling | Put sensor features on a reference range; pixels on [0,1] | N07a/b cell 30; N04d cell 11; N05b/c; demonstrated | Fit min–max limits on training data. Unseen values can lie outside [0,1]. Division by 255 is a fixed image conversion, not learned scaling. |
| Reproducible subsampling | Practical exploration and fair comparisons | N04d cells 3, 7; N06a shared sample indices; demonstrated | Reuse row/image identities across alternatives and set seeds. Record original and sampled prevalence separately. |
| Convert a multiclass dataset into an anomaly task | One normal class versus one (OVO), some (OVS), or all other classes (OVA) | L5 p. 24; N05b/c and N06a; demonstrated/configurable | Choose normal/abnormal semantics and mixture counts explicitly. Task difficulty changes with class similarity; train normal-only for novelty detection. |
| Temporal and entity split design | Prevent future/entity information from entering development | N02a/c first 30 days; N05a first 9 normal days; N07a/b separate engines; demonstrated | Classroom split choices are examples, not universal rules; see caveats below on shared cycles and preprocessing. |
| Sliding multivariate windows | Represent temporal patterns rather than isolated measurements | L5 pp. 17–20; N05a cells 11, 52; demonstrated | Flatten M steps × d features into M·d inputs. Associate each window with its end timestamp and align labels consistently. |
| Backward moving mean | Reveal persistent score changes and suppress isolated fluctuations | N05a cells 28–36, 65–74; demonstrated | Causal: current and past values only. Smoothing changes score distributions and can delay or hide brief events. |
| Cycle/group aggregation | Expose slow trajectories and compare entities at a meaningful time scale | N07a/b cells 26–27; N07a cell 38; N07b cells 44, 46, 63; demonstrated | Group by both unit and cycle. Mean signed residual, mean squared error and median residual answer different questions. |
| PCA projection and variance curves | Linear structure and the number of dimensions needed for normal variation | L4 pp. 23–31; N04d cells 14–17; demonstrated | Examine `explained_variance_ratio_` and cumulative variance (90%, 95% examples); retained variance does not guarantee anomaly sensitivity. |
| PCA loadings | Which original variables contribute to a principal direction | L5 p. 15; theory | Inspect component weights together with feature scaling. PCs are uncorrelated, not necessarily statistically independent. |
| PCA → t-SNE | Explore local neighborhoods in 2D | L4 pp. 32–37; N04d cells 18–22; demonstrated, with suggested perplexity sweep | Notebook reduces 784 pixels to 50 PCs, then uses perplexity 30; proposed comparisons use 5/30/50. Distant-cluster distances and sizes are unreliable. |
| PCA → UMAP | Explore neighborhood structure at different scales | L4 pp. 38–45; N04d cells 23–28; demonstrated, with optional alternatives | Example: 50 PCs, `n_neighbors=15`, `min_dist=0.1`, Euclidean metric. Pixel-only and parameter sweeps are commented experiments. |
| Embedding comparison, runtime and silhouette | Compare views and quantify label separation cautiously | N04d cells 29–32; demonstrated | `s(i)=(b(i)-a(i))/max(a(i),b(i))`. Here silhouette uses true labels: class separation in a distorted 2D map, not anomaly accuracy or proof of discovered clusters. |
| Pretrained feature representation | Compare raw pixels with learned image features | L6 pp. 41–44; N06a cells 24–35; demonstrated | ResNet152 without classification head plus global average pooling yields 2,048 features; compare on the same samples. Specific to images. |
| Original/reconstructed/error views | Identify what information is lost by a representation | N05b/c; N06a cells 35–36, 51–52; demonstrated | Use comparable color scales. A 32×64 reshaping of a 2,048-feature vector is a feature-index visualization, not spatial pixel localization. |
| Per-variable residual analysis | Direction, magnitude and persistence of deviations | N07a cells 55–62; N07b cells 39–46, 60–63; demonstrated | Inspect signed mean and median per cycle, plus magnitude. Reconstruction can spread one abnormal input's error across several outputs. |
| Sensitivity analysis and ensemble dispersion | Whether conclusions depend on arbitrary parameters | N03b gamma contours; N04a cells 18–21; demonstrated | LOF sweep produces mean, median and std per sample. Dispersion measures parameter disagreement, not calibrated uncertainty. |
| Case-based error analysis | Understand failure modes behind aggregate metrics | L6 p. 67; N06b cells 40–50; demonstrated | Inspect wrong high scores, wrong low scores, high-confidence mistakes and low-confidence correct cases; check raw observations and context. |

### Practical additions for this network project

These are applications/extensions of the course principles, not methods claimed as demonstrated in the supplied classroom notebooks. The combined network dataset has now been inspected in [01-exploration.ipynb](../01-exploration.ipynb), using the domain definitions in [DOMAIN_NOTES.md](DOMAIN_NOTES.md). The exploration uses both source partitions together, as requested, and makes no predictive generalization claim.

- Audit dtypes, missing/nonfinite values, duplicate rows, constant/nearly constant columns, impossible values, categorical cardinality, units and identifiers. Do not silently impute, drop or deduplicate anomaly cases.
- Preserve nominal categories as categories. If a later method needs numbers, choose an explicit encoding; arbitrary integer codes create artificial distances. Image target one-hot encoding in N06b is not a worked example of categorical traffic-feature encoding.
- For skewed nonnegative counts/rates, compare original histograms with a log axis or `log1p` view if justified; this is an extension, not a demonstrated notebook step. Check zero and negative values first.
- Compare numeric distributions within connection states/protocols and across acquisition periods or hosts, if such fields actually exist. Record which fields are available before proposing temporal or host splits.
- Keep a compact evidence log: observation → plot/statistic → possible explanation → next check. Distinguish rare legitimate traffic from attacks.

## 3. Statistical scoring and threshold toolkit

Let `r = y - y_hat`, `m = median(r)`, and `s(x)` be oriented so larger values mean more anomalous.

| Method | Rule / use | Source and status | Main limitation |
|---|---|---|---|
| Gaussian / z-score threshold | `z=(x-mu)/sigma`; flag a selected upper/lower tail or `abs(z)>k` | L1 pp. 21–25; L3 p. 12; N01; demonstrated | Three-sigma coverage assumes an appropriate normal model; skew and contamination can invalidate it. |
| Gaussian tail probability | Upper `1-F(x)`, lower `F(x)`, two-sided `2*min(F(x),1-F(x))`; compare with alpha | L1 pp. 24–25; theory | A p-value under normality is not the probability of an attack. Estimated distribution and repeated testing matter. |
| Median/MAD threshold | `median(x) ± k*1.4826*MAD(x)` | L1 p. 52; N01 cell 51; N01e cell 74; demonstrated | k=2 or 3 are examples. MAD can be zero for discrete/constant data; choose a documented fallback. |
| Quantile-spread threshold | Median ± multiplier × reference quantile span | N03b cells 53–56; N04a cells 26–30; N04b cells 20–23; demonstrated | N04a uses Q95−Q05, not the conventional IQR. Multipliers are heuristics, not equivalent false-alarm guarantees. |
| Empirical percentile | Choose a high normal-reference score quantile | N05a cell 22 (95%); N07a cells 39, 47 (99%); demonstrated | Use calibration data at the same aggregation/smoothing level as deployment scores; empirical coverage can change under drift. |
| Histogram-tail geometry | Find the start of a low-frequency tail | N05a cells 24–26, 34–36; demonstrated | The 1% bin-mass cutoff and bin count are arbitrary. Empty bins or multimodality can mislead; fitting it on test data is exploratory. |
| Triangle histogram threshold | Geometric selection of a histogram cutoff | N05a cell 81; suggested only | The exercise asks students to study it; no supplied implementation. |
| Labeled operational threshold | Sweep cutoffs, inspect precision/recall, choose acceptable missed-detection/false-alarm tradeoff | L2 pp. 53–54; N02a cells 32–43; demonstrated | Prefer validation labels. Perfect recall on development data does not imply perfect future recall. |
| Residual magnitude / reconstruction error | Signed r for one-sided deviations; `abs(r-m)` for two-sided; per-sample `mean((x-x_hat)**2)` for reconstruction | N01; N05a/b/c; N07a/b; demonstrated | Signed residual ranking can miss negative anomalies. Mean squared error can dilute one failing variable among many. |
| Alternative multivariate scores | Maximum normalized variable error; count of variable threshold exceedances | L5 p. 14; N07a cell 59; suggested | Define reference normalization and multiple-variable false-alarm behavior before use; these alternatives are not implemented in N07a. |

### Score direction convention

Store the raw score definition and its direction alongside every result. Scores are not automatically probabilities.

| Output | Direction / conversion |
|---|---|
| Supervised `predict_proba` for anomaly class | Higher means more likely anomalous; locate the class through `classes_`, rather than assuming its array position. |
| Supervised SVC `decision_function` | Sign refers to the encoded class ordering; an uncalibrated margin score, not a probability or generally a literal geometric distance. |
| OCSVM / IsolationForest `decision_function` | Lower means more abnormal; negate for the common higher-is-anomaly convention. |
| LOF `negative_outlier_factor_` | Negate it: LOF near 1 is typical; larger positive LOF indicates lower relative density. |
| OCSVM / IF / LOF binary output | Usually +1 normal and −1 abnormal; explicitly convert to 0 normal / 1 anomaly for evaluation. |
| DBSCAN labels | −1 means noise; nonnegative integers are cluster IDs, not anomaly severity. |
| Reconstruction MSE / absolute residual | Larger means greater deviation; neither is an anomaly probability. |

## 4. Detector and model inventory — secondary at this stage

| Method | Principle and when it is relevant | Source / coverage | Parameters, assumptions and useful diagnostics |
|---|---|---|---|
| Ordinary least squares regression | Explain a response by context; inspect deviations from the expected relationship | L1 pp. 31–36; N01 cells 25–28; N07b cells 33–46; demonstrated | Linear relationship; outliers influence squared loss. Plot fit, residuals and dependence on conditions. Includes multiple-input/multiple-output regression. |
| Huber robust regression | Quadratic loss for small residuals, linear growth for large residuals | L1 pp. 53–55; N01 cells 29–39; demonstrated | More resistant to large residuals; epsilon controls downweighting. Not a guarantee against leverage points or arbitrary contamination. |
| RANSAC | Repeated small fits, select a consensus of inliers | L1 p. 56; theory | Depends on sampling, inlier threshold and sufficient consensus. Dedicated exercise notebook cited in slides is absent. |
| Theil–Sen regression | Median of pairwise slopes in the simple univariate formulation | L1 p. 56; theory | Robust slope estimation; computational cost grows, and multivariate implementations generalize the simple formulation. |
| Logistic regression | Supervised anomaly-class probability baseline | L2 pp. 31–39; N02a; demonstrated | Needs both classes; imbalance affects useful cutoff. Analyze scores rather than accepting 0.5 automatically. |
| Support vector classification | Maximum-margin separation; soft margins and kernels | L2 pp. 40–52; N02c; demonstrated | C trades margin against violations; kernels include linear, polynomial, RBF. Solution notebook fits linear SVC; broader kernels are lecture exercises. |
| Mahalanobis / Hotelling-style T² | Covariance-aware squared distance `(x-mu)^T Sigma^-1 (x-mu)` | L3 pp. 10–17; theory; L5 p. 28 latent-space extension | Appropriate for roughly elliptical unimodal normal data. Difficult covariance estimation in high dimension; contamination and singularity need attention. Formal T² thresholds depend on the statistic and sampling setting. |
| DBSCAN | Density-connected clusters; noise as candidate outliers | L3 pp. 18–24; N03a; demonstrated | `eps`, `min_samples`; classroom starting heuristic MinPts≈2d and sorted k-distance knee. Variable density/high dimension are difficult; no native out-of-sample `predict`. |
| k-nearest-neighbor distance diagnostic | Large kth-neighbor distance marks sparsity; knee suggests epsilon | N03a cells 16–20; demonstrated | Explicitly excludes self when computing kth other neighbor. The distance heuristic can itself flag candidates before clustering. |
| One-Class SVM | Learn a boundary around the predominant/normal region | L3 pp. 25–30; N03b; demonstrated | RBF gamma controls local flexibility; nu controls training error/support-vector bounds. Median-distance gamma heuristic and boundary contour sweep demonstrated. |
| Support Vector Data Description | Enclose most data in a small feature-space sphere | L3 p. 29; theory | Related boundary formulation; no supplied implementation. |
| Local Outlier Factor | Compare local reachability density with neighbors' density | L4 pp. 7–14; N04a; demonstrated | `n_neighbors` controls locality. Contamination selects cutoff, not raw training LOF values. `novelty=True` is required for the novelty API on unseen observations. |
| LOF parameter ensemble | Aggregate score mean/median/std across neighborhood sizes | N04a cells 18–30; demonstrated | Notebook tries k=10 through 150 (selected values) and thresholds dispersion. Strong parameter disagreement calls for investigation, not automatic trust. |
| Isolation Forest | Random splits isolate unusual observations in fewer steps | L4 pp. 15–20; N04b; demonstrated | Trees, per-tree samples, feature subsampling, contamination and seed. Compare one versus two operating states; inspect scores rather than only binary predictions. |
| PCA reconstruction | Reconstruct from a normal-data linear subspace; error identifies deviations | L5 pp. 7–29; N05a/b; N07a; demonstrated | Number of PCs balances compression with normal reconstruction. Anomalies inside the retained subspace can have small reconstruction errors; latent T² is complementary. |
| Dense autoencoder | Nonlinear bottleneck learns normal reconstruction | L5 pp. 30–51; N05c; N06a; N07a; demonstrated | Bottleneck, depth, regularization and epochs. Good healthy reconstruction alone does not establish good detection; can reconstruct anomalies well. |
| Feedforward regression NN | Predict sensor responses from operating conditions | N07b cells 47–63; demonstrated | Small ReLU network, linear outputs, Adam, MSE, early stopping. Signed per-sensor residuals give more directed interpretation than reconstructing every input. |
| Pretrained ResNet152 + PCA / AE | Reconstruction detection in extracted image-feature space | L6 pp. 41–44; N06a; demonstrated | Correct grayscale-to-RGB conversion, resizing and ResNet preprocessing; pretrained features can help or hurt depending on task. |
| Random forest classifier | Supervised baseline on flattened images | L6 p. 46; N06b cell 7; demonstrated | Provides a simple comparison before CNNs. Classification accuracy here is separate from imbalanced anomaly evaluation. |
| Convolutional neural network classifier | Learn local image patterns through shared filters and pooling | L6 pp. 18–40, 45–47; N06b; demonstrated | Convolution, padding, ReLU, max pooling, flattening, dense/softmax head, receptive fields. These examples are 2D image CNNs. |
| Other named baselines | k-NN classifier, classification trees, XGBoost, classification NNs; k-means as an unsupervised-learning example | L2 pp. 25, 30, 52; mentioned | Named in the slides, without substantive worked implementations in the supplied notebooks (apart from N06b RF/CNN). |

### Training techniques retained for later work

L5 pp. 31–46 cover weighted sums, activations, dense layers, cross-entropy/MSE loss, gradient descent and backpropagation. N05c/N07a/b demonstrate Adam-based fitting. L6 pp. 48–66 and N06b cover the following development techniques; they are supporting techniques, not the immediate analysis plan.

| Technique | Purpose / diagnostic |
|---|---|
| Training and validation learning curves | Diagnose convergence, capacity limitations and overfitting; compare loss/accuracy under comparable conditions. |
| Early stopping | Monitor validation loss and restore the best weights; implemented in N07a/b. |
| Dropout | Randomly mask activations during training; N06b primarily uses dense-layer dropout. |
| Image augmentation | Horizontal flips, zero padding and random crops; training data only; visualize transformations to verify labels remain meaningful. |
| Wider/deeper architectures | Increase filters/layers systematically; compare capacity changes with validation performance and residual/error cases. |
| Batch normalization | Normalize intermediate activations with learned rescaling; assess optimization and generalization separately. |
| L2 regularization / weight decay idea | Penalize large weights; N06b uses Keras `kernel_regularizer=l2(...)`. This is not generally identical to decoupled optimizer weight decay. |
| Efficient batching | N06b uses `tf.data` shuffle, batch, map and prefetch; computational technique, not evidence of improved anomaly detection. |

## 5. Evaluation as an analysis tool

Sources: L1 pp. 38–48; L2 pp. 37–39; N01e cells 71–80; N02a/c; N03b/N04b; N07a cells 50–52; N06b cells 40–50.

- **Confusion matrix:** TP, TN, FP, FN at a specified cutoff; specify axes and use anomaly=positive consistently. In sklearn's usual `[0,1]` ordering the matrix is `[[TN, FP], [FN, TP]]`.
- **Recall / sensitivity / TPR:** `TP/(TP+FN)`; **precision:** `TP/(TP+FP)`; **FPR:** `FP/(FP+TN)`; **specificity:** `TN/(TN+FP)`. State handling of undefined denominators.
- **Precision–recall and ROC curves:** sweep score cutoffs rather than relying on one arbitrary threshold. PR is particularly useful when anomalies are rare; inspect anomaly prevalence and the relevant operating region.
- **Average precision and PR area:** N07a uses `average_precision_score`; N01e uses trapezoidal `auc(recall, precision)`. These are different summaries and should be named separately, despite slides' shorthand “AUPRC=AP.”
- **Threshold-versus-metric plots:** show how precision and recall change, and identify a defensible operational cutoff using development/validation data.
- **Label-conditioned log-likelihood:** N02a cell 20 computes `y*log(p)+(1-y)*log(1-p)`. This checks probability assigned to a known label, not a label-free anomaly score. Clip probabilities away from 0 and 1 when computing logs. Logistic fitting maximizes summed log-likelihood, equivalently minimizing binary cross-entropy.
- **Per-class analysis:** confusion patterns and class-conditioned recall in N06b reveal where a global accuracy hides failures. Its so-called “per-class accuracy” is diagonal / row sum, i.e. class recall.
- **Error review:** inspect misclassified examples, high-confidence errors, low-confidence correct examples and per-variable residuals. A high output confidence does not establish calibration.
- **Temporal behavior:** compare score trajectories, onset markers and delayed/smoothed alarms. Fault-detection delay is raised as a N05a exercise; no general event-level evaluation pipeline is supplied.
- **No labels:** stability and domain-informed inspection can assess plausibility; they do not establish detection recall or attack-detection accuracy. Document label limitations rather than invent performance claims.

## 6. Teaching-example caveats to preserve in future implementations

These notes are analysis of the supplied code/text, not additional experimentally established results.

1. **Separate discovery from testing.** L3 p. 6 discusses tuning on scarce test anomalies as a practical limitation. N05a calls all observations “test,” including its training prefix, and estimates histogram-tail cutoffs on those data. Such results are exploratory/in-sample in part; keep a separate holdout for generalization claims.
2. **Fit preprocessing after splitting.** N07a/b fit min–max scaling on all healthy development data before the train/validation split. This excludes test engines but shares validation information. Future implementations should split first, then fit transformations on the training portion. Random row splits within engines/cycles also give correlated validation observations; use grouped/temporal validation for stronger independence.
3. **Do not fit score normalization on the deployment batch.** N05b/c and N06a normalize test scores with a new `MinMaxScaler`. For nonconstant scores an exact monotonic rescaling preserves ranking curves, but makes numeric thresholds batch-dependent. It is unnecessary for sklearn ranking metrics and does not yield probabilities.
4. **Keep score direction and anomaly definition consistent.** N01 examples alternate between signed residual ranking and two-sided thresholds. Decide whether high, low or both deviations matter. N02a comments describe the labeled training interval as fault-free, but logistic regression needs both classes; inspect actual labels rather than trusting that comment.
5. **Distinguish scaling from detection.** Scaling does not make a detector robust. Isolation Forest does not have the same distance-based scaling requirement as LOF, DBSCAN and RBF SVM, although the notebook applies robust scaling to it too.
6. **Quantile names and multipliers matter.** N04a names Q95−Q05 `score_iqr`; N03a's knee heuristic uses Q90−Q10. Neither is the usual IQR. Three different spread measures with the same multiplier do not imply the same threshold.
7. **Treat knee/tail heuristics as fallible.** N03a's custom difference threshold can fail if no knee is found; also account for sklearn DBSCAN counting the point itself in `min_samples`, while the helper excludes self from kth-neighbor distances. N05a's histogram heuristic is bin-sensitive; the triangle method is only suggested.
8. **Handle time-window boundaries explicitly.** N05a correctly trims endpoint labels by M−1 for its windowed comparison. Do not let windows cross entity boundaries or leak shared raw observations across splits. Fit thresholds for the same smoothed/aggregated statistic that will be deployed; the example sometimes reuses an unsmoothed cutoff.
9. **Freeze the threshold used in every output.** N02a's final mask multiplies the selected threshold by 1.2; some N05a geometric-threshold plots draw `threshold` instead of `thresh_tail`. Compare the actual detection mask and displayed line before reuse.
10. **Compute actual mixture prevalence.** N05b/c define `n_anomalies = int(anomaly_ratio * n_normal)`. Thus the final fraction is approximately `ratio/(1+ratio)`; ratio 0.3 yields about 23.1%, not 30%. For target fraction p, use approximately `n_anomaly = p*n_normal/(1-p)` and report realized counts.
11. **Reconstruction attribution is limited.** N07a's largest variable errors need not identify the originating fault; signed errors can cancel under averaging. Comparing regression outputs conditioned on inputs helps interpretation but still does not establish a causal diagnosis.
12. **Feature maps need honest labels.** N06a reshapes pooled feature-vector residuals into 32×64 arrays. Their adjacency has no demonstrated spatial meaning. Its markdown discusses t-SNE, but the supplied N06a code does not compute a t-SNE embedding; the worked embedding is N04d.
13. **Some notebook cells are teaching fragments.** The image class-name cells contain bare text; N02b is a short exercise scaffold; N06a relies on earlier sampled indices and variables. These are references to adapt, not guaranteed clean-kernel executable modules.
14. **Resolve slide simplifications before production coding.** L4 p. 20's IF `max_samples='auto'` text says max(256,n), whereas the intended sklearn rule is min(256,n); use installed-version behavior. Its `decision_function = score_samples+0.5` corresponds to automatic contamination; in general subtract `offset_`. L6's dropout scaling discussion uses p=0.5, which hides the distinction between drop and keep probabilities: inverted dropout scales retained activations by `1/(1-drop_rate)`. L1's suggestion to raise Huber epsilon with more outliers should not become a rule: increasing epsilon delays downweighting.
15. **Validate claimed improvements.** More explained variance, lower healthy MSE, visually tighter embeddings, pretrained features, more layers or regularization do not guarantee better anomaly detection. Preserve case-level evidence and compare under the same task and sample selection.

## 7. Applying the toolkit to network intrusion exploration

L2 physical PDF p. 9 describes **UNSW-NB15, normal traffic versus DoS**, with illustrative variables `dur`, `dpkts`, `dbytes`, `rate`, and categorical `state`. These are the assignment description and illustrative columns; the actual data/schema must still be loaded and verified. Do not silently broaden the target to every attack category.

The first analysis deliverable should contain:

| Question | Evidence to produce before fitting a detector |
|---|---|
| What exactly is being detected? | Define a connection/flow record, normal versus DoS, label availability and false-alarm/missed-attack tradeoff. |
| What is in the data? | Schema and feature-role table, dimensions, class/state counts, quality audit, original split/source information. |
| Are attacks different in individual variables? | Comparable histograms and robust summaries for duration, counts, bytes and rates, once actual names/units are verified. |
| Are unusual values explained by context? | Selected scatterplots/pairplots and state-conditioned distributions; investigate rare legitimate groups. |
| What multivariate structure exists? | Scaled PCA with explained-variance curve and loadings, followed by sampled t-SNE/UMAP views if useful. |
| Are plots representative? | Document sampling and class proportions; show both population-shaped and class-balanced views where informative. |
| What can we evaluate honestly? | Explicit feature/label separation, validation/test roles, and time/entity independence if metadata supports it. |
| What should be tried later? | Choose a small justified baseline set from statistical scores, LOF, IF, OCSVM or PCA residuals; add supervised baselines only for a labeled setting. |

Do not implement the whole inventory automatically. Use the observed data geometry, context and task definition to choose the next analysis.
