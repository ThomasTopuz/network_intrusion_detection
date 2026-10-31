# Course context for future work

Read `COURSE_TOOLKIT.md` before planning analysis in this workspace. It records the methods in the supplied anomaly-detection lectures and notebooks, their assumptions, and implementation caveats. Use `COURSE_SOURCES.md` to locate original examples.

The user's current priority is data analysis and exploration; model fitting is secondary. Start with task definition, raw-data inspection, distributions, contextual relationships, operating regimes, and split design. Ground choices in the supplied course material. Clearly label additional methods as extensions.

Keep labels, metadata, and identifiers separate from predictor features. Distinguish exploratory use of labels from training and evaluation. Preserve a held-out test set when reporting generalization. Do not automatically remove unusual observations: they may be the events of interest.

These files are persistent project context, not a claim of permanent model training. Update the toolkit when additional course material is supplied.

Store all new project Markdown notes, plans, domain explanations and guidance in `skills/`, as requested by the user. Resolve paths in these notes relative to this folder.

For the current exploration phase, use the combined `content/normal_dos.csv` (relative to the project root); no train/test split is required at this stage. Model fitting remains outside the current scope.

Keep the notebook close to the class examples: direct pandas and matplotlib code, small plots, clear feature names and short explanations in everyday language. Prefer original units and clearly explained zooms. Do not add advanced plots, transformed coordinates, custom helper layers or lengthy technical explanations unless they answer a necessary question and the user wants them.

When the user asks for plots of correlations between features, use feature-against-feature scatterplots, with Normal and DoS colors. The user explicitly rejected heatmaps for this request.
