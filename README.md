# 🌍 SOBZ Earthquake Analytics
### End-to-End Data Engineering & BI Solution using Microsoft Fabric

[![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/fabric/)
[![Power BI](https://img.shields.io/badge/Microsoft%20Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![PySpark](https://img.shields.io/badge/PySpark-EA7523?style=for-the-badge&logo=apache&logoColor=white)](https://spark.apache.org/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/Status-Production_Ready-green)](https://github.com/)

> **A production-grade data engineering pipeline ingesting real-time USGS earthquake data, transforming it via a Medallion Architecture in Microsoft Fabric, and delivering interactive business intelligence via Power BI.**

---

## 🚀 Project Overview

This project demonstrates a complete **ETL/ELT pipeline** built in **Microsoft Fabric**, transforming raw seismic data from the **USGS Earthquake API** into actionable business insights. 

By leveraging the **Medallion Architecture** (Bronze → Silver → Gold), we ensure data quality, scalability, and separation of concerns. The final layer feeds a semantic model and an interactive **Power BI dashboard**, allowing stakeholders to monitor global seismic activity, analyze magnitude trends, and assess geographic risks.

### 🎯 Business Objectives
- **Real-Time Monitoring:** Ingest and visualize global earthquake data with minimal latency.
- **Risk Assessment:** Categorize earthquakes by magnitude and depth to identify high-risk zones.
- **Trend Analysis:** Identify temporal patterns in seismic activity to support scientific and safety reporting.
- **Data Quality:** Implement rigorous validation, cleaning, and deduplication in the Silver layer.

---

## 🏗️ Architecture & Design

The solution follows a robust **Medallion Architecture** within Microsoft Fabric Lakehouse:

```mermaid
graph LR
    A[USGS API] -->|Raw JSON| B(Bronze Layer)
    B -->|Flatten & Clean| C(Silver Layer)
    C -->|Aggregates & KPIs| D(Gold Layer)
    D -->|Semantic Model| E[Power BI Dashboard]
    
    style A fill:#f9f9f9,stroke:#333,stroke-width:2px
    style B fill:#e3f2fd,stroke:#1976d2
    style C fill:#e8f5e9,stroke:#388e3c
    style D fill:#fff3e0,stroke:#f57c00
    style E fill:#fce4ec,stroke:#c2185b
