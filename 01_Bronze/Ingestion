# 01_Bronze/bronze_ingestion.py
# Purpose: Ingest raw earthquake data from USGS API into Bronze layer
# Backdated from: August 3, 2025 to August 28, 2026

import requests
import json
from datetime import datetime, timedelta

def fetch_earthquake_data(start_date_str="2025-08-03", end_date_str="2026-08-28"):
    """
    Fetch earthquake data from USGS API for a specific date range.
    Returns raw JSON response.
    """
    url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
    params = {
        "format": "geojson",
        "starttime": start_date_str,
        "endtime": end_date_str,
        "minmagnitude": 1.0,
        "orderby": "time"
    }
    
    try:
        response = requests.get(url, params=params)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"API request failed: {e}")
        return None

def save_to_bronze(data, filename="raw_quakes_2025_2026.json"):
    """
    Save raw JSON data to Bronze layer.
    """
    if not data:
        print("⚠️ No data received from API")
        return
    
    features = data.get('features', [])
    print(f"✅ Received {len(features)} earthquake events")
    
    # Save to local file (adapt for Fabric Lakehouse later)
    with open(filename, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=2)
    
    print(f"✅ Saved to {filename}")
    return len(features)

if __name__ == "__main__":
    print("🌍 Fetching earthquake data from USGS API...")
    print("📅 Date Range: 2025-08-03 to 2026-08-28")
    
    raw_data = fetch_earthquake_data()
    event_count = save_to_bronze(raw_data)
    
    if event_count:
        print(f"🎉 Successfully ingested {event_count} earthquakes into Bronze layer!")
    else:
        print("❌ Failed to ingest data")
