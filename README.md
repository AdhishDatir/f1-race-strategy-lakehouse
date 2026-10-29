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

A data lakehouse on **Databricks** for analysing Formula 1 race strategy, built with **PySpark** and **Delta Lake** using the **medallion architecture** (🥉 Bronze → 🥈 Silver → 🥇 Gold). It ingests race results, lap-by-lap timing, and pit-stop data from the **[Jolpica F1 API](https://github.com/jolpica/jolpica-f1)**, enriches stints with tyre compounds from **[OpenF1](https://openf1.org/docs/)**, and models them for pace, stint, degradation, and pit-stop analysis. ⏱️🛞

## 🏗️ Architecture

```
  🌐 Jolpica F1 API ───────┐
  🌐 OpenF1 API ───────────┤
                           ▼
                    ┌──────────┐     ┌──────────┐     ┌──────────┐
                    │ 🥉 BRONZE │ ──▶ │ 🥈 SILVER │ ──▶ │ 🥇 GOLD   │
                    │ raw JSON │     │ cleaned  │     │ strategy │
                    │ → Delta  │     │ typed    │     │ metrics  │
                    └──────────┘     └──────────┘     └──────────┘
                                                        (in progress 🚧)
```

| Layer | Purpose | Tables |
|-------|---------|--------|
| 🥉 **Bronze** | Raw API responses landed as Delta tables with source metadata | 🏆 race results, ⏱️ lap data, 🛞 pit stops, 🏁 OpenF1 stints |
| 🥈 **Silver** | Cleaned, typed, deduplicated, validated data | 🏆 race results, ⏱️ lap times, 🛞 pit stops |
| 🥇 **Gold** | Aggregated, analysis-ready strategy metrics | 🧩 driver stint metrics, pace and degradation (in progress) |

## 📂 Repository Structure

Notebooks are numbered and meant to be run **in order**. ▶️

| | Notebook | Description |
|---|----------|-------------|
| 🔥 | `00_pyspark_basics.ipynb` | PySpark fundamentals and warm-up exercises |
| 🏠 | `01_lakehouse_setup.ipynb` | Creates the lakehouse environment: schemas and storage layout |
| 🥉 | `02_bronze_api_ingestion.ipynb` | Ingests race results from the Jolpica API into Bronze |
| 🥈 | `03_silver_race_results.ipynb` | Cleans and transforms race results into Silver |
| 🥉 | `04_bronze_lap_ingestion.ipynb` | Ingests paginated lap-level timing data into Bronze |
| 🥈 | `05_silver_lap_times.ipynb` | Cleans and transforms lap times into Silver |
| 🥉 | `06_bronze_pit_stop_ingestion.ipynb` | Ingests raw pit-stop data into Bronze |
| 🥈 | `07_silver_pit_stops.ipynb` | Cleans and standardizes pit stops into Silver |
| 🥇 | `08_gold_stints.ipynb` | Uses window functions to infer stint boundaries and metrics |
| 🛞 | `09_openf1_tyre_stints.ipynb` | Matches OpenF1 sessions and enriches stints with compounds |

## 🛠️ Tech Stack

| | Component | Details |
|---|-----------|---------|
| ☁️ | **Platform** | Databricks |
| ⚡ | **Processing** | PySpark |
| 🔺 | **Storage** | Delta Lake |
| 🌐 | **Data sources** | [Jolpica F1 API](https://github.com/jolpica/jolpica-f1) and [OpenF1](https://openf1.org/docs/) |
| 🐍 | **Language** | Python |
| 📓 | **Interface** | Databricks notebooks |
| 🧮 | **Analytics** | Spark SQL and PySpark window functions |

## 🚀 Getting Started

### ✅ Prerequisites

- ☁️ A Databricks workspace (the [Free Edition](https://www.databricks.com/learn/free-edition) works)
- 🖥️ Serverless compute or a running Spark cluster
- 🌍 Outbound internet access from the workspace to reach Jolpica and OpenF1 (no API key required for the historical endpoints used here)

### 📦 Setup

1. 🍴 Clone this repo into your workspace using **Workspace → Create → Git folder** and paste:
```
   https://github.com/AdhishDatir/f1-race-strategy-lakehouse.git
```
2. 🔌 Attach the notebooks to your compute.
3. 🏠 Run `01_lakehouse_setup.ipynb` to create `f1_bronze`, `f1_silver`, and `f1_gold` schemas.

### ▶️ Run

Run the notebooks in order:

1. 🔥 `00_pyspark_basics.ipynb` for the learning exercises
2. 🏠 `01_lakehouse_setup.ipynb` to create the lakehouse structure
3. 🥉 `02_bronze_api_ingestion.ipynb` and `04_bronze_lap_ingestion.ipynb` to load raw results and laps
4. 🥈 `03_silver_race_results.ipynb` and `05_silver_lap_times.ipynb` to build clean Silver tables
5. 🥉 `06_bronze_pit_stop_ingestion.ipynb` and 🥈 `07_silver_pit_stops.ipynb` to add pit-stop data
6. 🥇 `08_gold_stints.ipynb` to infer stint boundaries and calculate pace metrics
7. 🛞 `09_openf1_tyre_stints.ipynb` to validate the matching race session and enrich compounds

> 💡 `00_pyspark_basics.ipynb` is optional background reading.

> ⚠️ **API etiquette:** Jolpica and OpenF1 are community-run services with usage limits. Keep ingestion polite, use the correct race/session keys, and avoid re-downloading data unnecessarily.

## ✅ Current Outputs

```text
f1_bronze.api_responses
  ├── race_results
  ├── lap_times
  ├── pit_stops
  └── OpenF1 stint payloads

f1_silver
  ├── race_results       one row per driver per race
  ├── lap_times          one row per driver per lap
  └── pit_stops          one row per driver per stop

f1_gold.driver_stint_metrics
  └── stint boundaries, pace metrics, and compound enrichment in progress
```

## 🗺️ Roadmap

- [x] 🥉 Bronze ingestion for race results, lap times, and pit stops
- [x] 🥈 Silver race-result, lap-time, and pit-stop tables
- [x] ✅ Data-quality checks for duplicates, missing values, invalid laps, and durations
- [x] 🧩 Window-function stint boundary identification
- [x] 🛞 OpenF1 session matching and tyre-compound enrichment
- [ ] 📉 Tyre degradation and representative-lap metrics
- [ ] 🏎️ Undercut/overcut outcome analysis
- [ ] 🌦️ OpenF1 weather and safety-car/race-control enrichment
- [ ] ⏰ Orchestration with Databricks Workflows / Jobs
- [ ] 📊 Dashboards (Databricks SQL / AI/BI) for race strategy

## 🤝 Contributing

Issues and pull requests are welcome! 🙌

1. 🍴 Fork this repo
2. 🌿 Create a feature branch
3. 💾 Commit your changes
4. 📬 Open a pull request

## 🙏 Acknowledgements

- 🏁 [Jolpica F1](https://github.com/jolpica/jolpica-f1) for the open F1 data API
- 🛞 [OpenF1](https://openf1.org/docs/) for stint, compound, weather, and race-control data
- 🕰️ The Ergast project, which Jolpica continues

## 👤 Author

**Adhish Datir**

[![GitHub](https://img.shields.io/badge/GitHub-AdhishDatir-181717?style=flat-square&logo=github)](https://github.com/AdhishDatir)

---

<div align="center">

⭐ **Star this repo if you found it useful!** ⭐

🏁 *Lights out and away we go!* 🏁

</div>
