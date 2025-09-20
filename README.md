# Intragoal-SDG6
The repository contains the data for “Leaving no one behind? Synergies and trade-offs within the water SDG”. The code uses global datasets from the UN SDG Indicators database and the WHO/UNICEF Joint Monitoring Programme (JMP) to analyse progress on SDG 6 indicators across 200 countries (2000–2022)

This R workflow provides a comprehensive country-level analysis of synergies and trade-offs for SDG 6 (clean water and sanitation). It integrates:

Country-level classification based on trade-offs and synergies (derived from the supplementary flow diagram).

Intratarget trade-off analysis

Visualization outputs, including world maps and radar charts summarizing negative trends.

The workflow outputs several CSVs with processed summaries and charts for visualization and further analysis.

Workflow Summary
1. Country-Level Classifications

Input:

summary_df_with_characteristics_large_total_data.csv – country characteristics for clustering.

synergies_total.csv – country-level synergy classifications.

Steps:

Convert characteristics to numeric (1 = present, 0 = absent).

Assign unique clusters based on distinct country characteristics.

Standardize country names for consistency with global maps.

Merge trade-offs and synergies.

Plot world map with custom color coding for groups.

2. Intragoal Trade-Off Analysis 

Input:

Tradeoffs_within_sdg6.4.csv – country-year SDG6.4 metrics.

Optional: Series codes and metadata CSVs for column mapping.

Steps:

Filter and clean metric columns (convert >99 to 99, remove NAs).

Per-country trend analysis using Spearman correlations.

Filter significant correlations (p-value < 0.05).

Split metrics into WUEYST and stress groups.

Identify negative trends and rows with both positive and negative values.

Convert negative trends to binary, summarize counts and percentages.

Prepare radar chart visualization with sector-level summaries.

3. Outputs
File	Description
summary_all_correlations_sdg6.4_with_p_value.csv	All per-country correlations for SDG6.4 metrics.
summary_significant_correlations_sdg6.4.csv	Filtered correlations (p < 0.05).
wueyst_sdg6.4.csv	WUEYST metrics per country.
stress_sdg6.4.csv	Water stress metrics per country.
wueyst_sdg6.4_negative_values.csv	WUEYST metrics with negative trends.
stress_sdg6.4_negative_values.csv	Water stress metrics with negative trends.
Radar charts	Visual summary of negative trends per sector for WUEYST and stress.
World map	Country-level trade-offs and synergy classifications.
4. Methodology Notes

Country-level classifications follow the flow diagram in the supplementary information.

Trend analysis: Only countries with ≥3 observations and a minimum range of 1 are analyzed.

Significant correlations: p-value < 0.05; negative trends indicate trade-offs.

Radar charts: show prevalence (%) of negative trends across sectors (Industries, Agriculture, Services, Total).

Binary coding: 1 = negative trend present; NA = no negative trend.

5. Data Sources
SDG 6 Target	Custodian Organization	Data Links
6.1 & 6.2	WHO & UNICEF	washdata.org

6.3	WHO, UN-Habitat, UNSD	SDG 6.3.1 country files

6.3 (GEMSTAT)	GEMSTAT	gemstat.org

6.4	FAO	Aquastat

6.5	UNEP, UNECE, UNESCO	IWRM portal
, UNESCO transboundary water

6.6	UNEP	SDG661

6.a & 6.b	WHO & OECD	Archived IDS Online

Note: JMP and UN SDG data must be downloaded from the links above for the trade-off analysis.

6. Running the Workflow

Set file paths for input CSVs (data_raw, synergies_raw, Tradeoffs_within_sdg6.4.csv).

R packages:

library(dplyr)
library(ggplot2)
library(gridExtra)
library(rworldmap)
library(sf)
library(circlize)
library(tidyr)
library(stringr)
library(broom)
library(scales)
library(fmsb)
