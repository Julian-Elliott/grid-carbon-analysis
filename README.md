# Grid Carbon Analysis

A comprehensive analysis of the best time of day to use energy to maximise your usage of low carbon energy.

<Picture to break up sections to go here>

## Project Overview

This project analyses the carbon intensity of electricity across different regions of Great Britain, helping businesses and consumers understand when and where electricity is cleanest. By combining generation mix data with regional carbon intensity forecasts from NESO (National Energy System Operator), I provide actionable insights for sustainable energy consumption.

## Why This Matters

**For Businesses:** Understanding electricity carbon intensity helps optimise energy usage for sustainability targets, reduce emissions, and make informed decisions about when to run energy-intensive processes.

**For Consumers:** Know when your electricity has the lowest carbon footprint, typically when renewable generation is high and fossil fuel use is low.

**For Policymakers:** Track progress toward net-zero goals and identify regional differences in grid decarbonisation.

## Data Sources & Quality

### NESO Historic Generation Mix
- **Source:** National Energy System Operator Open Data Portal
- **Resolution:** 30-minute intervals
- **Content:** Power generation by fuel type (gas, coal, nuclear, wind, solar, etc.)

### NESO Regional Carbon Intensity Forecasts
- **Source:** NESO Regional Carbon Intensity Data Portal
- **Resolution:** 30-minute intervals  
- **Content:** Predicted carbon intensity per region (gCO2/kWh)

<Picture to break up sections to go here>

## Data Transformation Process

### Step 1: Data Quality Assessment
**What we found:**
- Generation mix data: Perfect completeness (100% coverage)
- Regional carbon intensity: Small gaps (1.1% missing values) occurring systematically

**Why this matters:** Missing data can skew analysis results. We needed to understand the pattern of missing values before deciding how to handle them.

### Step 2: Standardisation
**Actions taken:**
- Converted all timestamps to UTC timezone for consistency
- Standardised column naming conventions
- Verified 30-minute interval completeness

**Business impact:** Ensures reliable time-series analysis and accurate regional comparisons.

### Step 3: Missing Data Strategy
**The Challenge:** Regional carbon intensity data has systematic gaps where all regions are missing simultaneously, suggesting NESO system downtime rather than regional issues.

**Our Solution Options:**
1. **Simple Removal:** Delete incomplete records (loses 1.1% of data)
2. **Mean Imputation:** Fill gaps with regional averages (preserves data volume)
3. **Time-Pattern Imputation:** Use daily/seasonal patterns (more accurate)
4. **ML-Enhanced Imputation:** Combine generation mix with time patterns (most sophisticated)

**Recommendation:** We'll implement time-pattern imputation enhanced with generation mix data for maximum accuracy while maintaining data integrity.

### Step 4: Feature Engineering
**Enhancements added:**
- Time-based features (hour, day of week, season)
- Generation mix ratios (renewable vs fossil)
- Carbon intensity categories (low, medium, high)
- Regional groupings for analysis

**Business value:** These features enable deeper insights into when and why carbon intensity varies.

<Picture to break up sections to go here>

## Key Insights from Data Exploration

### Generation Mix Patterns
- **Renewable Energy:** Highly variable, weather-dependent
- **Gas Generation:** Primary backup for renewables
- **Coal Usage:** Declining trend, used mainly for peak demand
- **Nuclear:** Consistent baseload power

### Regional Carbon Intensity Variations
- **Cleanest Regions:** Areas with high renewable generation (Scotland, wind-rich areas)
- **Highest Intensity:** Industrial regions relying on fossil fuels
- **Time Patterns:** Lower carbon intensity during windy/sunny periods

### Negative Carbon Intensity Explanation
**Why some regions show negative values:** When a region generates more renewable energy than it consumes, it exports clean electricity to neighbouring regions, effectively "offsetting" their carbon intensity. This is a positive indicator of renewable energy success.

<Picture to break up sections to go here>

## Data Pipeline Architecture

```
Raw Data Sources
├── NESO Generation Mix (Complete)
└── NESO Regional Carbon Intensity (99% Complete)
    ↓
Data Validation & Quality Checks
    ↓
Standardisation & Cleaning
├── UTC timestamp conversion
├── Missing data imputation
└── Feature engineering
    ↓
Processed Datasets
├── generation_mix_processed.csv
├── regional_carbon_intensity_processed.csv
└── Documentation files
    ↓
Analysis-Ready Data
```

<Picture to break up sections to go here>

## File Structure

```
data/
├── raw/                           # Original NESO data files
├── processed/                     # Clean, analysis-ready datasets
├── data_dictionary/               # Column descriptions and metadata
└── descriptive_statistics/        # Summary statistics and data profiles

notebooks/
├── 01_extract_neso_data.ipynb                    # Data collection from NESO URL
├── 02_transform_generation_mix.ipynb             # Generation mix cleaning
├── 03_transform_regional_carbon_intensity.ipynb  # Regional data processing
└── 04_transform_merged_data.ipynb                # Combined dataset creation
```

<Picture to break up sections to go here>

## Quality Assurance

### Data Validation Checks
- Timestamp completeness (30-minute intervals)
- Reasonable value ranges (carbon intensity 0-800 gCO2/kWh)
- Regional data consistency
- Generation mix totals validation

### Transformation Transparency
- Complete statistical summaries for each dataset
- Detailed data dictionaries with NESO source definitions
- Transformation logs documenting all changes
- Quality flags identifying imputed vs original data

<Picture to break up sections to go here>

## Data Reliability Statement

This analysis uses official data from NESO, the UK's electricity system operator. All transformations maintain data integrity while improving usability. Missing values are handled transparently with quality flags, ensuring users can distinguish between original and imputed data.

**Data Currency:** Updated through August 11, 2025
**Confidence Level:** High (99% complete source data)
**Validation Status:** All datasets pass quality checks