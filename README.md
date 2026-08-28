# 🌍 SOBZ Earthquake Analytics

### End-to-End Earthquake Data Engineering & Analytics Platform

[![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-Data%20Engineering-blue)](https://learn.microsoft.com/en-us/fabric/)
[![PySpark](https://img.shields.io/badge/PySpark-Data%20Processing-orange)](https://spark.apache.org/docs/latest/api/python/)
[![Power BI](https://img.shields.io/badge/Power%20BI-Analytics-yellow)](https://powerbi.microsoft.com/)
[![USGS](https://img.shields.io/badge/Data%20Source-USGS-green)](https://earthquake.usgs.gov/)
[![Architecture](https://img.shields.io/badge/Architecture-Medallion-purple)](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)

> **A production-style data engineering project that ingests earthquake event data from the USGS Earthquake Catalog API, transforms it through a Microsoft Fabric Lakehouse using Medallion Architecture, and delivers analytics-ready data for Power BI reporting.**

---

## 📌 Project Overview

Earthquakes generate large volumes of geospatial and time-series data. Turning these raw events into reliable analytical information requires more than simply downloading data.

This project demonstrates an end-to-end modern data platform for collecting, processing, transforming, modelling, and visualizing earthquake events.

The solution uses the **USGS Earthquake Catalog API** as its primary data source and implements a **Bronze → Silver → Gold Medallion Architecture** within Microsoft Fabric.

The final solution is designed to answer questions such as:

* How many earthquakes occurred during a selected period?
* Where are earthquake events concentrated?
* How does earthquake activity change over time?
* What magnitude ranges occur most frequently?
* Which locations experience the strongest earthquakes?
* How deep are the recorded earthquake events?
* How does earthquake activity vary geographically?
* What trends can be identified from historical earthquake data?

---

# 🎯 Project Objectives

The primary objectives of this project are to:

1. Build an end-to-end data engineering pipeline using Microsoft Fabric.
2. Ingest earthquake data directly from the USGS API.
3. Preserve raw source data in a Bronze layer.
4. Clean, validate, standardize, and enrich the data in a Silver layer.
5. Create analytics-ready Gold datasets.
6. Develop a semantic model for analytical consumption.
7. Build an interactive Power BI dashboard.
8. Implement pipeline orchestration and automation.
9. Apply data engineering best practices including data quality, lineage, reproducibility, and layered architecture.
10. Demonstrate practical skills relevant to modern data engineering and the **Microsoft Fabric DP-700** learning path.

---

# 🏗️ Solution Architecture

```text
                         ┌──────────────────────────┐
                         │     USGS EARTHQUAKE      │
                         │          API             │
                         └────────────┬─────────────┘
                                      │
                                      │ API Ingestion
                                      ▼
                         ┌──────────────────────────┐
                         │      BRONZE LAYER        │
                         │                          │
                         │ Raw JSON / Source Data  │
                         │ Original Records        │
                         └────────────┬─────────────┘
                                      │
                                      │ PySpark
                                      │ Validation
                                      ▼
                         ┌──────────────────────────┐
                         │      SILVER LAYER        │
                         │                          │
                         │ Cleaned Data             │
                         │ Standardized Data        │
                         │ Deduplicated Data        │
                         │ Enriched Attributes      │
                         └────────────┬─────────────┘
                                      │
                                      │ Aggregation
                                      │ Business Logic
                                      ▼
                         ┌──────────────────────────┐
                         │       GOLD LAYER         │
                         │                          │
                         │ Curated Tables            │
                         │ Analytical Datasets       │
                         │ KPI Tables                │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │     SEMANTIC MODEL       │
                         │                          │
                         │ Relationships             │
                         │ Measures                  │
                         │ Business Logic            │
                         └────────────┬─────────────┘
                                      │
                                      ▼
                         ┌──────────────────────────┐
                         │       POWER BI           │
                         │                          │
                         │ Interactive Dashboard    │
                         │ Maps & KPIs               │
                         │ Trends & Analysis         │
                         └──────────────────────────┘
```

This architecture follows the principle of progressively improving data quality as it moves through Bronze, Silver, and Gold layers. Microsoft describes Bronze as raw data, Silver as validated/enriched data, and Gold as curated data optimized for analytics.

---

# 🔌 Data Source

## United States Geological Survey — USGS

The project uses the **USGS Earthquake Catalog API** to retrieve earthquake event information.

The USGS event service supports configurable queries including start time, end time, magnitude and output format. The project uses **GeoJSON** because it provides structured event information together with geographic coordinates.

**Source:**

https://earthquake.usgs.gov/fdsnws/event/1/

### Example API request

```text
https://earthquake.usgs.gov/fdsnws/event/1/query
?format=geojson
&starttime={start_date}
&endtime={end_date}
```

### Key information available from the API

* Earthquake event ID
* Magnitude
* Magnitude type
* Location / place
* Event timestamp
* Longitude
* Latitude
* Depth
* Event status
* Tsunami indicator
* Number of stations
* Felt reports
* Significance
* USGS event URL

---

# 🥉 Bronze Layer — Raw Ingestion

The Bronze layer is the landing zone for source data.

The objective is to preserve the data as close as possible to its original form before applying transformations.

### Bronze responsibilities

* Connect to the USGS API
* Dynamically specify the extraction period
* Retrieve earthquake events
* Receive JSON / GeoJSON data
* Store raw source records
* Preserve source information for traceability
* Support repeatable ingestion

### Example ingestion flow

```text
Data Factory Pipeline
        │
        ▼
Start Date + End Date
        │
        ▼
USGS API
        │
        ▼
Python / Requests
        │
        ▼
Raw JSON
        │
        ▼
Fabric Lakehouse
        │
        ▼
Bronze
```

### Example Python approach

```python
import requests
import json

url = f"https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime={start_date}&endtime={end_date}"

response = requests.get(url)
response.raise_for_status()

data = response.json()

earthquake_data = data["features"]
```

The raw data is then written into the Lakehouse Files area for downstream processing.

---

# 🥈 Silver Layer — Data Transformation

The Silver layer transforms raw Bronze data into reliable analytical records.

This is where data quality and standardization become central.

### Silver processing includes

* Reading Bronze JSON files
* Flattening nested GeoJSON structures
* Extracting earthquake properties
* Extracting geographic coordinates
* Standardizing column names
* Converting timestamps
* Casting numeric fields
* Handling missing values
* Removing duplicate events
* Validating latitude and longitude
* Validating magnitude and depth
* Creating analytical attributes

### Example transformation

```text
Bronze JSON
     │
     ▼
Read with PySpark
     │
     ▼
Flatten nested structures
     │
     ▼
Extract properties
     │
     ▼
Extract coordinates
     │
     ▼
Clean + Validate
     │
     ▼
Deduplicate
     │
     ▼
Silver Delta Table
```

### Example PySpark

```python
from pyspark.sql.functions import col

df = spark.read \
    .option("multiline", "true") \
    .json(bronze_path)

silver_df = df.select(
    col("id"),
    col("properties.mag").alias("magnitude"),
    col("properties.place").alias("location"),
    col("properties.time").alias("event_time"),
    col("properties.status").alias("status"),
    col("geometry.coordinates")[0].alias("longitude"),
    col("geometry.coordinates")[1].alias("latitude"),
    col("geometry.coordinates")[2].alias("depth")
)
```

---

# 🥇 Gold Layer — Analytics-Ready Data

The Gold layer contains curated datasets designed for analytics and reporting.

Instead of exposing complex raw structures to business users, Gold provides clean, understandable datasets optimized for analytical questions.

### Potential Gold datasets

```text
Gold
│
├── fact_earthquake
│
├── dim_date
│
├── dim_location
│
├── earthquake_daily_summary
│
├── earthquake_magnitude_summary
│
└── earthquake_geographic_summary
```

### Example analytical attributes

| Attribute          | Purpose                     |
| ------------------ | --------------------------- |
| Earthquake ID      | Unique event identification |
| Event Date         | Time analysis               |
| Event Time         | Detailed temporal analysis  |
| Magnitude          | Earthquake strength         |
| Magnitude Category | Classification              |
| Depth              | Depth analysis              |
| Latitude           | Geographic analysis         |
| Longitude          | Geographic analysis         |
| Location           | Human-readable location     |
| Tsunami Flag       | Tsunami-related analysis    |
| Significance       | Event importance            |

---

# 📊 Key KPIs

The Power BI solution will focus on meaningful earthquake indicators.

### Primary KPIs

**Total Earthquakes**

Number of earthquake events recorded during the selected period.

**Average Magnitude**

Average magnitude of recorded earthquake events.

**Maximum Magnitude**

Strongest earthquake recorded in the selected period.

**Average Depth**

Average depth of earthquake events.

**Significant Earthquakes**

Number of events meeting the defined significance threshold.

**Tsunami Events**

Number of earthquake events associated with a tsunami indicator.

---

# 📈 Power BI Dashboard

The Gold layer will feed an interactive Power BI report.

### Planned dashboard sections

#### 1. Executive Overview

* Total earthquakes
* Maximum magnitude
* Average magnitude
* Average depth
* Significant events

#### 2. Earthquake Trend

* Earthquakes over time
* Magnitude over time
* Daily / monthly activity
* Significant event trends

#### 3. Geographic Analysis

* Earthquake event map
* Latitude and longitude distribution
* Regional concentration
* High-magnitude locations

#### 4. Magnitude Analysis

* Magnitude distribution
* Magnitude categories
* Strongest events
* Magnitude trend

#### 5. Depth Analysis

* Depth distribution
* Magnitude vs depth
* Average depth by region
* Deepest earthquake events

---

# 🎛️ Dashboard Filters

Users will be able to interact with the data through slicers such as:

* Date
* Magnitude
* Magnitude category
* Location
* Region
* Depth
* Tsunami indicator
* Event status

---

# 🧠 Analytical Questions

The project is designed to investigate questions including:

### Temporal

* When does earthquake activity increase?
* Which periods have the highest number of events?
* Are there noticeable changes in earthquake frequency?

### Geographic

* Which areas experience the most earthquake activity?
* Where are high-magnitude events concentrated?
* Which regions have the deepest earthquakes?

### Magnitude

* What magnitude range occurs most frequently?
* How many major earthquake events occurred?
* Where are the strongest earthquakes located?

### Depth

* What is the typical earthquake depth?
* Are stronger earthquakes associated with specific depth ranges?
* Which locations experience deeper events?

---

# 🧪 Data Quality

Data quality is treated as an important part of the pipeline.

### Quality checks include

```text
✓ Null checks
✓ Duplicate checks
✓ Data type validation
✓ Latitude validation
✓ Longitude validation
✓ Magnitude validation
✓ Depth validation
✓ Timestamp validation
✓ Required-field validation
✓ Record-count validation
```

The objective is to prevent invalid or unreliable records from reaching the analytical layer.

---

# 🔄 Pipeline Orchestration

The project is designed around an orchestrated Fabric workflow.

```text
Pipeline Start
      │
      ▼
Set Extraction Parameters
      │
      ▼
Call USGS API
      │
      ▼
Store Bronze Data
      │
      ▼
Run Silver Transformation
      │
      ▼
Validate Silver
      │
      ▼
Run Gold Transformation
      │
      ▼
Refresh Analytical Model
      │
      ▼
Power BI
```

Future iterations can introduce scheduled execution, monitoring, retry handling, and incremental processing.

---

# 🛠️ Technology Stack

| Technology           | Purpose                           |
| -------------------- | --------------------------------- |
| **Microsoft Fabric** | Unified analytics platform        |
| **OneLake**          | Centralized data storage          |
| **Lakehouse**        | Data storage and analytics        |
| **Data Factory**     | Pipeline orchestration            |
| **PySpark**          | Data transformation               |
| **Python**           | API ingestion                     |
| **Spark SQL**        | Analytical transformations        |
| **Delta Lake**       | Reliable table storage            |
| **Semantic Model**   | Analytical data modelling         |
| **Power BI**         | Visualization and reporting       |
| **GitHub**           | Version control and documentation |
| **USGS API**         | Earthquake data source            |

Microsoft Fabric uses OneLake as its unified logical data lake, while Lakehouse solutions can combine storage, Spark processing, SQL querying, and BI consumption within the Fabric platform.

---

# 📂 Repository Structure

```text
SOBZ-Earthquake-Analytics/
│
├── 01_Bronze/
│   ├── README.md
│   └── bronze_ingestion.py
│
├── 02_Silver/
│   ├── README.md
│   └── silver_transformation.py
│
├── 03_Gold/
│   ├── README.md
│   └── gold_transformation.py
│
├── 04_Semantic_Model/
│   └── README.md
│
├── 05_PowerBI/
│   ├── README.md
│   └── screenshots/
│
├── 06_Pipeline/
│   └── README.md
│
├── 07_Documentation/
│   ├── architecture.png
│   └── data_dictionary.md
│
├── notebooks/
│   ├── Notebook_Bronze.ipynb
│   ├── Notebook_Silver.ipynb
│   └── Notebook_Gold.ipynb
│
├── sql/
│   └── gold_queries.sql
│
├── README.md
└── .gitignore
```

---

# 🔐 Data & Security Considerations

This repository should contain **code, documentation, and selected screenshots**, rather than private Fabric credentials or sensitive configuration.

Never commit:

```text
❌ API keys
❌ Passwords
❌ Access tokens
❌ Connection strings containing secrets
❌ Personal credentials
❌ Private workspace information
```

Where configuration is required, use environment variables or placeholder values.

---

# 📚 Project Learning Outcomes

This project demonstrates practical experience in:

* API data ingestion
* Data engineering
* Lakehouse architecture
* Medallion architecture
* Microsoft Fabric
* OneLake
* PySpark
* Spark SQL
* Data transformation
* Data quality
* Data modelling
* Pipeline orchestration
* Semantic modelling
* Power BI
* Git/GitHub
* Analytical storytelling

It also provides a practical implementation environment for concepts relevant to the **Microsoft Fabric DP-700 Data Engineer** certification.

---

# 🚀 Future Enhancements

Potential future improvements include:

* Automated scheduled ingestion
* Incremental API loading
* Pipeline failure notifications
* Advanced data quality monitoring
* Partition optimization
* Historical trend analysis
* Earthquake anomaly detection
* Automated data refresh
* Advanced geographic analysis
* Machine learning for earthquake classification
* Deployment across Development → Test → Production environments

---

# 📌 Project Status

| Component            | Status                |
| -------------------- | --------------------- |
| USGS API             | ✅ Connected           |
| Fabric Workspace     | ✅ Created             |
| Lakehouse            | ✅ Created             |
| Bronze Layer         | ✅ In Progress / Built |
| Silver Layer         | ✅ In Progress / Built |
| Gold Layer           | 🔄 Building           |
| Semantic Model       | 🔄 Planned            |
| Power BI Dashboard   | 🔄 Planned            |
| Pipeline Automation  | 🔄 Planned            |
| GitHub Documentation | 🔄 Building           |

> **Project status will be updated as each component is completed.**

---

# 🏆 Final Deliverable

The completed project will provide an end-to-end analytical platform:

```text
             🌎 USGS EARTHQUAKE DATA
                       │
                       ▼
              📥 API INGESTION
                       │
                       ▼
              🥉 BRONZE LAYER
                       │
                       ▼
              🥈 SILVER LAYER
                       │
                       ▼
               🥇 GOLD LAYER
                       │
                       ▼
             🧠 SEMANTIC MODEL
                       │
                       ▼
              📊 POWER BI REPORT
                       │
                       ▼
             💡 DATA-DRIVEN INSIGHTS
```

The goal is not simply to produce a dashboard.

The goal is to demonstrate the complete lifecycle of a modern data product — **from raw API data to trusted analytical insights.**

---

# 👤 Author

**Dumisani Sobahle**

Data Analytics | Data Engineering | Microsoft Fabric

### Areas of Interest

* Data Engineering
* Microsoft Fabric
* Lakehouse Architecture
* PySpark
* SQL
* Power BI
* Data Analytics
* Cloud Data Platforms

---

# 📖 References

* [USGS Earthquake Catalog API](https://earthquake.usgs.gov/fdsnws/event/1/)
* [Microsoft Fabric Documentation](https://learn.microsoft.com/en-us/fabric/)
* [Microsoft Fabric Medallion Architecture](https://learn.microsoft.com/en-us/fabric/onelake/onelake-medallion-lakehouse-architecture)
* [Microsoft Fabric Lakehouse](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-introduction)

---

⭐ **If you find this project useful, consider giving the repository a star.**
