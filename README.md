<div align="center">

# 🚢 Global Supply Chain Risk 2026

### Why do shipments fail, and what can a logistics team actually do about it?

A statistical deep-dive into **5,000 global shipments** (Jan 2024 – Dec 2025), plus a **Power BI dashboard** that turns the findings into decisions.

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-Statistics-8CAAE6?logo=scipy&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-2ea44f)
![License](https://img.shields.io/badge/License-MIT-green)

</div>

<!-- Replace with your final dashboard screenshot -->
<p align="center">
  <img src="images/dashboard.png" alt="Global Supply Chain Risk dashboard" width="920">
</p>

---

## 🎯 At a Glance

| 📦 Shipments | ⚠️ Disrupted | 🌪️ Top Risk Factor | 💸 Estimated Loss |
|:---:|:---:|:---:|:---:|
| **5,000** | **61.3%** (3,063) | **Storm / Hurricane** (OR ≈ 18.6) | **≈ $243M** |

> **One-line takeaway:** extreme weather causes almost all of the damage. Hurricane alone drives **82.6%** of estimated losses, so mitigation money should follow the weather, not be spread evenly.

---

## 📑 Table of Contents

- [Key Findings](#-key-findings)
- [Project Workflow](#-project-workflow)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Results](#-results)
- [Financial Impact](#-financial-impact)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Recommendations](#-recommendations)
- [Limitations](#-limitations)
- [Future Work](#-future-work)
- [Author](#-author)

---

## 🔍 Key Findings

| # | Finding | Evidence |
|:-:|---|---|
| 1 | **Weather is the strongest driver.** Storm and Hurricane almost always end in disruption. | Odds ratio 18.57; Hurricane 100%, Storm 79.5% disrupted |
| 2 | **Geopolitical risk raises the odds.** | 49.8% (low) → 74.5% (high); +26% odds per +1 score point |
| 3 | **Reliable carriers help, with a smaller effect.** | About 9-point gap; same benefit in every weather condition |
| 4 | **Mode, distance, and month do not change disruption odds.** They only shape lead time. | Mode p = 0.98; month p = 0.69; distance OR = 1.00 |
| 5 | **Losses are extremely concentrated.** | Hurricane = 82.6% of ≈ $243M estimated loss |

---

## 🧭 Project Workflow

```mermaid
flowchart LR
    A[Raw CSV<br/>5,000 shipments] --> B[Data quality<br/>checks]
    B --> C[Hypothesis tests<br/>H1 - H6]
    C --> D[Logistic and<br/>lead-time regression]
    D --> E[Mitigation and<br/>loss estimate]
    E --> F[Power BI<br/>dashboard]
```

---

## 🗂️ Dataset

**File:** `global_supply_chain_risk_2026.csv` · 5,000 rows · Jan 2024 – Dec 2025

| Column | Description |
|---|---|
| `Shipment_ID` | Unique shipment identifier |
| `Date` | Shipment date |
| `Origin_Port`, `Destination_Port` | Origin and destination ports |
| `Transport_Mode` | Air, Rail, Road, Sea |
| `Product_Category` | Automotive, Electronics, Perishables, Pharmaceuticals, Textiles |
| `Distance_km` | Shipping distance |
| `Weight_MT` | Cargo weight (metric tons) |
| `Fuel_Price_Index` | Fuel price index |
| `Geopolitical_Risk_Score` | Geopolitical risk score (0–10) |
| `Carrier_Reliability_Score` | Carrier reliability score |
| `Weather_Condition` | Clear, Rain, Fog, Storm, Hurricane |
| `Lead_Time_Days` | Delivery time in days |
| `Disruption_Occurred` | **Target:** 1 = disrupted, 0 = not disrupted |

**Derived columns (created in the notebook):** `Kelompok_Geo`, `Kelompok_Carrier`, `Kelompok_Jarak`, `Bulan`, `SLA_hari`, `Hari_telat`, `Kerugian_USD`.

> [!NOTE]
> Derived column names are in Indonesian (`Kelompok` = group, `Jarak` = distance, `Bulan` = month, `Hari_telat` = days late, `Kerugian` = loss) because the notebook itself is written in Indonesian.

---

## 🧪 Methodology

The notebook is organized **by hypothesis**. Every chart is backed by a statistical test (α = 0.05), so differences are never judged by eye alone.

| Stage | What it does |
|---|---|
| **Data quality** | Column types, missing values, duplicates, distributions |
| **Baseline** | Overall disruption rate; lead time mean and median |
| **H1** Weather → Disruption | Chi-square, Cramér's V, pairwise tests (Bonferroni) |
| **H2** Transport mode → Lead time | Kruskal-Wallis, Mann-Whitney |
| **H3** Geopolitical risk → Disruption | Chi-square, point-biserial correlation |
| **H4** Carrier reliability → Disruption | Chi-square, two-proportion z-test, relative risk |
| **H5** Distance → Disruption and lead time | Chi-square, Spearman |
| **H6** Time trend | Chi-square across months, Spearman on month order |
| **Multivariate** | Carrier × Distance, Carrier × Weather, logistic regression (odds ratios), log lead-time regression |
| **Risk ranking** | Spread, p-value, Cramér's V per factor |
| **Mitigation and finance** | P90 buffer by weather × mode, loss estimate, carrier-switch scenario |

<details>
<summary><b>Why non-parametric tests for lead time?</b></summary>

Lead time is right-skewed (mean 19.4 days vs median 8.2 days), so the analysis uses rank-based tests (Kruskal-Wallis, Mann-Whitney, Spearman) and reports the median alongside the mean.

</details>

---

## 📊 Results

### Risk factors (logistic regression, other factors controlled)

| Factor | Odds ratio | Significant? |
|---|:-:|:-:|
| Storm / Hurricane | **18.57** | ✅ |
| Fog | 1.61 | ✅ |
| Geopolitical score (+1 point) | 1.26 | ✅ |
| Reliable carrier (+0.1 score) | 0.86 | ✅ |
| Rain | 1.21 | ❌ |
| Distance (+1,000 km) | 1.00 | ❌ |

### Disruption rate by segment

| Weather | Disrupted |
|---|---:|
| 🔴 Hurricane | 100.0% |
| 🔴 Storm | 79.5% |
| 🟢 Fog | 48.1% |
| 🟢 Rain | 42.0% |
| 🟢 Clear | 37.0% |

| Geopolitical risk | Disrupted | Carrier reliability | Disrupted |
|---|---:|---|---:|
| High | 74.5% | Low | 65.2% |
| Medium | 64.7% | Medium | 62.8% |
| Low | 49.8% | High | 55.7% |

### Lead time

- Median **8.2 days**, mean **19.4 days** (right-skewed).
- Mode sets speed: **Sea** (median ≈ 21.8 days) is slowest, **Air** (≈ 0.9 days) is fastest.
- Mode does **not** change the odds of disruption (p = 0.98).

### P90 lead-time buffer (days)

| Weather | Air | Rail | Road | Sea |
|---|---:|---:|---:|---:|
| Hurricane | 7.2 | 90.2 | 81.6 | 198.4 |
| Storm | 2.8 | 37.5 | 31.1 | 69.7 |
| Fog | 1.7 | 18.9 | 15.2 | 36.5 |
| Rain | 1.3 | 14.1 | 12.4 | 28.6 |
| Clear | 1.3 | 11.6 | 9.5 | 24.1 |

Planning teams can use this table directly: when the forecast shows a given weather type, add the buffer for the mode in use.

---

## 💸 Financial Impact

The dataset has no cost columns, so the estimate relies on **assumption parameters** that you can replace with real company figures:

```python
BIAYA_TETAP_GANGGUAN    = 5000   # USD fixed cost per disrupted shipment
BIAYA_TELAT_PER_MT_HARI = 20     # USD per metric ton per day beyond SLA
PERSENTIL_SLA           = 0.90   # SLA = P90 lead time of non-disrupted shipments
```

- **SLA** = P90 lead time of non-disrupted shipments, per mode × distance group.
- **Loss** = fixed disruption cost + (days beyond SLA × weight × daily cost per MT).
- Total estimated loss: **≈ $243.1M** (≈ $48.6K per shipment on average).
- **82.6%** of that loss comes from Hurricane shipments.

> [!WARNING]
> These are estimates built on assumed costs, not actual financial data.

---

## 📈 Power BI Dashboard

**What is on the page**

- **KPI row:** Total Shipments, Disrupted Shipments, Disruption Rate, Avg Lead Time, Avg Geopolitical Risk, Avg Carrier Reliability
- **Disruption Rate %** by Weather, Geopolitical Risk, and Carrier Reliability
- **Avg Lead Time** by Transport Mode
- **Shipments** by Distance Band
- **Disruption Rate %** by Month
- **Slicers:** Transport Mode, Year, Weather Condition, Product Category
- **Key insight** box with the headline takeaway

**Design decisions**

| Choice | Why |
|---|---|
| One color rule for every disruption chart: 🔴 ≥ 70%, 🟡 50–70%, 🟢 < 50% | The same color always means the same risk level |
| Every rate chart uses a 0–100% axis | Bars are comparable across charts |
| Categories ordered Low → High, short → long distance | Natural reading order |
| Lead-time chart highlights only Sea (blue, not risk colors) | Lead time is speed, not risk |
| Monthly line is not smoothed, with an average line | Small wobbles are not mistaken for a trend |

> [!NOTE]
> The notebook groups geopolitical risk into 3 levels. The dashboard may use 4 bands (Low to Very High), so band-level percentages can differ slightly from the notebook.

### Rebuild it

**1. Export the data from the notebook (include derived columns):**

```python
out = fin.copy()
out["Bulan"] = out["Date"].dt.to_period("M").astype(str)
out.to_csv("data_dashboard.csv", index=False)
```

**2.** Power BI Desktop → **Get Data → Text/CSV** → select `data_dashboard.csv` → **Load**.

**3. Create measures** (**New measure**):

```dax
Total Shipments       = COUNTROWS(global_supply_chain_risk_2026)
Disrupted Shipments   = CALCULATE([Total Shipments], global_supply_chain_risk_2026[Disruption_Occurred] = 1)
Disruption Rate %     = AVERAGE(global_supply_chain_risk_2026[Disruption_Occurred])
Avg Lead Time Days    = AVERAGE(global_supply_chain_risk_2026[Lead_Time_Days])
Median Lead Time Days = MEDIAN(global_supply_chain_risk_2026[Lead_Time_Days])
P90 Lead Time Days    = PERCENTILE.INC(global_supply_chain_risk_2026[Lead_Time_Days], 0.9)
Total Loss USD        = SUM(global_supply_chain_risk_2026[Kerugian_USD])
```

**4. Conditional colors:** **Format → Bars → Colors → fx → Rules** based on `Disruption Rate %` (unit **Number**): `≥ 0.7` red-orange, `≥ 0.5` amber, below that green.

**5. Sort categories** (geopolitical, carrier, distance bands) with **Sort by column** so they run Low → High.

**6. Polish:** neutral page background (`#F3F5F9`), white rounded cards, one font family (Segoe UI), slicers in a top row, tile style for Transport Mode and Year.

---

## 📁 Repository Structure

```
.
├── README.md
├── Project_supply_risk_revisi_versi1.ipynb   # full analysis
├── data/
│   ├── global_supply_chain_risk_2026.csv     # raw data
│   └── data_dashboard.csv                    # processed data for Power BI
├── dashboard/
│   └── supply_chain_risk.pbix                # Power BI file
└── images/
    └── dashboard.png                         # dashboard screenshot
```

> Adjust this tree to match your actual folders.

---

## 🚀 Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/<username>/<repo-name>.git
cd <repo-name>
```

**2. Install dependencies**

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

**3. Set the data path** in the notebook's configuration cell:

```python
DATA_PATH = "data/global_supply_chain_risk_2026.csv"
```

**4. Run the notebook**

```bash
jupyter notebook Project_supply_risk_revisi_versi1.ipynb
```

Choose **Run All**. Every number in the notebook is generated from the data, and the recommendation text adapts to the test results.

---

## ✅ Recommendations

1. **Extreme-weather playbook.** For Storm and Hurricane, reschedule shipments and hold safety stock. For Fog and Rain, add a lead-time buffer from the P90 table.
2. **Carrier selection.** Assign reliable carriers to high-value lanes. The benefit holds in every weather condition (no significant interaction).
3. **Geopolitical monitoring.** For high-risk scores, prepare alternative routes and split large shipments.
4. **Focus the mitigation budget** on Hurricane and Storm: a small share of shipments, but most of the loss.

---

## ⚠️ Limitations

- The analysis is **associational**, not causal.
- Loss estimates depend on **assumed costs**; replace them with real figures for accurate results.
- Geopolitical group cut-offs (4 and 7.5) are initial assumptions; results are also tested on the continuous score so they do not depend on those cut-offs.
- Hurricane is 100% disrupted (*perfect separation*), so it is combined with Storm in the regression.
- The data has no actual costs, contractual SLAs, or carrier identities.

---

## 🔭 Future Work

- [ ] Per-shipment disruption prediction model (logistic regression or gradient boosting) with cross-validation
- [ ] Real-time weather forecast integration for early warnings
- [ ] Second dashboard page: savings simulation for switching carriers
- [ ] Scheduled data refresh in Power BI Service

---

## 👤 Author

**Herlina** · [LinkedIn](https://linkedin.com/in/username](https://www.linkedin.com/in/herlina-st-cplm/) · [GitHub](https://github.com/herlinahl779-wq)

If this project helped you, consider giving it a ⭐.

## 📄 License

Released under the [MIT License](LICENSE). The data is illustrative and intended for learning purposes only.
