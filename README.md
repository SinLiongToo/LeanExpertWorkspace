# Interactive 8D Problem Solving & Process Capability Simulator

An advanced, interactive browser-based dashboard designed for quality engineering, industrial statistics, manufacturing process optimization, and structured problem-solving. This tool combines the structured **8D (Eight Disciplines) Problem Solving** methodology with interactive statistical simulators (Process Capability $C_{pk}$, Gage R&R, and Hypothesis Testing) and an integrated **Project Management Action Item Tracker (Kanban)**.

Built with a premium slate-dark aesthetic, it leverages **Plotly.js** for high-performance interactive graphing, **MathJax 3** for beautiful LaTeX mathematical typesetting, and runs entirely in the browser with zero external dependencies.

---

## 🚀 Quick Start

1. **Download / Locate** the dashboard files: [`CPKn SIMULATOR.html`](file:///c:/Users/tu-hs/OneDrive/文件/2022_0308_MASA/2022-0708/Projects_antigravity/CPK%20distribution%20simulator/CPKn%20SIMULATOR.html) or [`8d_problem solving.html`](file:///c:/Users/tu-hs/OneDrive/文件/2022_0308_MASA/2022-0708/Projects_antigravity/CPK%20distribution%20simulator/8d_problem%20solving.html). Both contain the exact same fully-integrated application.
2. **Double-click** either file to open it in any modern web browser (Chrome, Edge, Firefox, or Safari).
   *Note: An active internet connection is required to load remote CDN dependencies (Plotly.js and MathJax).*

---

## 📊 Feature Modules & Tabs

### 1. 📋 8D Problem Solving (Structured Workspace)
A comprehensive, step-by-step problem-solving system that guides quality teams through the Ford 8D methodology, fully integrated with project management and statistics:
- **8D Steps (D1-D8)**: Structured text cards for documentation. Steps are context-linked to the statistical tools (e.g. D4 links to Gage R&R/Cpk, D6 to Significance testing, D7 to Process Cpk).
- **👥 Team / Org (D1 Support)**: A structured team management tab. Tracks team members, roles (Leader, Sponsor, Core, Support), departments, contact details, and highlights the designated **DRI (Directly Responsible Individual)** with custom badges. Dynamically summarizes team configuration inside the main D1 wizard card.
- **3×5 Why Analysis Tree**: Redesigned as a horizontal flowchart. Visually maps three parallel failure paths (Physical, Escape, and Management System) from the symptom to the root cause using neon arrows.
- **SVG Fishbone (Ishikawa) Diagram**: A dynamic, interactive fishbone skeleton rendered in SVG. Adding causes dynamically draws ribs and horizontal branches in real-time.
- **Action Tracker (Kanban Board)**: An interactive project management board:
  - Columns: **To Do**, **In Progress**, and **Done**.
  - Metric Indicators: Total Action Items, Completion Rate (%), and Overdue Items.
  - Overdue Detection: Highlights tasks with a pulsing red **[OVERDUE]** badge if the target date is in the past and the task is not marked Done.
- **Failure Distribution**: A Plotly-backed bar chart that counts causes in each of the 6M categories (People, Machine, Method, Material, Measurement, Environment).
- **FMEA Risk Assessment**: Guided AIAG-VDA Failure Mode and Effects Analysis panel. Integrates Severity (S), Occurrence (O), and Detection (D) ratings to calculate modern Action Priority (AP: HIGH, MEDIUM, LOW) rankings. Features a compact item deletion system and is fully serialized into the JSON project files.

### 2. 📈 Process Cpk (Capability Analysis)
Evaluate process capability indices ($C_{pk}$) under various distribution models:
- **Supported Distributions**: Normal (Gaussian), Log-Normal (heavy right-skewness), Weibull, Uniform, and Bimodal mixtures.
- **Parametric Standard Cpk**: Assumes normality; calculated using sample mean ($\bar{x}$) and sample standard deviation ($\sigma$).
- **ISO 22514-2 Percentile Cpk**: Robust, distribution-free capability calculation using actual percentiles ($X_{0.135\%}$, Median $X_{50\%}$, and $X_{99.865\%}$) to accurately represent defect rates for skewed/non-normal processes.
- **Dual Visualizations**: Interactive Probability Density Function (PDF) histogram and Cumulative Distribution Function (CDF) curve with percentile markings.

### 3. ⚖️ Distribution Comparison
Directly compare a baseline process (**Dataset A** / Before improvement) against a trial/optimized process (**Dataset B** / After improvement) in a 2x2 grid:
- Analyze the centering shift (**Delta Mean / Median**).
- Analyze the change in dispersion (**Sigma Ratio** $\sigma_B / \sigma_A$).
- Multi-dimensional overlays: overlaid histograms, overlaid CDFs, side-by-side boxplots, and **Quantile-Quantile (Q-Q) plots** (used to diagnose subtle location, scale, and shape/skewness changes between A and B).

### 4. 📊 Statistical Significance Tests
Confirm if observed process differences between Dataset A and Dataset B are statistically significant or merely random sampling noise based on sample sizes ($N_A, N_B$):
- **Welch's t-Test**: Analyzes the shift in means ($\Delta$) without assuming equal variances. Computes the $t$-statistic, Welch-Satterthwaite degrees of freedom, p-value, and 95% Confidence Interval for Delta.
- **Variance F-Test**: Compares variances to confirm the significance of the Sigma Ratio. Computes the $F$-statistic, p-value, and 95% Confidence Interval.
- **Combined Process Diagnostic Conclusion**: A smart diagnostic engine that synthesizes t-test and F-test results into four actionable engineering scenarios (Center Shift only, Variability Shift only, Both shifted, or No significant change) with troubleshooting recommendations.

### 5. 📏 Gage R&R / MSA (Measurement System Capability - MSC)
A dedicated module to simulate and evaluate the quality and trustworthiness of your measurement system:
- **ANOVA Method**: Calculates two-way ANOVA with operator-part interaction.
- **Variance Components Breakdown**: Displays variance contribution (%Contribution), standard deviation, study variation (%StudyVar), and tolerance consumption (%Tolerance).
- **AIAG Compliance Cards**: Categorizes the gage as *Acceptable*, *Marginal*, or *Unacceptable* based on %StudyVar and number of distinct categories ($ndc$).
- **Interactive Visualizations**: Gage component variance comparison bar chart and Operator-by-Part run chart.

### 6. ⏳ Device Drift & Shift
Evaluate parametric stability and repeatability of devices across multiple stress loops or runs:
- **Interactive Drift Simulator**: Customize device offset variation ($\sigma_{\text{device}}$), repeatability noise ($\sigma_{\text{repeatability}}$), systematic intrinsic drift ($\beta$ per run), sudden stress shift (DSA jump), number of devices (ECIDs), and run count ($R$).
- **Visualizations**: Run Chart tracking device-specific measurements across runs with USL/LSL lines, and Boxplot Chart summarizing distribution spread per run.
- **Repeatability %Tolerance**: Computes measurement error relative to tolerance using two-way random-effects ANOVA:
  $$\text{Repeatability \%Tolerance} = \frac{6 \sigma_{\text{repeatability}}}{USL - LSL} \times 100\%$$
- **NPI Repeatability Metrics**: Real-time evaluation of median-normalized repeatability metrics:
  1. **SD / Median Ratio**: $\left|\frac{SD}{P_{50}}\right| \times 100\%$ (Excellent $\le 2\%$, Acceptable $\le 5\%$, Poor $> 5\%$)
  2. **Range / Median Ratio**: $\left|\frac{\text{Max} - \text{Min}}{P_{50}}\right| \times 100\%$ (Excellent $\le 5\%$, Acceptable $\le 10\%$, Poor $> 10\%$)
  3. **Robust Sigma / Median Ratio**: $\left|\frac{1.4826 \times \text{MAD}}{P_{50}}\right| \times 100\%$ (Excellent $\le 2\%$, Acceptable $\le 5\%$, Poor $> 5\%$)
  4. **Drift Repeatability (Rpt)**: $Rpt = 3 \times \sigma_{\text{drift}}$ (Excellent $\le 0.5$, Acceptable $\le 1.0$, Poor $> 1.0$)
- **Device Qualification Status**: Evaluates whether a device qualifies based on repeatability and statistical significance.

### 7. 📊 Quality Statistics Hub
An independent top-level tab providing tools for sample size estimation and mathematical significance tests in sub-PPM defect rate environments:
- **PPM Sample Size Tool (Poisson)**: Calculate the required sample size ($n$) to verify a target PPM boundary with a specified confidence level (90%/95%/99%) under a given number of allowed defects ($c \le 2$).
- **Chi-Square & Realized CL Display**: Compares Group A and Group B defect counts to compute the expected counts, $\chi^2$ statistic, $p$-value, and realized Confidence Level. Automatically flags calculations if expected counts fall below the mathematical limit of 5.0.
- **TTR Run Planner (Inverse Chi-Square)**: Automatically predicts the minimum sample size ($n_B$) required for a validation run to achieve statistical significance given a baseline Group A performance.

### 8. 📋 Project Status Report
An executive status reporting utility integrated into the dashboard to summarize engineering developments:
- Milestones and corrective action tracking tables with status drop-downs.
- Executive summary fields including Manager, Objective, Business Impact, and Highlights.
- Local storage persistence and independent JSON export/import.

### 9. 💎 Wafer Yield Calculator
A dedicated semiconductor manufacturing calculator to estimate silicon wafer productivity:
- **DPW & GPPW Estimation**: Calculates Dies Per Wafer (DPW) and Good Parts Per Wafer (GPPW) based on wafer diameter, scribe lane width, edge exclusion margin, and die sizes.
- **Yield Modeling**: Compares standard Poisson, Murphy, and Negative Binomial (industry-standard defect clustering) yield models.
- **Interactive Defect Cluster Visualization**: Includes a 2D canvas curve graph of yield vs. die area, along with procedural mini wafer defect maps representing low and high clustering ($\alpha$) factors.

---

## 📐 Mathematical Reference for MSC (Gage R&R)

### 1. Variance Decomposition (Two-Way ANOVA with Interaction)
The total observed measurement variation ($\sigma^2_{\text{Total}}$) is decomposed into true Part-to-Part variation ($\sigma^2_{\text{PV}}$) and Measurement System variation ($\sigma^2_{\text{GRR}}$):

$$\sigma^2_{\text{Total}} = \sigma^2_{\text{PV}} + \sigma^2_{\text{GRR}}$$

$$\sigma^2_{\text{GRR}} = \sigma^2_{\text{Repeatability}} + \sigma^2_{\text{Reproducibility}}$$

$$\sigma^2_{\text{Reproducibility}} = \sigma^2_{\text{Operator}} + \sigma^2_{\text{Operator} \times \text{Part Interaction}}$$

### 2. Gage Assessment Metrics
- **%Study Var (%SV)**: The proportion of total process standard deviation consumed by gage error.
  $$\%SV = \frac{\sigma_{\text{GRR}}}{\sigma_{\text{Total}}} \times 100\%$$
- **%Tolerance**: The proportion of specification width ($USL - LSL$) consumed by the $6\sigma$ gage variation.
  $$\%Tolerance = \frac{6\sigma_{\text{GRR}}}{USL - LSL} \times 100\%$$
- **Number of Distinct Categories (ndc)**: Measures the resolution of the gage.
  $$ndc = 1.41 \cdot \frac{\sigma_{\text{PV}}}{\sigma_{\text{GRR}}}$$

---

## 🎨 Technology Stack
- **Structure & Styling**: HTML5, CSS3 (premium slate-dark palette, responsive layouts, **Dark / Light theme toggle**).
- **Interactive Charting**: Plotly.js (WebGL-backed rendering).
- **Typesetting**: MathJax 3 (LaTeX parser).
- **Statistical Engine**: Pure JavaScript (non-parametric percentiles, CDF calculations, t-distribution and F-distribution solvers, and numerical solvers for confidence intervals).

---

## ❓ Frequently Asked Questions (FAQ)

### Q1: Why are Welch's t-Test and F-Test conducted separately?
- **Welch's t-Test** checks for center shift (mean difference $\Delta$) between before/after processes.
- **F-Test** checks for dispersion shift (variance ratio $\sigma_B / \sigma_A$).
- Combining both tests helps engineers diagnose whether process capability ($C_{pk}$) changed due to center correction, variability reduction, or both.

### Q2: What is the relationship between Gage R&R and Process Capability ($C_{pk}$)?
- **Gage R&R (MSC)** measures measurement error. **Process Capability ($C_{pk}$)** measures manufacturing error.
- You must verify that the measurement system is capable (Gage R&R %StudyVar $< 30\%$ and $ndc \ge 5$) *before* trusting $C_{pk}$ calculations. A poor gage bloats observed variation and artificially degrades calculated capability.

### Q3: How can statistical validation prevent "pseudo-improvements" in the 8D workflow?
- During **D6 (Verification of Corrective Actions)**, changes in average metrics can be caused by random sampling noise rather than true improvement.
- Conducting a **Welch's t-Test** and **F-Test** provides mathematical proof ($p < 0.05$) that the process mean shifted or variance decreased, ensuring corrective actions had a genuine, statistically significant impact.

### Q4: How does the Kanban board integrate with 8D target dates?
- The **Kanban Board** organizes D3 (containment) and D5/D6 (corrective) actions.
- In quality management, keeping containment and resolution on schedule is vital to prevent customer escapes. The board highlights overdue tasks with a pulsing red `🚨 OVERDUE` badge to enforce the 30-day full closure cycle.

### Q5: What is the difference between a CDF plot and a Q-Q plot in process diagnostics?
- **CDF Plot ($x \to p$):** Shows cumulative probability. Answers *"How much probability accumulates up to each value?"* It is highly intuitive for comparing specification exceedance rates (defect rates) and viewing the shape relative to USL/LSL. However, subtle differences in the distribution tails or shape are hard to detect because the CDF lines lie very close together in tail regions.
- **Q-Q Plot ($p \to x$):** Shows quantile differences. Answers *"How do the corresponding quantiles differ?"* By plotting baseline quantiles vs. trial quantiles at identical percentiles, it magnifies differences in shape, scale, and skewness. Identical distributions form a perfect 45-degree line ($y=x$), location shifts translate to parallel offsets, scale differences alter the slope, and skewness differences bend the line into a curve.

---

## 📝 Optimization Log (優化日誌)

### 2026-06-23 (Unified 8D & Process Capability Simulator Upgrade)
- **Merged 8D & CPK Modules**: Combined the primitive 8D problem solving HTML tool and the advanced CPKn simulator into a single, cohesive, premium dashboard. Enabled seamless navigation for teaching quality engineering.
- **Added Project Management Tools (Kanban)**: Implemented a full-featured Action Item Kanban Board inside the 8D workspace. Tasks support fields for Title, 8D Phase (D3, D5, D7), Assignee, Target Date, and Priority. Included real-time dashboard analytics (Total items, Completion rate, Overdue count).
- **Overdue Task Alert Engine**: Implemented an automated date-checking script that flags pending tasks with a flashing red `🚨 OVERDUE` badge if their target date is older than the current date.
- **Dynamic SVG Fishbone (Ishikawa)**: Developed an interactive SVG fishbone diagram that renders causes dynamically along diagonal 6M ribs (People, Machine, Method, Material, Measurement, Environment).
- **Enhanced 3x5 Why Diagram**: Redesigned the 3x5 Why multi-chain analysis as a horizontal flowchart showing linear logical paths from symptom to root cause using clean arrow connectors.
- **Unified Visualizations**: Switched the 8D module from Chart.js to Plotly.js to eliminate redundant charting libraries and match the dark-theme aesthetic across all tabs.
- **Refined Data Export/Import**: Upgraded the JSON loader to pack/unpack D1-D8 texts, 3x5 Whys, dynamic Fishbones, and Kanban tasks concurrently. Added robust migration handling for older 8D JSON files.
- **Added Team / Organization Workspace (D1 & DRI)**: Developed an interactive grid to establish cross-functional teams with columns for Name, Role, Department, Contact, and DRI (Directly Responsible Individual) status. Replaced the D1 report text area with a live summary display and quick-action redirect. Extended the JSON exporter/importer and mock data loader to support structured team arrays, with auto-parsing fallbacks for legacy plain text team entries.

### 2026-07-04 (Q-Q Plot Integration)
- **Q-Q Plot in Distribution Comparison**: Added a Quantile-Quantile (Q-Q) Plot comparing Dataset A and Dataset B quantiles to help engineers diagnose shape, center, and variance differences in a 2x2 grid.
- **Added CDF vs. Q-Q Documentation**: Documented the physical and mathematical difference between CDF plots and Q-Q plots in both the Help tab and README.md.

### 2026-07-07 (Integrated FMEA & Quality Statistics Hub Dashboard Modules)
- **FMEA Risk Assessment**: Integrated the AIAG-VDA FMEA Manager as a subset tab of the 8D workspace. Features automated Action Priority (AP) grading, row deletion, and full JSON project save/load serialization.
- **Quality Statistics Hub**: Added an independent top-level tab containing PPM Sample Size, Chi-Square Significance, and TTR Run Planner calculators in a clean 3-column responsive layout.
- **Collapsible Help Guides**: Expanded the Help / Readme panel with interactive documentation for FMEA risk metrics and Chi-Square formulas.

### 2026-07-06 (Implemented Device Drift & Shift Tab & Layout Optimizations)
- **Device Drift & Shift Tab**: Developed a complete interactive module to simulate and qualify device drift over stress loops. Integrates random-effects ANOVA for repeatability assessment, Welch's t-test for run-to-run drift significance, and boxplot distribution analysis.
- **NPI Repeatability Metrics**: Added real-time calculation and grading (Excellent, Acceptable, Poor) for SD/Median, Range/Median, Robust Sigma/Median, and Drift Repeatability (Rpt) metrics.
- **Dynamic Spec Limit Ranges**: Implemented automatic slider scaling for spec limits (LSL and USL) based on the chosen distribution (Normal, Log-normal, Weibull, etc.) to prevent squashed charts and over-expanded axes.
- **Bold Formatting Refactoring**: Replaced all raw markdown bold markers (`**`) with standard HTML `<strong>` tags in help sections and diagnostics for clean, web-standards rendering.
- **CSS Arrow Fixes**: Replaced ASCII unicode characters in CSS accordion collapse arrows with native UTF-8 minus signs (`−`) to resolve character literal rendering issues.

### 2026-07-08 (Combined Tools & Theme Customization)
- **Integrated Project Status Report**: Merged executive status reporting with milestone tracking and action items list into the main dashboard.
- **Integrated Wafer Yield Calculator**: Merged the semiconductor manufacturing productivity tool with 2D yield-vs-area curve canvas and Defect Clustering visualizations.
- **Added Dark / Light Mode**: Implemented a responsive toggle button in the header with theme persistence (`localStorage`).
- **Dynamic Chart & Canvas Themes**: Wrapped `Plotly.newPlot` to intercept layout configurations and auto-scale grid, font, and background colors to light/dark themes. Made fishbone SVG text and defect wafer canvas drawings theme-aware.
- **Robust Schema Verification**: Patched local storage loading logic to handle corrupt or outdated data objects cleanly, avoiding Javascript compilation breaks.

