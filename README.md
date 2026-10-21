<div align="center">

# 🏎️💨 F1 Race Strategy Lakehouse

**Turning raw lap times into winning pit-wall decisions** 🏁

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADD4?style=for-the-badge&logo=delta&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Notebooks-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-In_Progress-yellow?style=for-the-badge)

</div>

---

## 📖 About

A data lakehouse on **Databricks** for analysing Formula 1 race strategy, built with **PySpark** and **Delta Lake** using the **medallion architecture** (🥉 Bronze → 🥈 Silver → 🥇 Gold). It ingests race results and lap-by-lap timing data from the **[Jolpica F1 API](https://github.com/jolpica/jolpica-f1)**, then cleans and models them for strategy analysis such as pace, stints and pit stop impact. ⏱️🛞

## 🏗️ Architecture

```
  🌐 Jolpica F1 API
        │
        ▼
┌──────────┐     ┌──────────┐     ┌──────────┐
│ 🥉 BRONZE │ ──▶ │ 🥈 SILVER │ ──▶ │ 🥇 GOLD   │
│ raw JSON │     │ cleaned  │     │ strategy │
│ → Delta  │     │ typed    │     │ metrics  │
└──────────┘     └──────────┘     └──────────┘
                                   (planned 🚧)
```

| Layer | Purpose | Tables |
|-------|---------|--------|
| 🥉 **Bronze** | Raw API responses landed as Delta tables, minimal transformation | 🏆 race results, ⏱️ lap data |
| 🥈 **Silver** | Cleaned, typed, deduplicated and validated data | 🏆 race results, ⏱️ lap times |
| 🥇 **Gold** | Aggregated, analysis-ready strategy metrics | 🚧 *planned* |

## 📂 Repository Structure

Notebooks are numbered and meant to be run **in order**. ▶️

| | Notebook | Description |
|---|----------|-------------|
| 🔥 | `00_pyspark_basics.ipynb` | PySpark fundamentals and warm-up exercises |
| 🏠 | `01_lakehouse_setup.ipynb` | Creates the lakehouse environment: schemas and storage layout |
| 🥉 | `02_bronze_api_ingestion.ipynb` | Ingests race results from the Jolpica API into Bronze |
| 🥈 | `03_silver_race_results.ipynb` | Cleans and transforms race results into Silver |
| 🥉 | `04_bronze_lap_ingestion.ipynb` | Ingests lap-level timing data into Bronze |
| 🥈 | `05_silver_lap_times.ipynb` | Cleans and transforms lap times into Silver |

## 🛠️ Tech Stack

| | Component | Details |
|---|-----------|---------|
| ☁️ | **Platform** | Databricks |
| ⚡ | **Processing** | PySpark |
| 🔺 | **Storage** | Delta Lake |
| 🌐 | **Data source** | [Jolpica F1 API](https://github.com/jolpica/jolpica-f1) (open-source successor to the Ergast API) |
| 🐍 | **Language** | Python |
| 📓 | **Interface** | Databricks notebooks |

## 🚀 Getting Started

### ✅ Prerequisites

- ☁️ A Databricks workspace (the [Free Edition](https://www.databricks.com/learn/free-edition) works)
- 🖥️ A running cluster or serverless compute
- 🌍 Outbound internet access from the workspace to reach the Jolpica API (no API key required)

### 📦 Setup

1. 🍴 Clone this repo into your workspace using **Workspace → Create → Git folder** and paste:
```
   https://github.com/AdhishDatir/f1-race-strategy-lakehouse.git
```
2. 🔌 Attach the notebooks to your compute.
3. 🏠 Run `01_lakehouse_setup.ipynb` to create the schemas.

### ▶️ Run

Run the notebooks in order:

1. 🏠 `01_lakehouse_setup.ipynb` to create the lakehouse structure
2. 🥉 `02_bronze_api_ingestion.ipynb` and `04_bronze_lap_ingestion.ipynb` to load raw data
3. 🥈 `03_silver_race_results.ipynb` and `05_silver_lap_times.ipynb` to build the Silver tables

> 💡 `00_pyspark_basics.ipynb` is optional background reading.

> ⚠️ **API etiquette:** Jolpica is a free community-run service with rate limits. Keep ingestion polite: add small delays between requests and avoid re-downloading data you already have.

## 🗺️ Roadmap

- [ ] 🥇 Gold layer: stint analysis, pace degradation, pit stop windows
- [ ] 🛑 Pit stop ingestion (Jolpica `pitstops` endpoint)
- [ ] ✅ Data quality checks
- [ ] ⏰ Orchestration with Databricks Workflows / Jobs
- [ ] 📊 Dashboards (Databricks SQL / AI/BI) for race strategy

## 🤝 Contributing

Issues and pull requests are welcome! 🙌

1. 🍴 Fork the repo
2. 🌿 Create a feature branch
3. 💾 Commit your changes
4. 📬 Open a pull request

## 🙏 Acknowledgements

- 🏁 [Jolpica F1](https://github.com/jolpica/jolpica-f1) for the open F1 data API
- 🕰️ The Ergast project, which Jolpica continues


## 👤 Author

**Adhish Datir**

[![GitHub](https://img.shields.io/badge/GitHub-AdhishDatir-181717?style=flat-square&logo=github)](https://github.com/AdhishDatir)

---

<div align="center">

⭐ **Star this repo if you found it useful!** ⭐

🏁 *Lights out and away we go!* 🏁

</div>
