# Steel Surface Quality Defect Analysis

> Python-based root-cause analysis identifying statistically significant process drivers of surface defects on hot-rolled steel bars at a manufacturing plant. The work was performed during my role as a Quality Engineer at Sunflag Iron & Steel.

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0+-orange.svg)](https://pandas.pydata.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.10+-red.svg)](https://scipy.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🔒 Data Confidentiality

The original analysis was performed on real plant data covering 350+ steel heats merged with the corresponding surface quality reports. Due to a non-disclosure agreement with my employer, the actual production data, customer names, and shift personnel cannot be published.

To allow the methodology to be reproducible and the code to be executed end-to-end by anyone reviewing this repository, the published notebook generates a **synthetic dataset** with the same structure and statistical relationships as the original. All findings, statistical methods, and recommendations described here were validated on the actual plant data.

---

## 🎯 Business Problem

A steel rolling mill was experiencing approximately **40% bad-quality classification** on hot-rolled steel bars from the HVM (heavy-section) rolling mill route. Two separate Excel files existed in the plant:

- A **rolling-mill process log** capturing furnace temperatures, mill speed, descaling pressures, and pass count per heat per shift
- A **surface quality report** classifying each heat as Good, Average, or Bad based on per-bar surface inspection

Nobody had formally linked these two files to identify which process parameters were driving the bad outcomes.

---

## 🧪 Approach

1. **Ingest and clean** two heterogeneous Excel files: standardized 50+ column names using regex, converted text-based pressure fields to numeric with `pd.to_numeric(errors="coerce")`, and parsed mixed-format dates.
2. **Filter** to HVM-rolled heats only (ASM and BSM mills use different process routes that would have introduced noise).
3. **Aggregate** the process log to one row per heat (mean of numeric parameters across shifts, mode of categoricals).
4. **Merge** on Heat Number — inner join yielded 99 common heats.
5. **Statistically compare** each process parameter between Good and Bad heats using **Welch's t-test** (handles unequal sample sizes and variances), ranked by p-value.
6. **Deep-dive** within individual grades (SAE52100, S48CS1V) to confirm the pattern is a process-control issue rather than material-specific.
7. **Visualize** with box plots, ranked bar charts, and categorical breakdowns.
8. **Deliver** a multi-sheet Excel report with prioritized recommendations to plant operations.

---

## 📊 Key Findings (from the real plant analysis)

After merging 350+ heats and restricting to HVM-rolled bars, four statistically significant drivers separated Good from Bad surface quality (all p-values < 0.001):

| Driver | Direction | Magnitude | Why It Matters |
|---|---|---|---|
| **HV Mill Speed** | Lower in Bad heats | **−53%** (3.79 → 1.78 m/s) | Slow rolling exposes the bar to high temperature longer → surface oxidation and scale |
| **Soaking Zone Temperature** | Higher in Bad heats | **+46 °C** (1197 → 1244 °C) | Excessive temperature causes grain-boundary oxidation |
| **Number of HV Passes** | Fewer in Bad heats | **−27%** (13.9 → 10.1) | Insufficient mechanical deformation to refine the surface |
| **Heating Zone Temperature** | Higher in Bad heats | **+24 °C** | Same over-heating pattern propagated through the furnace |

The same drivers appeared inside individual grade families, confirming this is a **process-control issue**, not a material-specific one.

---

## 🛠 Recommendations Delivered

| Priority | Action | Rationale |
|---|---|---|
| HIGH | Enforce minimum HV mill speed ≥ 3.0 m/s | Strongest single driver |
| HIGH | Cap soaking-zone top temperature at 1230 °C | Reduces over-soak and scale formation |
| HIGH | Enforce minimum 12 HV passes | Ensures adequate surface refinement |
| MEDIUM | Audit pass schedule for smaller output sizes (75-RD) | High Bad-rate concentration |

---

## 🧰 Tools and Skills Demonstrated

- **Python:** pandas, numpy, scipy.stats, matplotlib, seaborn
- **Statistical analysis:** Welch's t-test, ANOVA, hypothesis testing, p-value interpretation
- **Data engineering:** multi-file merging, column-name standardization with regex, type coercion, missing-value handling
- **Domain analysis:** grade-specific and size-specific sub-analysis to rule out confounders
- **Reporting:** multi-sheet Excel deliverables for non-technical stakeholders

---

## 📂 Repository Structure

```
Steel-Surface-Quality-Defect-Analysis/
├── Defect_Analysis_Public.ipynb     # Main notebook (runs end-to-end with synthetic data)
├── outputs/                          # Generated when notebook runs
│   ├── 02_top_drivers.png
│   ├── 03_top4_boxplots.png
│   ├── defect_analysis_tables.xlsx
│   └── df_merged_cleaned.csv
├── images/                           # Screenshots of key results (committed)
│   ├── top_drivers.png
│   └── top4_boxplots.png
├── requirements.txt
├── .gitignore
└── README.md
```

---

## ▶️ How to Run

```bash
git clone https://github.com/nikhilvardhann/Steel-Surface-Quality-Defect-Analysis.git
cd Steel-Surface-Quality-Defect-Analysis
pip install -r requirements.txt
jupyter notebook Defect_Analysis_Public.ipynb
```

Run all cells. The notebook will:
1. Generate a synthetic dataset of 350 heats
2. Clean, filter, and merge the two source DataFrames
3. Run statistical comparisons across 12 process parameters
4. Produce 2 visualizations and a 6-sheet Excel report under `outputs/`

---

## 📌 Limitations of the Original Analysis

- Single month of production data; seasonal effects not captured
- Good-group sample size smaller than Bad-group; magnitude estimates are directional, not precise
- The analysis identifies **correlation, not causation** — full validation would require a controlled trial in which one parameter is held within recommended bounds for a defined period and outcomes are compared

---

## 👤 Author

**Nikhil Vardhan** — Quality Engineer at Sunflag Iron & Steel  
B.Tech, Metallurgical and Materials Engineering, VNIT Nagpur  

[LinkedIn](https://www.linkedin.com/in/nikhil-vardhan-4b248a26b) · [GitHub](https://github.com/nikhilvardhann)
