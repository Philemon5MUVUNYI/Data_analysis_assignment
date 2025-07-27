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

  ```python
  # Convert 'pickup_datetime' to datetime (if not already done)
  df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'])

  # Extract features
  df['hour'] = df['pickup_datetime'].dt.hour          # Hour of day (0-23)
  df['day_of_week'] = df['pickup_datetime'].dt.dayofweek  # Monday=0, Sunday=6
  df['month'] = df['pickup_datetime'].dt.month        # Month (1-12)
  df['year'] = df['pickup_datetime'].dt.year          # Year

  # Check results
  df[['pickup_datetime', 'hour', 'day_of_week', 'month', 'year']].head()
  ```
  <img width="718" height="297" alt="Extracting some features" src="https://github.com/user-attachments/assets/565910fa-524d-4884-90ff-4ca4c354e6e2" />

  ```python
  # Define peak hours (e.g., 7-9 AM and 5-7 PM)
  df['is_peak'] = df['hour'].apply(lambda x: 1 if (7 <= x <= 9) or (17 <= x <= 19) else 0)

  # Label weekdays vs. weekends
  df['is_weekend'] = df['day_of_week'].apply(lambda x: 1 if x >= 5 else 0)  # 5=Sat, 6=Sun

  # Check counts
  print("Peak vs. Off-Peak Rides:")
  print(df['is_peak'].value_counts())
  ```
  <img width="320" height="141" alt="Defining peak hours and label weekdays and weekends" src="https://github.com/user-attachments/assets/ba1f22fe-a602-4197-ba24-394c264913ad" />

  ```python
   from math import radians, sin, cos, sqrt, atan2

  def haversine(lon1, lat1, lon2, lat2):
    # Convert degrees to radians
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])
    # Haversine formula
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))
    distance_km = 6371 * c  # Earth radius in km
    return distance_km

  # Calculate distance (ensure columns exist)
  if all(col in df.columns for col in ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude', 'dropoff_latitude']):
    df['trip_distance_km'] = df.apply(
        lambda row: haversine(
            row['pickup_longitude'], row['pickup_latitude'],
            row['dropoff_longitude'], row['dropoff_latitude']
        ), axis=1
    )
    print(df['trip_distance_km'].describe())
  else:
    print("Coordinate columns not found. Skipping distance calculation.")
  ```
  <img width="472" height="258" alt="Creating distance based features" src="https://github.com/user-attachments/assets/6d97f807-8cb2-42f1-bf17-3147b90302f5" />

```python
# Example: Convert payment type to dummy variables
if 'payment_type' in df.columns:
    df = pd.get_dummies(df, columns=['payment_type'], prefix='pay')
    df.head()
df.to_csv('uber_enhanced.csv', index=False)
print("Enhanced dataset saved successfully!")
```

```python
#Data cleaning 
#Keep positive fare
df = df[df['fare_amount'] > 0]
# Adjust threshold based on data
df = df[df['fare_amount'] < 100]

# Check results
df['fare_amount'].describe()
```
<img width="379" height="242" alt="Screenshot 2025-07-27 231202" src="https://github.com/user-attachments/assets/f4394ef1-4ecc-4201-afd6-cc5ff3cfab8d" />

<img width="705" height="478" alt="Screenshot 2025-07-27 231740" src="https://github.com/user-attachments/assets/bb7e1f89-3a51-4929-b2bf-f8d8cf1379c4" />

<img width="1141" height="729" alt="Screenshot 2025-07-27 232708" src="https://github.com/user-attachments/assets/12ad707a-af96-43a5-8e49-e842b7497896" />


## Analysis

### 📊 Analysis: Detailed Findings & Statistical Insights

#### 🚗 1. Fare Distribution
- Most fares fall between $5–$20
- Box plot revealed outliers above $100, likely long-distance or airport trips
- Peak hour fares tend to be slightly higher due to demand

#### ⏰ 2. Time-Based Ride Patterns
- Busiest hours: 7–9 AM and 5–7 PM (commute hours)
- Highest ride count on Fridays and Saturdays
- Monthly trends show increased activity during summer months

#### 📍 3. Geographic Trends
- Ride pickups are concentrated in central NYC
- Hotspots include Midtown Manhattan and Lower Manhattan
- Map visualization shows higher fares around airports and business districts

#### 📈 4. Correlations & Metrics
- Positive correlation between fare amount and distance
- Longer ride durations generally result in higher fares
- Average fare per km stays consistent but slightly rises during peak hours

## Key Results:
### Key Discoveries and Pattern Identification
#### Peak Ride Hours
 - Highest ride volumes occur during morning (7–9 AM) and evening (5–7 PM) commute times.
#### Weekly Trends
 - Fridays and Saturdays show the most Uber activity, indicating weekend demand.
#### Fare Patterns
 - Most fares are between $5 and $20, with a few outliers above $100 from longer trips.
#### Geographic Hotspots
 - Frequent pickups in Midtown and Lower Manhattan; high fares near airport areas.
#### Distance vs Fare
 - Clear positive correlation between ride distance and fare amount.
#### Seasonal Variation
 - Increased ride activity during summer months, suggesting seasonal travel trends.
#### Peak vs Off-Peak Impact
 - Average fares are slightly higher during peak hours due to higher demand.

## Conclusion
### Summary of Main Findings
- Uber rides show clear peak usage during morning and evening commute hours.
- Weekends, especially Fridays and Saturdays, record the highest ride volumes.
- The majority of fares are affordable, ranging between $5 and $20, with a few high-value outliers.
- Ride density is highest in central NYC, particularly Midtown and Lower Manhattan.
- Airport-related trips tend to have higher fares and longer distances.
- There is a strong correlation between fare amount and distance traveled.
- Seasonal patterns indicate increased ride demand during summer months.
- Peak time rides are associated with slightly higher average fares, reflecting surge pricing or demand pressure.


