Spotify Song Popularity Analysis — Power BI Dashboard

Project Overview
This project explores what makes a song popular on Spotify through data-driven analysis.  
It analyzes over **3,900 songs** from Spotify’s API, comparing high and low popularity tracks to uncover how audio traits like **energy**, **danceability**, and **valence** influence success.  
Using **Excel for cleaning** and **Power BI for modeling and visualization**, the dashboard decodes the key factors behind musical popularity and builds a predictive model to estimate song success.

Objectives
- Identify audio features that most strongly affect popularity.  
- Compare genre trends and emotional tone (valence) across years.  
- Build and visualize a weighted prediction model for popularity.  
- Present insights interactively through Power BI dashboards.

Dataset Description
- **Source:** Spotify API (via Kaggle dataset)  
- **Data Volume:** 3,900 total songs (≈1,300 popular + 2,600 less popular)  
- **Core Columns:**
  - `energy`, `danceability`, `valence`, `loudness`, `instrumentalness`
  - `speechiness`, `tempo`, `track_popularity`, `genre`, `release_date`
- **Files Included:**
  - `high_popularity_spotify_data_cleaned.csv`
  - `low_popularity_spotify_data_cleaned.csv`

Data Cleaning & Preparation
- Cleaned and normalized data using **Microsoft Excel**.  
- Removed special characters and standardized date formats.  
- Deleted redundant columns and rows.  
- Unified datasets (high and low popularity) and appended them in Power BI.  
- Added a “Popularity Type” column to distinguish the two groups.

Data Modeling (Power BI)
- Combined both datasets into a single model (`Spotify_Data`).  
- Created calculated columns for `Year`, `Decade`, and `Popularity Type`.  
- Added a **Date Table** for time intelligence (YTD, Rolling Avg).  
- Built DAX measures for averages, predictive weights, and feature ranking.

DAX Measures Used
| Measure | Purpose |
|----------|----------|
| `Avg_Popularity` | Mean popularity across all songs |
| `Avg_Energy`, `Avg_Danceability`, `Avg_Valence`, `Avg_Loudness` | Core feature averages |
| `Rolling_3M_Avg`, `Rolling_6M_Avg` | Time-based smoothing |
| `Predicted_Popularity` | Weighted heuristic model |
| `Top_Genre` / `Most_Influential_Feature` | Dynamic summary insights |

Predictive Model (Heuristic DAX)
```DAX
Predicted_Popularity =
(
    (AVERAGE(Spotify_Data[energy]) * 0.25) +
    (AVERAGE(Spotify_Data[danceability]) * 0.20) +
    (AVERAGE(Spotify_Data[valence]) * 0.30) +
    ((1 - AVERAGE(Spotify_Data[instrumentalness])) * 0.15) +
    (DIVIDE(AVERAGE(Spotify_Data[loudness]) + 60, 60) * 0.10)
) * 100
