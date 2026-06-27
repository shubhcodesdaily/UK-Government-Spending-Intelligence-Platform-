# GB UK Spending Intelligence Platform

A beginner-friendly end-to-end data pipeline that downloads UK Government spending data, cleans it, loads it into a PostgreSQL database, and produces visual analysis charts — all in a single Jupyter notebook.

---

## 📋 Table of Contents

- [What This Project Does](#what-this-project-does)
- [Project Structure](#project-structure)
- [Database Setup](#database-setup)
- [Output Files](#output-files)
- [Pipeline Architecture](#pipeline-architecture)
- [Important Security Note](#important-security-note)

---

## What This Project Does

This project follows a classic **ETL pipeline** (Extract → Transform → Load):

| Step | What Happens |
|------|-------------|
| **Extract** | Downloads UK government spending data (CRA 2017) directly from GOV.UK as an Excel file |
| **Transform** | Cleans column names, removes duplicates, fills missing values, and saves a clean CSV |
| **Load** | Uploads the clean data to a PostgreSQL database (AWS RDS) |
| **Analyse** | Runs exploratory data analysis (EDA) and saves 6 charts as PNG files |

The dataset covers **government spending across all UK departments, regions, and functions from 2012–2017**.

---

## Project Structure

```
uk-spending-intelligence/
│
├── notebooks/
│   └── 01_ingestion.ipynb      # Main notebook — run this
│
├── outputs/
│   └── plots/                  # Charts are saved here after running
│
├── .env.example                # Template for your database credentials
├── .gitignore                  # Tells git what NOT to upload
├── requirements.txt            # All Python packages you need
└── README.md                   # This file
```

---


Use AWS RDS (what the notebook is configured for)

1. Log in to [AWS Console](https://console.aws.amazon.com/)
2. Go to **RDS → Create database**
3. Choose **PostgreSQL**, select the free tier
4. Set a username and password
5. Copy the endpoint URL and paste it into your `.env` file

### Option B — Use a local PostgreSQL database (easier for beginners)

1. [Download PostgreSQL](https://www.postgresql.org/download/)
2. Install and start it
3. Create a database called `uk_spending`
4. Your `.env` `DB_URL` would then be:
   ```
   DB_URL=postgresql+psycopg2://postgres:your_password@localhost:5432/uk_spending
   ```

---

## Running the Notebook

### 1. Start Jupyter

```bash
jupyter notebook
```

Your browser will open automatically. If it doesn't, visit `http://localhost:8888`.

### 2. Open the notebook

Click on `notebooks/` → `01_ingestion.ipynb`

### 3. Run the cells

- Click **Kernel → Restart & Run All** to run everything from top to bottom
- Or press **Shift + Enter** to run one cell at a time

The notebook will:
1. Download the UK government Excel file (~20 MB)
2. Clean and save it as `clean_data.csv`
3. Upload it to your database
4. Generate and save 6 charts

> ⏱️ The Excel download may take 30–60 seconds depending on your internet speed.

---

## Output Files

After running, you will find these files in your project folder:

| File | Description |
|------|-------------|
| `clean_data.csv` | Cleaned version of the raw government data |
| `plot_01_spending_by_country.png` | Bar chart: total spending per UK country |
| `plot_02_top_departments.png` | Bar chart: top 10 spending departments |
| `plot_03_capital_vs_current.png` | Pie chart: capital vs current spending split |
| `plot_04_spending_trend.png` | Line chart: total spending per year (2012–2017) |
| `plot_05_by_function.png` | Bar chart: top 8 spending functions (e.g. health, education) |
| `plot_06_by_region.png` | Bar chart: spending by UK region |

---

## Pipeline Architecture

```
GOV.UK Excel file
      │
      ▼
  [EXTRACT]  ── Download Excel via URL
      │
      ▼
 [TRANSFORM] ── Clean columns, rename, fill nulls, deduplicate
      │
      ▼
   [LOAD]    ── Upload to PostgreSQL (AWS RDS or local)
      │
      ▼
   [EDA]     ── Generate 6 analytical charts

---

## Data Source

- **Dataset:** Country and Regional Analysis (CRA) 2017
- **Publisher:** HM Treasury, UK Government
- **Licence:** [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)
- **URL:** https://www.gov.uk/government/statistics/country-and-regional-analysis-2017
