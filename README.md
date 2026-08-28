# 🌍 SOBZ Earthquake Analytics
### End-to-End Data Engineering & BI Solution using Microsoft Fabric

<div align="center">

[![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/fabric/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![PySpark](https://img.shields.io/badge/PySpark-EA7523?style=for-the-badge&logo=apache&logoColor=white)](https://spark.apache.org/)
[![USGS API](https://img.shields.io/badge/Data-USGS%20Earthquake%20API-00A9E0?style=for-the-badge&logo=usgs&logoColor=white)](https://earthquake.usgs.gov/fdsnws/event/1/)

**A production-grade pipeline ingesting real-time seismic data, transforming it via Medallion Architecture, and delivering actionable insights.**

</div>

---

## 🚀 Project Overview

This project demonstrates a **full-stack data engineering lifecycle** built on **Microsoft Fabric**. It ingests raw earthquake data from the **USGS Earthquake API**, processes it through a robust **Bronze → Silver → Gold** Medallion Architecture, and visualizes critical seismic trends in an interactive **Power BI dashboard**.

### 💡 Why This Project Matters
- **Real-Time Risk Monitoring:** Provides immediate visibility into global seismic activity for safety and disaster response planning.
- **Data Integrity:** Implements rigorous cleaning, deduplication, and validation to ensure 100% data reliability.
- **Scalable Architecture:** Uses **Delta Lake** for ACID compliance, time travel, and seamless scaling from MBs to TBs.
- **Business Intelligence:** Transforms raw JSON into strategic KPIs (Magnitude Trends, Depth Analysis, Geographic Hotspots).

---

## 🏗️ Architecture & Workflow

The solution leverages the **Medallion Architecture** to ensure data quality at every stage.

```mermaid
flowchart TD
    A[🌐 USGS Earthquake API] -->|Raw JSON| B(🥉 Bronze Layer)
    B -->|Flatten & Validate| C(🥈 Silver Layer)
    C -->|Aggregate & Enrich| D(🥇 Gold Layer)
    D -->|Semantic Model| E(📊 Power BI Dashboard)
    
    style A fill:#f9f9f9,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5
    style B fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style C fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    style D fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style E fill:#fce4ec,stroke:#c2185b,stroke-width:2px
