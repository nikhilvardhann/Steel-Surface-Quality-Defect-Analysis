# Steel Surface Quality — Defect Analysis & Process Root Cause Investigation

Exploratory data analysis on real-world steel bar surface quality inspection data from a rolling mill, merged with furnace process control data to identify the **root causes of poor surface quality** — specifically temperature deviations and descaler pressure failures.

---

## Project Overview

Steel bars are inspected at **50 surface points** per bar. Each point is scored and the average determines the surface quality grade:

| Grade | Condition | Avg Score Range |
|-------|-----------|-----------------|
| A | Good | ~1.5 – 6.5 |
| B | Average | ~7 – 14 |
| C | Bad | ~15 – 33 |

This project answers one key business question:

> **Why do certain heats produce Bad surface condition — and what process parameters predict it?**

---

## What Makes This Project Different

Most surface quality analyses stop at the inspection data. This project goes one step further — it **merges a second process control dataset** (furnace temperatures, standard temperatures, descaler pressures) with the inspection results to directly correlate process parameters with surface outcomes.

This is a **two-dataset join analysis** — not a single-table EDA.

---

## Datasets Used

> ⚠️ **Neither dataset is included in this repository.**
> Both contain proprietary industrial data from a steel manufacturing facility and cannot be shared publicly.

**Dataset 1 — Surface Quality Inspection Report**
- 136 records (HVM rolling mill only, after filtering out ASM/BSM)
- 65 columns: heat metadata + 50 inspection point scores + surface grade
- Key fields: HEAT NO, GRADE, SIZE, CAST SIZE, ROLLING MILL, Avg, Surface Quality, Surface Condition

**Dataset 2 — Furnace Process Control Report**
- Contains heat-level furnace process readings
- Key fields: Heating Zone Temp (Top/Bottom), Soaking Zone Temp (Top/Bottom), Standard Temps, Descaler Pressure (Blooming Mill 250 bar, HV Mill 210 bar), Surface Temp after Descaling, Core Temp, Temp before/after HV Mill entry

**Merge Operation**
```python
df_process_quality = pd.merge(
    df,       # surface quality dataset
    df1,      # process control dataset
    on=['heat no', 'grade', 'customer'],
    how='inner'
)
```
Merged on 3 common keys: heat number, steel grade, and customer.

---

## Analysis Pipeline

### Phase 1 — Surface Quality EDA (Dataset 1)
- Filtered data to HVM rolling mill only (removed ASM, BSM)
- Dropped 50 inspection point columns after computing Avg score
- Standardised date formats and converted dtypes
- Identified top 3 highest-defect grades: SAE4319 (32.9), SCR420H (28.7), ST52-3 (25.4)
- Surface condition split: Bad=67, Average=38, Good=31 (out of 136 records)
- Mean defect scores by condition: Good=4.5, Average=10.8, Bad=20.7

### Phase 2 — Process Control Merge & Root Cause Analysis (Dataset 2)
- Merged inspection results with furnace process data on heat no + grade + customer
- Cleaned process columns: extracted numeric values from mixed-format pressure columns
- Engineered two deviation features:
  - `heating_dev` = actual heating zone temp − standard heating temp
  - `soaking_dev` = actual soaking zone temp − standard soaking temp
- Grouped by surface condition and computed mean process parameters
- Built Good vs Bad comparison table sorted by difference magnitude

### Phase 3 — Visualisations
- Catplot: process parameters by surface condition (Good / Average / Bad)
- Boxplot: heating zone temperature imbalance (top vs bottom) per condition
- Boxplot: descaler pressure distribution by surface condition
- Displot with KDE: parameter distributions for Bad condition heats
- Heatmap: correlation between temperature parameters in Bad heats
- Bar chart: Good vs Average vs Bad mean process values across all parameters
- Swarm plot: per-heat temperature values coloured by surface condition
- Per-heat-number bar charts showing temperature profiles for each unique heat

---

## Key Findings

- **Temperature deviation is the primary driver** — Bad surface heats show significantly higher actual vs standard temperature gaps in both heating and soaking zones
- **Descaler pressure below threshold** directly correlates with Bad surface outcomes — both Blooming Mill (250 bar standard) and HV Mill (210 bar standard) show lower average pressures for Bad heats
- **Heating zone imbalance** (top vs bottom temperature difference) is visibly higher in Bad condition heats — indicating uneven heating as a contributing factor
- **Grade-level risk** — SAE4319, SCR420H, ST52-3 consistently produce Bad outcomes regardless of other factors, suggesting alloy-specific process adjustments are needed
- **Good condition heats** show tight temperature clustering close to standard values — process stability = surface quality

---

## Technologies Used

- `pandas` — data loading, merging, cleaning, GroupBy analysis
- `numpy` — numerical operations, deviation calculations
- `matplotlib` — base plotting, bar charts, figure layout
- `seaborn` — catplot, boxplot, displot, heatmap, swarm plot, stripplot
- `openpyxl` — reading `.xlsx` Excel files

---

## How to Run

**Requirements:** Python 3.x, Google Colab or Jupyter Notebook

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/steel-defect-analysis.git
cd steel-defect-analysis
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Add your datasets:
   - Rename your inspection file to `surface_quality_data.xlsx`
   - Rename your process control file to `process_control_data.xlsx`
   - Place both in the project folder

4. The notebook expects `header=1` for the surface quality file (headers on row 2)

5. Open notebook:
```bash
jupyter notebook Defect_Analysis.ipynb
```

---

## Project Structure

```
steel-defect-analysis/
├── Defect_Analysis.ipynb        # Main analysis notebook
├── requirements.txt              # Python dependencies
├── .gitignore                    # Blocks data files from upload
└── README.md                     # Project overview
```

---

## Skills Demonstrated

- **Multi-dataset merge** — inner join across two real industrial datasets on 3 keys
- **Root cause analysis** — moved beyond descriptive stats to identify *why* defects occur
- **Feature engineering** — created temperature deviation columns from actual vs standard values
- **Advanced visualisation** — 10+ chart types including per-heat swarm plots, KDE distributions, and correlation heatmaps
- **Data cleaning** — mixed-format string extraction for pressure columns, datetime standardisation, dtype conversion
- **Domain expertise** — furnace temperature zones, descaling pressure thresholds, steel grade behaviour
