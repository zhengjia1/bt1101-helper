You are designing a Cyberchef-style website for student to enter in relevant RStudio outputs and generate the corresponding prose format. The website MUST consist of ONE single, standalone file. All computation must be done locally, on the user's computer, without any external API calls. When parsing the results, you must allow some level of flexibility when a user's pastes extra/missing spaces (up to 3).



Separate each tutorial into its own collapsible, with the tests below. Within each test, you must also provide a sample output for the user to reference and cross check - the variables may be dummy variables. Also implement a search function for the user to search for the relevant test.



You must also have a "universal" search to intelligently identify the test that the code belongs to and generate the correct output. This universal search should be the 1st result.



Guidelines for Rounding Off

Rounding off in your working steps may reduce accuracy of your final answers. So limit rounding off

to the final answers.

If the answer is greater than 1, round off to 2 decimal places.

If the answer is less than 1, round off to 3 significant numbers.

When rounding, also take note of the natural rounding points, for example, costs in dollars would

round off to 2 decimal places.

For descriptive statistics tables, unless otherwise told, you can decide whether to round off to 2 or 3

decimal places, depending on the value of the numbers in the table



Tests

Shapiro-Wilk

Pearson Correlation Test (cor.test)

Corr.test (for multiple) 



Tutorial 4 - Statistical Measures \& Probability Distributions 

Summarize for stats - fill in variable name

Describe coefficient of skewness, kurtosis, correlation

Wide table format



Tutorial 5 - Statistical Inference

Calculate confidence/prediction interval + template

1-Sample: T/Z test

2-Samples: T Test, Var Test

>2 samples: Anova procedure



Tutorial 6 - Linear Regression

lm(), predict() + template

Statistical significance of model

Pairwise model comparison with anova

Dummy variables/Reference Groups

Interaction terms

GLM/Binary variable

Confusion matrix



Tutorial 7 - Data Mining

PCA (and significance of each)

Forward/backward stepwise model selection

K means clustering

K Fold Crosss Validation



Tutorial 8 - Linear Optimization

lpSolve

Sensitivity Analysis



Others

Ggplot
## Current implementation summary

`index.html` is the complete BT1101 Prose Lab application. It is intentionally a single, standalone HTML file containing all markup, CSS, JavaScript, and embedded Source Sans Pro font data. It performs every calculation and parse locally in the browser, makes no network or API requests, and does not require a server or build step.

### Application structure

- The interface follows a CyberChef-style recipe/workbench layout.
- The left recipe library is divided into collapsible tutorial groups and can be searched by test name, topic, or R function.
- Universal Detector is always the first operation. It examines pasted R output/code and routes recognised content to the appropriate local parser.
- The recipe sidebar is resizable by dragging its divider. The divider also supports Left/Right arrows, Shift for larger steps, Home/End, and double-click reset. Its width is stored in local browser storage. On small screens, the sidebar becomes the existing collapsible mobile library.
- Relevant operations include sample inputs or sample configurations, automatic regeneration when fields change, and copy buttons for generated prose or R code.
- Generated prose boxes grow according to their content. Parsing is designed to tolerate ordinary whitespace variation, including up to three added or missing spaces around expected output tokens.

### Implemented operations

- Core tests: Shapiro-Wilk, Pearson `cor.test()`, and multi-variable `psych::corr.test()`.
- Tutorial 4: single/multiple-column descriptive-statistics code, skewness and kurtosis/excess-kurtosis interpretation, and narrow-to-wide table code.
- Tutorial 5: confidence intervals, prediction intervals, sample-size calculations, one-sample mean/proportion tests, two-sample mean/variance tests, and ANOVA/Welch ANOVA workflows.
- Tutorial 6: one-variable and multiple linear regression, prediction, model significance, nested-model comparison, dummy/reference groups, interactions, binomial GLMs, and confusion matrices.
- Tutorial 7: PCA and component significance, forward/backward stepwise selection, K-means clustering, and K-fold cross-validation.
- Tutorial 8: an `lpSolve` model builder, mathematical model table, generated R code, and sensitivity-analysis interpretations.
- Plotting and tables: summarize-then-plot, frequency/bar, pie, contingency, grouped means, histogram, single/grouped boxplots, scatterplots, line plots, and Pareto analysis.

### Important behaviour and validation

- Statistical decisions use the supplied significance level and generate hypotheses, decisions, and context-specific prose.
- Pearson output reports significance separately from correlation direction and strength.
- Final values follow the rounding rules above: values greater than 1 normally use two decimal places; values below 1 use three significant figures; natural units use their appropriate rounding point.
- Interval controls disable combinations that do not apply. Calculations use local JavaScript implementations, including critical-value calculations.
- The lpSolve constraint editor is keyboard-oriented. Constraint rows append automatically as the user types; the trailing blank row is not included in generated code. Each populated row tabs through name, coefficients, operator, RHS, and remove. There is no add button and no custom RHS up/down controls.
- Every populated lpSolve constraint must contain exactly one numeric coefficient per decision variable. Missing variables must be represented by `0`. Mismatched or non-numeric rows are highlighted, an explicit error is announced, and R-code generation/copying is refused until all populated rows are valid. The trailing blank row is ignored.
- The lpSolve smart parser accepts Markdown/ASCII model tables and plain text. It detects min/max direction, multiline objectives, named constraints, parentheses, `X_1`/`X_{1}`/`X1`, Unicode comparison signs, `\le`, `\leq`, `\ge`, `\geq`, and binary/integer declarations. A complete parse populates the manual builder automatically; incomplete input does not overwrite the current model.
- Integer or binary lpSolve models automatically disable sensitivity calculations because `lpSolve` cannot combine those options with `compute.sens`.

### Maintenance constraints

- Keep the production website in `index.html`; do not split CSS, JavaScript, fonts, data, or templates into additional runtime files.
- Do not add CDNs, analytics, remote fonts, external APIs, or dependencies that require network access.
- Preserve placeholder names such as `dataset`, `column1`, `column2`, and so on.
- Preserve keyboard labels, focus behaviour, live error regions, responsive layouts, and the locally saved sidebar preference.
- Any new operation should be added to the searchable recipe catalogue, given a dedicated collapsible/tutorial location, supplied with a realistic sample, and connected to Universal Detector when it has recognisable R syntax.
- After editing, validate the embedded script with `node --check`, confirm there are no duplicate HTML IDs or missing JavaScript ID targets, and test both valid and invalid states for affected generators.

## Conversation handoff — finalized behaviour (23 July 2026)

The following notes summarize the detailed implementation decisions made during the iterative design conversation. They supersede older notes above wherever they conflict.

### Shared interface conventions

- Keep Source Sans Pro throughout the interface. Do not introduce a separate code font or external font request.
- Remove decorative one-line descriptions, route-match banners, character counts, auto-generation labels, duplicated copy controls, and yellow reminder panels unless they provide essential guidance.
- All substantial workflow cards should be collapsible using the disclosure toggle in the card heading.
- Use a plain **Use sample** button when an operation has exactly one sample. A sample dropdown is appropriate only when multiple samples are genuinely available, such as Universal Detector.
- Input and output areas should be sized for the expected content instead of reserving space for dozens of lines. Auto-grow textareas from a compact minimum and cap their height before enabling scrolling. Generated prose should size to its content.
- For ordinary completed cards, copying may mark the card green, add a **Done!** badge, and collapse it. Staged workflows may require both code and prose to be copied before the whole card completes.
- Use large dismissible modal warnings, not fleeting toasts, for choices that can materially change the answer: disabling K-means scaling, selecting a known population standard deviation, copying prose with placeholder variable names, and copying regression prose whose units may need editing.
- Preserve accessible keyboard focus, live error announcements, responsive layouts, the resizable sidebar, and local persistence of its width.

### Universal Detector and navigation

- Universal Detector remains the first recipe and supports a dropdown listing its trained sample types.
- Do not navigate away for self-contained parsers: Shapiro-Wilk, confidence/prediction intervals, Pearson correlation, one-sample mean/proportion, and two-sample mean/variance.
- Navigate directly to the relevant interpretation block for workflows with additional tools: multi-variable linear regression output, binary GLM, confusion matrix interpretation, PCA variance interpretation, K-means, and lpSolve.
- Linear-regression output must route to **Multi-variable linear regression → Interpret regression output**. Confusion-matrix output must route to its interpretation card. PCA importance output must route to **Variance explained by retained components**.
- Stepwise model selection is not a Universal Detector sample/destination.
- Parsers must continue tolerating ordinary console prefixes and spacing variation, including up to three extra or missing spaces around expected tokens.

### Tutorial 4

- **Summarize for stats** uses dynamically appended column-name fields. One populated column generates the single-column descriptive workflow; two or more generate the multi-column `summarise()` table. Toast on both transitions: “Changed to multi-column summary code” and the corresponding return to single-column mode.
- Never derive column fields from unrelated controls elsewhere in the document. Only the dedicated visible column inputs feed the generated summary code.
- Keep the skewness interpretation separate from correlation. Provide a kurtosis/excess-kurtosis toggle and the mesokurtic, platykurtic, and leptokurtic explanations supplied by the user.
- Keep the narrow-to-wide table generator and its matching R code.

### Tutorial 5 intervals and hypothesis tests

- Confidence intervals, prediction intervals, and sample-size calculations share one sidebar operation. Grey out inapplicable combinations of target, parameter type, and standard-deviation status.
- Generate usable R code as soon as dataset name, column/parameter name, and confidence level are available; numeric sample summaries are optional. Accept pasted R results and generate prose from the returned interval.
- Support confidence intervals for a mean with unknown or known population standard deviation, confidence intervals for a proportion (including filtered conditions such as `Amount > 22`), prediction intervals for a mean, and sample-size determination for means/proportions.
- Choosing known standard deviation must show a dismissible confirmation asking whether the user knows the **population** rather than sample standard deviation.
- Retain local one-sample mean/proportion, two-sample mean/variance, and correlation-test parsers and prose.

### ANOVA workflow

- The ANOVA page is a four-stage collapsible workflow whose Step 0 values are the single source of truth for every generated code block and sample output:
  - **Step 0 — Setup:** dataset, response variable, grouping variable, significance level, and a compact normality/large-sample (CLT) toggle. Do not show a yellow explanatory panel beneath it.
  - **Step 1 — Normality:** grouped Shapiro-Wilk code. Disable/grey and collapse this step when the Step 0 normality/CLT toggle is on.
  - **Step 2 — Homogeneity of variance:** ask for the number of groups. For two groups generate `var.test(dataset$response ~ dataset$group)`; for more than two generate `bartlett.test(response ~ group, data = dataset)`. Parse the pasted F-test or Bartlett output and produce hypotheses, decision, and prose. If `p > alpha`, enable/route to Step 3a; if `p < alpha`, enable/route to Step 3b and grey the irrelevant route.
  - **Step 3a — Standard ANOVA + Tukey HSD:** generate only `model <- aov(...)`, `summary(model)`, and `TukeyHSD(model)`. Parse degrees of freedom, F, p, and Tukey rows. State whether the omnibus test is significant and, when applicable, name significant pairs with adjusted p-values or state that Tukey found none.
  - **Step 3b — Welch ANOVA + Games-Howell:** generate only `dataset %>% welch_anova_test(...)` and `dataset %>% games_howell_test(...)`. Parse the Welch result and Games-Howell rows and describe significant pairwise differences.
- Do not include explanatory `#` comments in any generated ANOVA R code.
- Every sample output must substitute the current Step 0 dataset, response, and grouping names. Clicking a sample-output button must never overwrite Step 0.
- Keep ANOVA textareas compact and remove excessive blank lines from generated interpretations.
- Step 2, Step 3a, and Step 3b use staged completion: copying code turns only the code panel green and leaves the card open; after both code and prose have been copied, the whole card turns green, gains **Done!**, and collapses. Editing setup/code/output resets the affected completion state.

### Tutorial 6 regression and classification

- One-variable regression supports no transform, square root, inverse, natural log, and base-10 log.
- Multi-variable regression has collapsible setup, generated-code, and interpretation cards. Its output parser extracts each predictor's estimate, t-value, and p-value; writes the coefficient effect while holding other predictors constant; then gives a separate population-parameter significance decision. Also provide distinct blocks for multiple R-squared and the overall F-test.
- Copying regression prose must display a large dismissible warning that “1 unit” may need replacement with the natural unit such as minutes or dollars.
- Pairwise/nested-model ANOVA parses F and p and names the predictors added by the full model.
- Confusion-matrix interpretation explains sensitivity, specificity, and accuracy with information hovers. Recompute these metrics from the four cells and reject inconsistent reported values with an explicit error.

### Tutorial 7 data mining

- PCA uses collapsible small workflow blocks with individual copy controls and **Copy all**. Preserve the downstream regression/GLM component-significance model.
- PCA interpretation supports individual and cumulative variance toggles, retained-component prose, loading-matrix parsing into human-readable PC equations, and downstream component-significance parsing.
- Stepwise selection is titled **Forward/Backward Stepwise Selection** and has a direction toggle. Forward mode starts from the null model with scope; backward mode starts from the full model while excluding the named columns, then reports adjusted R-squared.
- K-means has a scaling toggle. Disabling it must first show the large warning explaining that scaling should only be disabled for directly comparable variables. When disabled, omit `X_scaled <- scale(X)` and use `X` in every downstream call.

### Tutorial 8 optimization and sensitivity

- Sidebar entry is a single **lpSolve / Sensitivity analysis** operation.
- The model parser accepts Markdown/ASCII/plain-text formulations, multiline objectives, named constraints, parentheses, Unicode/LaTeX comparison signs, variable spellings such as `X_1`, and binary/integer declarations.
- Constraint rows append automatically as the user types. Ignore the trailing blank row, show only a remove control, and do not show custom RHS up/down controls. Provide **Reset all**.
- A populated constraint must contain exactly one numeric coefficient per decision variable. Refuse code/table generation on any mismatch and explain that absent variables require `0`.
- Hide the sensitivity checkbox entirely for all-integer and selected-binary models.
- Put each constraint on its own readable R-code line. Format mathematical expressions without unnecessary `.00` suffixes.
- Provide model-table copying as ASCII. Sensitivity output is split into **Solution**, **Shadow Price**, **Reduced Cost**, and **Slack**, each with its own copy control. The solution prose must state every decision-variable quantity and the objective value.
- Copying solution/prose must show a large dismissible warning to replace `X1`, `X2`, and similar placeholders with meaningful variable names.

### Production verification

- Treat `index.html` as production code: remove temporary data, debug logging, highlighted development text, and obsolete UI branches.
- After every edit, extract the embedded JavaScript and run `node --check`; check for duplicate HTML IDs and JavaScript references to missing IDs; then exercise both success and error states for each changed parser or generator.
