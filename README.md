🔍 Layer Breakdown
Layer	Technology	Key Operations	Output
Bronze	API + Lakehouse	Raw JSON ingestion, metadata capture, immutable storage	raw_quakes.json
Silver	PySpark / Delta	GeoJSON flattening, timestamp normalization, null handling, deduplication	clean_quakes
Gold	SQL / Delta	Magnitude categorization, time-series aggregation, geographic slicing	kpi_magnitude, geo_hotspots
Semantic	Fabric Semantic Model	DAX measures, relationship modeling, row-level security	Model.pbix
Report	Power BI	Interactive maps, trend lines, KPI cards, drill-through pages	Dashboard
🛠️ Tech Stack
Category	Technology
Cloud Platform	Microsoft Fabric (Lakehouse, Notebooks, Pipelines)
Data Processing	PySpark (Apache Spark), Delta Lake
Data Source	USGS Earthquake API (REST)
Analytics	Power BI (DAX, Custom Visuals)
Orchestration	Fabric Data Pipeline (Scheduled Triggers)
Version Control	Git, GitHub
Language	Python 3.9+, SQL
📊 Key Insights & Dashboard Features
The final Power BI dashboard empowers stakeholders with:

🗺️ Global Heatmap: Real-time visualization of earthquake epicenters with depth-based sizing.
📈 Magnitude Trends: Time-series analysis showing frequency of Minor, Light, Moderate, and Major quakes.
📉 Depth Correlation: Analysis of earthquake depth vs. frequency to identify deep-seismic vs. shallow events.
🌍 Regional Risk: Top 10 most active countries/regions with drill-down capabilities.
⚡ Live KPIs: Total Events, Max Magnitude, Average Depth, and Recent Activity Alerts.
📂 Repository Structure
text

Copy
SOBZ-Earthquake-Analytics/
│
├── 01_Bronze/                # Raw ingestion logic & API connectors
├── 02_Silver/                # Data cleaning, transformation, and validation
├── 03_Gold/                  # Aggregated tables and KPI definitions
├── 04_Semantic_Model/        # DAX measures and relationship diagrams
├── 05_PowerBI/               # Dashboard screenshots and .pbix files
├── 06_Pipeline/              # Orchestration scripts and scheduling config
├── 07_Documentation/         # Data dictionary, architecture diagrams
├── notebooks/                # Exported Fabric Notebooks (.ipynb)
├── sql/                      # Gold layer SQL queries
├── .gitignore                # Git ignore rules
└── README.md                 # This file
🚀 How to Reproduce
1️⃣ Prerequisites
Microsoft Fabric Capacity (F64 or higher recommended)
Python 3.9+ environment (optional for local testing)
Git installed
2️⃣ Setup & Ingestion
bash

Copy
# Clone the repository
git clone https://github.com/YOUR_USERNAME/SOBZ-Earthquake-Analytics.git
cd SOBZ-Earthquake-Analytics

# (Optional) Run locally if you have a Fabric workspace ID
# python notebooks/bronze_ingestion.py
3️⃣ Deployment in Fabric
Create Lakehouse: Name it SOBZ_Lakehouse.
Ingest Bronze: Run Notebook_Bronze to pull data from USGS API.
Transform Silver: Run Notebook_Silver to clean and flatten data.
Aggregate Gold: Execute gold_queries.sql or run Notebook_Gold.
Build Dashboard: Connect Power BI to Gold tables and apply DAX measures.
4️⃣ Automation
Configure a Fabric Pipeline to trigger the Bronze → Silver → Gold flow every 6 hours.
📈 Data Quality & Performance
Validation: 100% of records validated for coordinate bounds (-180 to 180, -90 to 90).
Deduplication: Automatic removal of duplicate API responses using event_id.
Null Handling: Critical fields (magnitude, depth) imputed or flagged.
Scalability: Delta Lake format ensures ACID transactions and efficient upserts.
📸 Dashboard Preview
(Add screenshots of your Power BI dashboard here later)

Tip: Upload 2-3 high-res screenshots to the 05_PowerBI/screenshots/ folder and link them here.

👨‍💻 Author
[Your Name]
🔗 LinkedIn Profile
📧 [Email Address]
📍 [Location]

Built with ❤️ for Data Engineering Excellence.

🙏 Acknowledgments
USGS for providing open, real-time seismic data.
Microsoft for the powerful Fabric platform.
Apache Spark community for the open-source engine.
