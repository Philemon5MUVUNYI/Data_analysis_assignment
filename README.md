# Uber_Fare_Data_analysis_assignment

## 🎯 Project Objectives
- To analyze Uber fare patterns,ride durations, and temporal trends
- To create new analytical features (e.g., hour, day, time categories)
- To develop an interactive Power BI dashboard showing:
  
### 💵 Fare distribution
 1. histograms
 2. box plots
### 🚗 Ride patterns 
 1. Hourly
 2. Daily
 3. Monthly
    
### 🌦️ Seasonal and 🌍 geographic trends
- To highlight busiest periods and, if possible, assess weather impact
- To deliver actionable business insights through data storytelling

## Methodology:

#### 🗃️ 1. Data Collection
- Downloaded Uber Fares dataset from Kaggle
- Included:
   - fare amount
   - pickup time
   - geolocations

  #### 🧹 2. Data Cleaning (Python)
- Used Pandas to load and inspect data
  ```python
  import pandas as pd

  // Load the cleaned CSV from Phase 1
  df = pd.read_csv('uber_cleaned.csv')

  // Verify
  df.head()
  ```
  <img width="1734" height="472" alt="Loading the dataset and checking for the top 5 row" src="https://github.com/user-attachments/assets/f316bf84-95b6-49e6-9d53-638ce358f895" />


