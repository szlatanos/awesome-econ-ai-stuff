---
name: econ-visualization
description: Create publication-quality charts and graphs for economics papers.
workflow_stage: communication
compatibility:
  - claude-code
  - cursor
  - codex
  - gemini-cli
author: Awesome Econ AI Community
version: 2.0.0
tags:
  - visualization
  - ggplot2
  - matplotlib
  - matlab
  - charts
  - publication
  - bayesian
  - uncertainty-quantification
---

# Econ Visualization

## Purpose

This skill creates publication-quality figures for economics papers, using clean styling, consistent scales, and export-ready formats. It supports R, Python, and MATLAB with specialized patterns for Bayesian uncertainty visualization, multi-panel layouts, and macro econometric time series.

## When to Use

- Building figures for empirical results and descriptive analysis
- Visualizing Bayesian posterior distributions and credible intervals
- Creating multi-panel layouts for comparative analysis
- Standardizing chart style across a paper or presentation
- Exporting figures to PDF or PNG at journal quality

## Instructions

Follow these steps to complete the task:

### Step 1: Understand the Context

Before generating any code, ask the user:

- What is the dataset and key variables?
- What chart type is needed (line, bar, scatter, event study, uncertainty bands)?
- What language/environment (R, Python, MATLAB)?
- For Bayesian analysis: Do you have posterior draws or pre-computed quantiles?
- What output format and size are required?

### Step 2: Generate the Output

Based on the context, generate code that:

1. **Uses a consistent theme** for academic styling
2. **Labels axes and legends clearly**
3. **Exports figures** at high resolution (vector format preferred)
4. **Includes reproducible steps** for data preparation
5. **Adds uncertainty visualization** when appropriate (credible/confidence intervals)

### Step 3: Verify and Explain

After generating output:

- Explain how to regenerate or update the plot
- Suggest alternatives (log scales, faceting, smoothing, different uncertainty levels)
- Note any data transformations used
- For Bayesian plots: explain quantile structure and credible interval interpretation

## Example Prompts

- "Create an event study plot with confidence intervals in R"
- "Plot GDP per capita over time for three countries in Python"
- "Build a MATLAB figure with shaded Bayesian credible intervals"
- "Generate a multi-panel layout showing trends and cycles"
- "Create prior vs posterior comparison plots"
- "Add recession shading to macroeconomic time series"

## Language-Specific Examples

### R (ggplot2) - Basic Time Series

```r
# ============================================
# Publication-Quality Figure in R
# ============================================
library(tidyverse)

df <- read_csv("data.csv")

ggplot(df, aes(x = year, y = gdp_per_capita, color = country)) +
  geom_line(size = 1) +
  scale_y_continuous(labels = scales::comma) +
  labs(
    title = "GDP per Capita Over Time",
    x = "Year",
    y = "GDP per Capita (USD)",
    color = "Country"
  ) +
  theme_minimal(base_size = 12) +
  theme(
    legend.position = "bottom",
    panel.grid.minor = element_blank()
  )

ggsave("figures/gdp_per_capita.pdf", width = 7, height = 4, dpi = 300)
```

### Python (matplotlib) - Basic Time Series

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

plt.figure(figsize=(7, 4))
for country in df['country'].unique():
    data = df[df['country'] == country]
    plt.plot(data['year'], data['gdp_per_capita'], label=country, linewidth=1.5)

plt.xlabel('Year')
plt.ylabel('GDP per Capita (USD)')
plt.title('GDP per Capita Over Time')
plt.legend(loc='best')
plt.grid(True, alpha=0.3)
plt.tight_layout()

plt.savefig('figures/gdp_per_capita.pdf', format='pdf', dpi=300)
plt.show()
```

### MATLAB - Bayesian Time Series with Uncertainty Bands

```matlab
% ============================================
% MATLAB: Shaded Quantile Bands
% ============================================
% For Bayesian posterior distributions
% Data structure: posteriorDraws is T × N (time periods × MCMC draws)

% Step 1: Compute quantiles from posterior draws
quantiles = [0.05, 0.16, 0.50, 0.84, 0.95];
qData = quantile(posteriorDraws, quantiles, 2);  % T × 5 matrix

% Step 2: Plot with shaded uncertainty bands
PlotStatesShaded(Time, qData);

% ----------------------------------------
% Core function: PlotStatesShaded
% ----------------------------------------
function PlotStatesShaded(Time, qS, color, transparency)
    % Plots time series with nested Bayesian credible intervals
    %
    % Parameters:
    %   Time - Time vector (datetime, datenum, or numeric)
    %   qS - Matrix with columns [q05, q16, q50, q84, q95]
    %   color - RGB triplet (default: [0.5 0.6 1])
    %   transparency - Alpha value 0-1 (default: 0.5)
    %
    % Creates:
    %   - Outer band (5%-95% credible interval) in lighter shade
    %   - Inner band (16%-84% credible interval) in darker shade
    %   - Median line (50th percentile)
    %   - Zero reference line

    if nargin < 4, transparency = 0.5; end
    if nargin < 3, color = [0.5 0.6 1]; end  % Default blue

    linecolor = color * 0.65;  % Darken for median line

    % Outer band (90% credible interval: 5th to 95th percentile)
    fill([Time; flip(Time)], [qS(:,1); flip(qS(:,5))], ...
         0.9*color, 'LineStyle', 'none', 'FaceAlpha', transparency);
    hold on;

    % Inner band (68% credible interval: 16th to 84th percentile)
    fill([Time; flip(Time)], [qS(:,2); flip(qS(:,4))], ...
         0.6*color, 'LineStyle', 'none', 'FaceAlpha', transparency);

    % Median line (50th percentile)
    plot(Time, qS(:,3), '-', 'Color', linecolor, 'LineWidth', 0.5);

    % Zero reference
    plot(Time, Time*0, 'k', 'LineWidth', 0.25);

    hold off;
    axis tight; box on;

    % Format dates if applicable
    if isdatetime(Time) || isnumeric(Time)
        datetick('x', 'yyyy', 'keeplimits');
    end
end
```

### MATLAB - Multi-Panel Publication Layouts

```matlab
% ============================================
% MATLAB: Multi-Panel Figure (2×4 grid)
% ============================================

% Setup: Publication-ready dimensions
f = figure('Units', 'inches', 'Position', [0, 0, 12.5, 5]);
tiledlayout(2, 4, 'TileSpacing', 'compact', 'Padding', 'compact');

% Custom styling
darkRed = [230, 77, 77]/255;
markerInterval = 12;  % Place markers every 12 observations
markerIndices = 1:markerInterval:length(Time);

plotOptions = {'Color', darkRed, ...
               'Marker', 's', ...
               'MarkerFaceColor', darkRed, ...
               'MarkerEdgeColor', darkRed, ...
               'MarkerIndices', markerIndices, ...
               'LineWidth', 1.5, ...
               'MarkerSize', 6};

% Define consistent axis ranges
xlimRange = [1960, 2020];
ylimRanges = {[-10, 15], [0, 12], [0, 12], [-5, 20], ...
              [-3, 12], [-3, 12], [-3, 12], [-50, 50]};
titles = {'Output growth', 'Unempl. Rate', 'Unempl. Rate Exp. (1yr)', ...
          'Nominal Interest Rate', 'Inflation', 'Inflation Exp. (1yr)', ...
          'Inflation Exp. (10yr)', 'Oil Price Inflation'};

% Loop through variables
for i = 1:8
    nexttile;

    % Plot data with shaded quantiles
    PlotStatesShaded(Time, posteriorQuantiles{i});

    % Overlay observations if available
    if ~isempty(observations)
        hold on;
        plot(Time, observations(:,i), plotOptions{:});
        hold off;
    end

    % Set limits and title
    xlim([datenum(datetime(xlimRange(1), 1, 1)), ...
          datenum(datetime(xlimRange(2), 1, 1))]);
    ylim(ylimRanges{i});
    title(titles{i}, 'Interpreter', 'latex');
end

% Export to PDF
exportgraphics(f, 'figures/multi_panel.pdf', 'ContentType', 'vector');
```

### MATLAB - Prior vs Posterior Comparison

```matlab
% ============================================
% MATLAB: Bayesian Prior/Posterior Visualization
% ============================================

f = figure;

% Posterior distribution (blue filled area)
[dens_post, grids_post] = ksdensity(posterior_samples);
area(grids_post, dens_post, ...
     'FaceColor', [0 0.4470 0.7410], ...
     'EdgeColor', [0 0.4470 0.7410]);
hold on;

% Prior distribution (red line)
[dens_prior, grids_prior] = ksdensity(prior_samples);
plot(grids_prior, dens_prior, ...
     'Color', [0.6350 0.0780 0.1840], ...
     'LineWidth', 2.5);

legend({'Prior', 'Posterior'}, 'FontSize', 18, 'Interpreter', 'latex');
title('Parameter Distribution: $\sigma_{\pi}$', 'Interpreter', 'latex');
xlabel('Value');
ylabel('Density');

% Export to PDF
exportgraphics(f, 'figures/sigma_pi_posterior.pdf', 'ContentType', 'vector');
```

### MATLAB - Recession Shading for Macro Time Series

```matlab
% ============================================
% MATLAB: Add NBER Recession Shading
% ============================================

% After creating your time series plot, add recession bars
% recessionplot();  % If you have this function in your path

% Or implement manually:
function addRecessionShading(recessionDates)
    % recessionDates: N×2 matrix of [startDate, endDate] pairs
    % Example: [datenum('1990-07-01'), datenum('1991-03-01');
    %           datenum('2001-03-01'), datenum('2001-11-01');
    %           datenum('2007-12-01'), datenum('2009-06-01');
    %           datenum('2020-02-01'), datenum('2020-04-01')]

    ylims = get(gca, 'YLim');
    hold on;

    for i = 1:size(recessionDates, 1)
        fill([recessionDates(i,1), recessionDates(i,2), ...
              recessionDates(i,2), recessionDates(i,1)], ...
             [ylims(1), ylims(1), ylims(2), ylims(2)], ...
             [0.9 0.9 0.9], 'EdgeColor', 'none', 'FaceAlpha', 0.3);
    end

    % Bring plot elements to front
    ch = get(gca, 'Children');
    set(gca, 'Children', [ch(end-1:end); ch(1:end-2)]);

    hold off;
end
```

## Visualization Types

### Time Series Visualizations

**Single Series:**
- Line plot with trend
- With/without confidence/credible intervals
- Optional recession shading for macro data

**Multiple Series:**
- Comparative time series
- Different line styles for each series
- Shared or separate y-axes

**Uncertainty Quantification:**
- Shaded bands for Bayesian credible intervals (68%, 90%, 95%)
- Error bars or ribbons for frequentist confidence intervals
- Fan charts for forecasts

### Multi-Panel Layouts

**Trends vs Cycles:**
- Separate panels for long-run trends and cyclical components
- Consistent styling across panels
- Shared time axis

**Variable Grids:**
- 2×2, 2×4, 3×3 layouts for related variables
- Compact spacing for publication
- Optional shared axis ranges for comparability

**Comparative Analysis:**
- Side-by-side model comparisons
- Different specifications in separate panels
- Robustness checks visualization

### Bayesian Analysis Plots

**Distribution Comparisons:**
- Prior vs posterior distributions
- Kernel density estimates
- Histogram overlays

**Parameter Uncertainty:**
- Trace plots for convergence diagnostics
- Posterior predictive checks
- Credible interval plots

**Model Comparison:**
- Bayes factors visualization
- Posterior model probabilities
- WAIC/LOO comparisons

## Requirements

### Software

- **R 4.0+** OR
- **Python 3.10+** OR
- **MATLAB R2020b+**

### Packages

- **For R**: `ggplot2`, `scales`, `dplyr`, `tidyr`
- **For Python**: `matplotlib`, `seaborn`, `pandas`, `numpy`
- **For MATLAB**: Built-in graphics functions (custom helper functions recommended for advanced features)

## Best Practices

### General

1. **Use vector formats** (PDF, EPS, SVG) for publication
2. **Keep labels concise** and readable
3. **Document data filters** and transformations
4. **Consistent color schemes** across related figures
5. **Accessible colors** (colorblind-friendly palettes)

### MATLAB-Specific

6. **Use `exportgraphics` (R2020a+)** instead of `print` for better performance and quality (officially recommended by MathWorks)
7. **Exact paper dimensions** for journal requirements (`'Units', 'inches'`)
8. **Use `tiledlayout`** (not `subplot`) for modern multi-panel figures
9. **Shaded uncertainty bands** for Bayesian analyses (two-level: 68% + 90%)
10. **Recession shading** for macroeconomic time series
11. **Custom color palettes** (avoid default MATLAB blue)
12. **Marker intervals** for long time series (readability)
13. **LaTeX interpreter** for mathematical notation in titles/labels
14. **Consistent axis ranges** across related panels
15. **Organized output** (subfolders for figures)

### R-Specific

16. **`theme_minimal()` or `theme_bw()`** for clean academic style
17. **`ggsave()` with explicit dimensions** and DPI
18. **Faceting** for multi-panel layouts (`facet_wrap`, `facet_grid`)

### Python-Specific

19. **`plt.tight_layout()`** to prevent label cutoff
20. **Seaborn themes** for quick styling (`sns.set_style()`)
21. **Explicit figure sizes** with `figsize` parameter

## Common Patterns

### MATLAB: Publication-Ready Figure Setup

```matlab
% Create figure with exact dimensions
f = figure('Units', 'inches', 'Position', [0, 0, 12.5, 5]);

% Export to PDF
exportgraphics(f, 'figures/my_figure.pdf', 'ContentType', 'vector');
```

### MATLAB: Custom Colors and Styling

```matlab
% Define custom colors (avoid default blue)
darkRed = [230, 77, 77]/255;
darkBlue = [51, 102, 204]/255;
darkGreen = [34, 139, 34]/255;

% Marker options for long time series
markerInterval = 12;  % Every 12 observations
markerIndices = 1:markerInterval:length(Time);

plotOptions = {'Color', darkRed, ...
               'Marker', 's', ...             % Square markers
               'MarkerFaceColor', darkRed, ...
               'MarkerEdgeColor', darkRed, ...
               'MarkerIndices', markerIndices, ...
               'LineWidth', 1.5, ...
               'MarkerSize', 6};

% Apply to plot
plot(Time, data, plotOptions{:});
```

### MATLAB: Data Preparation for Quantile Plots

```matlab
% Convert MCMC posterior draws to quantile structure
% posteriorDraws: T × N matrix (T time periods, N MCMC draws)

% Standard quantiles for credible intervals
quantiles = [0.05, 0.16, 0.50, 0.84, 0.95];

% Compute quantiles across draws (dimension 2)
qData = quantile(posteriorDraws, quantiles, 2);  % Returns T × 5 matrix

% Structure: [5th percentile, 16th, median, 84th, 95th]
% - 5th to 95th: 90% credible interval (outer band)
% - 16th to 84th: 68% credible interval (inner band)
% - 50th: median (central line)

% Now ready to plot
PlotStatesShaded(Time, qData);
```

### R: Event Study Plot with Confidence Intervals

```r
# Event study coefficients and standard errors
event_study <- data.frame(
  period = -10:10,
  coef = c(...),  # Coefficients
  se = c(...)     # Standard errors
)

# Create confidence intervals
event_study <- event_study %>%
  mutate(
    lower_95 = coef - 1.96*se,
    upper_95 = coef + 1.96*se
  )

# Plot
ggplot(event_study, aes(x = period, y = coef)) +
  geom_ribbon(aes(ymin = lower_95, ymax = upper_95), alpha = 0.2) +
  geom_line() +
  geom_point() +
  geom_hline(yintercept = 0, linetype = "dashed") +
  geom_vline(xintercept = -0.5, linetype = "dotted", color = "red") +
  labs(
    title = "Event Study: Treatment Effect Over Time",
    x = "Periods Relative to Treatment",
    y = "Coefficient"
  ) +
  theme_minimal()
```

## Common Pitfalls

### General
- Overcrowded plots without clear labeling
- Inconsistent scales across figures
- Exporting low-resolution images (always use 300+ DPI)
- Using default colors that aren't publication-quality
- Forgetting to label axes or add titles

### MATLAB-Specific
- Using `subplot` instead of `tiledlayout` (outdated)
- Not setting exact paper dimensions (causes whitespace issues)
- Forgetting to specify `'Interpreter', 'latex'` for math notation
- Plotting without transparency for overlapping elements
- Not handling date formatting properly

### Bayesian Uncertainty
- Confusing credible intervals with confidence intervals
- Not labeling which quantiles are being shown
- Inconsistent credible interval levels across figures
- Forgetting to explain posterior interpretation

## References

- [ggplot2 documentation](https://ggplot2.tidyverse.org/)
- [matplotlib documentation](https://matplotlib.org/)
- [MATLAB Graphics Documentation](https://www.mathworks.com/help/matlab/graphics.html)
- [Tufte (2001) The Visual Display of Quantitative Information](https://www.edwardtufte.com/tufte/books_vdqi)
- [Gelman et al. (2013) Bayesian Data Analysis](http://www.stat.columbia.edu/~gelman/book/)
- [Kruschke (2014) Doing Bayesian Data Analysis](https://sites.google.com/site/doingbayesiandataanalysis/)

## Changelog

### v2.0.0

- Added comprehensive MATLAB support
- Bayesian uncertainty visualization patterns
- Multi-panel layout examples
- Prior vs posterior comparison plots
- Recession shading for macro time series
- Custom color palette examples
- Publication-ready export utilities
- Quantile data preparation workflows
- Enhanced with real econometric research patterns

### v1.0.0

- Initial release with R and Python examples
