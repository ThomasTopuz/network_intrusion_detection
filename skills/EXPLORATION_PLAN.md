# Data exploration: current approach

The notebook [01-exploration.ipynb](../01-exploration.ipynb) now follows the simpler approach requested by the user. It uses the combined [normal_dos.csv](../content/normal_dos.csv), with no model fitting or training/testing split.

## Class examples to follow

- Statistical temperature notebook 01b: look at the measurements, draw histograms, zoom in, and compare variables in a small pairplot.
- Engine notebook 07a: understand raw variables and their meaning before changing them; sample rows when a plot has too many points.
- Single/two-state machine examples: compare similar operating conditions. Here that means considering protocol, service and state.

## Notebook steps

1. Explain flows, packets, directions, DoS and the security team's problem.
2. Check column types, missing values, zeros, repeated measurements and label counts.
3. Explain category names; compare protocol, service and state using simple bar charts and counts.
4. Compare numerical measurements using means, medians, quartiles and histograms. To show their very wide ranges, the full-range histograms use a logarithmic horizontal axis with actual-value labels and separately reported zero counts. Follow these with ordinary linear close-ups and report the percentage shown.
5. Draw simple scatterplots and a two-variable pairplot. Label axes with actual packet counts rather than transformed coordinates. The requested correlation section means feature-against-feature scatterplots, not heatmaps: show six useful pairs, color Normal and DoS differently, and explain any zooms.
6. Compare packet rates within TCP and UDP, not only across the whole dataset.
7. Calculate two easy-to-explain summaries: return packet share and source bytes per packet. Handle zero denominators explicitly.
8. Look at example rows and finish by defining a flow-level anomaly-detection task, with its limits.

## Writing and code

Use short explanations headed by questions such as “What do we see?” and “What does this mean?”. Use plain terms such as packets sent, packets returned, average, and median. Keep important limits without repeating them in every section.

Use direct pandas and matplotlib operations. No custom analysis framework, cumulative-distribution plots, density maps, correlation heatmaps, log-transformed coordinate labels or run-manifest code is needed for this version. A logarithmic horizontal axis is used specifically for the wide-range numerical histograms, with a short reading guide and zero counts.

The current figures are saved in `../content/exploration/simple_figures/`. Earlier exports in the other exploration subfolders belong to the previous version; the current notebook is the reference for this simpler analysis.
