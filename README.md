# Steel Surface Quality — Defect Analysis

Exploratory data analysis on steel bar surface quality inspection data from a rolling mill. The goal is to identify patterns in surface defects across different steel grades, bar sizes, and rolling mill parameters to support quality control decisions.

---

## Project Overview

Steel bars are inspected at **50 surface points** per bar. Each point is scored, and the average score determines the surface quality grade:

| Grade | Condition | Meaning |
|-------|-----------|---------|
| A | Good | Surface meets quality standard |
| B | Average | Minor surface issues present |
| C | Bad | Surface defects — requires review |

This project analyses what process factors (grade, size, rolling mill, cast size) most strongly influence whether a bar ends up as Good, Average, or Bad.

---

## Key Findings

- **Steel grade significantly affects surface condition** — certain alloy grades show consistently higher defect averages
- **Bar size impacts quality variability** — larger cross-sections tend to produce more variable defect scores
- **Rolling mill type** (BSM, ASM, HVM) shows measurable differences in average surface quality outcomes
- **Average defect score distribution is right-skewed** — most bars cluster at low defect averages but a small number of heats produce significantly worse outcomes
- **Grade A (Good)** accounts for the majority of inspected bars, indicating overall process stability

---

## Dataset

> ⚠️ **The original dataset is not included in this repository.**
> It contains proprietary industrial inspection data from a steel manufacturing facility and cannot be shared publicly.

To run this notebook with your own data, provide an Excel file named `surface_quality_data.xlsx` with the following column structure:

| Column | Description |
|--------|-------------|
| `INSP. DATE` | Date of surface inspection |
| `CAST DATE` | Date the billet was cast |
| `CAST SIZE` | Billet cross-section size (e.g. 165X165) |
| `Rolling Date` | Date the billet was rolled |
| `HEAT NO.` | Unique heat identifier |
| `GRADE` | Steel grade (e.g. SCM420HV, 42CrMo4, C43) |
| `SIZE` | Finished bar diameter (e.g. 34-RD) |
| `Rolled pcs` | Number of pieces rolled in that heat |
| `ROLLING MILL` | Mill identifier (BSM / ASM / HVM) |
| `1` to `50` | Defect scores at each of the 50 inspection points |
| `Avg` | Average defect score across all 50 points |
| `Surface Quality` | Grade: A / B / C |
| `Surface Condition` | Good / Average / Bad |
| `Remarks` | Any additional notes |

Place this file in the same directory as the notebook before running.

---

## Libraries Used

- `pandas` — data loading, cleaning, and manipulation
- `numpy` — numerical operations
- `matplotlib` — base plotting
- `seaborn` — statistical visualisations (boxplots, countplots, histplots)
- `openpyxl` — reading `.xlsx` Excel files

---

## How to Run

**Option 1 — Local (Jupyter)**

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/steel-defect-analysis.git
cd steel-defect-analysis
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Add your dataset — rename your file to `surface_quality_data.xlsx` and place it in the project folder

4. Open the notebook:
```bash
jupyter notebook Defect_Analysis.ipynb
```

**Option 2 — Google Colab**

- Open `Defect_Analysis.ipynb` in Colab
- Upload your `surface_quality_data.xlsx` when prompted
- Run all cells

> Note: The notebook uses `header=1` when reading the Excel file — ensure your column headers are on row 2.

---

## Project Structure

```
steel-defect-analysis/
├── Defect_Analysis.ipynb     # Main analysis notebook
├── requirements.txt           # Python dependencies
├── .gitignore                 # Files excluded from version control
└── README.md                  # Project overview
```

---

## Skills Demonstrated

- Real-world industrial data analysis (not a Kaggle toy dataset)
- Data cleaning: handling missing values, column filtering, type conversion
- Analysis across 65 columns including 50 inspection point scores
- GroupBy aggregation by grade, bar size, and rolling mill type
- Multi-chart visualisation: boxplots, countplots, distribution plots
- Domain knowledge: steel grades, surface quality classification, rolling mill processes
