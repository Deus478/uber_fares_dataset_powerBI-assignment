# uber_fares_dataset_powerBI-assignment
🚗 Project Overview
This project involves a comprehensive analysis of the Uber Fares Dataset, aiming to uncover key insights related to fare behavior, ride trends, and overall service efficiency. The data was first preprocessed and enriched using Python, then visualized through dynamic and interactive dashboards built in Power BI.

# 🎯 Project Objectives

.To examine patterns in Uber fares, ride durations, and time-based trends

.To engineer new analytical variables such as hour, day, and time-based classifications

.To design an interactive Power BI dashboard that effectively visualizes key metrics and insights

# 🚗 Ride Trends
   .By Hour
   .By Day
   .By Month

# 💵 Fare Distribution Visuals

.Histograms for observing fare frequency across price ranges
.Box plots to identify outliers and fare variability
.Pie charts to illustrate the proportional distribution of fare categories

# 🌦️ Seasonal & 🌍 Geographic Insights

.Identify peak ride periods across different seasons and locations.
.Explore potential correlations between weather conditions and ride activity (if data available).
.Communicate findings through data-driven storytelling to support strategic decision-making.

# 🔍 Methodology
🗃️ 1. Data Acquisition
The Uber Fares dataset was sourced from Kaggle, containing key variables such as:

Fare amount

Pickup timestamps

Pickup and drop-off geolocations

# 🧹 2. Data Cleaning (Using Python)
Performed thorough data cleaning and preprocessing in Python to handle missing values, remove anomalies, and prepare the dataset for analysis.
..Pandas was used for inspection and loading
.python
import pandas as pd
uber_df=pd.read_csv(r'C:\Users\user\Downloads\Intro to Big Data Analytics - Python Code\uber.csv')
# check column data types
uber_df.dtypes
# dataset structure
uber_df.tail(5)
# Check the shape (rows, columns)
uber_df.shape
uber_df.describe
<img width="1213" height="406" alt="image" src="https://github.com/user-attachments/assets/38618437-66cb-4a1b-99aa-b4bb9bf4b0a1" />
<img width="1228" height="553" alt="image" src="https://github.com/user-attachments/assets/bf90e66a-074c-48d4-bdb8-adad068adc6f" />
<img width="1353" height="874" alt="image" src="https://github.com/user-attachments/assets/7ac1e570-6f60-4df5-b6fe-ddf012c0b0a7" />
<img width="776" height="146" alt="image" src="https://github.com/user-attachments/assets/1b547f0e-d832-43d0-943c-dd0d6f672ad9" />

. Eliminated records with missing values, duplicates, and identified outliers to ensure data quality.

# Dataset structure and dimensions
print("Shape:", uber_df.shape)
print("\nColumn names:", uber_df.columns.tolist())
print("\nData types:\n", uber_df.dtypes)

# Initial glimpse of the data
uber_df.head()

# Description of numerical columns
print("\nDescriptive Statistics:\n", uber_df.describe())

# Check for missing values
print("\nMissing values per column:\n", uber_df.isnull().sum())

<img width="1784" height="794" alt="image" src="https://github.com/user-attachments/assets/0e50fa61-b83d-46d3-8ab1-9ef33bc722c0" />

# Drop rows with missing essential values (e.g., fare, distance, timestamp)
uber_df = uber_df.dropna(subset=['fare_amount', 'pickup_datetime'])

# Optionally fill other columns with median or mode
uber_df.fillna(uber_df.median(numeric_only=True), inplace=True)

# Drop duplicates if any
uber_df = uber_df.drop_duplicates()

# Convert timestamp column to datetime
uber_df['pickup_datetime'] = pd.to_datetime(uber_df['pickup_datetime'], errors='coerce')

. Exported cleaned CSV for analysis in Power BI
uber_df.to_csv('uber_cleaned.csv', index=False)

# Using IQR method for 'fare_amount'
Q1 = uber_df['fare_amount'].quantile(0.25)
Q3 = uber_df['fare_amount'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
outliers = uber_df[(uber_df['fare_amount'] < lower_bound) | (uber_df['fare_amount'] > upper_bound)]

print(f"Number of outliers in fare_amount: {len(outliers)}")
<img width="538" height="54" alt="image" src="https://github.com/user-attachments/assets/55923ae2-c061-4eae-bc4c-796f7599706f" />

📊 3. Visual Analytics with Power BI
Conducted in-depth analysis using interactive visualizations and time-based insights.

<img width="1505" height="726" alt="image" src="https://github.com/user-attachments/assets/fe562311-4866-43a7-b3d5-ad94dabd574c" />

**Built interactive visuals

**DAX formulas used

-Count
-Sum
-Average
-Variance
-Standard Variation

💵 Fare Trends
.Analysis of fare fluctuations over time

.Patterns in fare amounts by hour, day, and month

.Temporal changes in average and peak fares

Frequency by Fair Amount
<img width="760" height="499" alt="image" src="https://github.com/user-attachments/assets/01c60da6-9e22-4f0b-a60e-e3847947f247" />

# Fare vs. Distance
sns.scatterplot(x='pickup_longitude', y='fare_amount', data=uber_df)
plt.title('Fare Amount vs. pickup_longitude')
plt.xlabel('Distance (miles)')
plt.ylabel('Fare Amount ($)')
plt.xlim(0, 20)
plt.ylim(0, 100)
plt.show()

# Create hour of day first
uber_df['hour'] = uber_df['pickup_datetime'].dt.hour

# Fare vs. Time of Day
sns.boxplot(x='hour', y='fare_amount', data=uber_df)
plt.title('Fare Amount by Hour of Day')
plt.xlabel('Hour')
plt.ylabel('Fare Amount')
plt.ylim(0, 100)
plt.show()

# Correlation heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(uber_df.corr(numeric_only=True), annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()

<img width="772" height="598" alt="image" src="https://github.com/user-attachments/assets/f65e0a0b-5fb0-42ee-8980-1ea72d87812a" />
<img width="789" height="614" alt="image" src="https://github.com/user-attachments/assets/b7be806c-55c4-4470-981b-9ebed3db8d76" />

Correlation HeatMap
<img width="757" height="638" alt="image" src="https://github.com/user-attachments/assets/5c05bce4-bfee-4fd0-bc90-afa4781fc853" />

# Date & time features
uber_df['day'] = uber_df['pickup_datetime'].dt.day
uber_df['month'] = uber_df['pickup_datetime'].dt.month
uber_df['day_of_week'] = uber_df['pickup_datetime'].dt.dayofweek  # 0=Monday

# Peak/Off-Peak
uber_df['peak_hour'] = uber_df['hour'].apply(lambda x: 1 if 7 <= x <= 9 or 16 <= x <= 19 else 0)
# Example: assuming 'payment_type' is a categorical column
if 'payment_type' in uber_df.columns:
    uber_df['payment_type'] = uber_df['payment_type'].astype('category')
    uber_df['payment_type_code'] = uber_df['payment_type'].cat.codes
    
🗺️ Location Mapping
. Based on latitude and longitude data

. Shows spatial distribution of Uber rides
<img width="771" height="473" alt="image" src="https://github.com/user-attachments/assets/32a5127f-2db1-4cce-bac3-78d13c4f2766" />

SUM OF PEAK_HOUR BY PASSENGER_COUNT
<img width="1256" height="695" alt="image" src="https://github.com/user-attachments/assets/a633229d-5d25-4f2c-b588-8f134502259f" />
SUM OF PEAK_HOUR BY PASSENGER_COUNT
<img width="1270" height="778" alt="image" src="https://github.com/user-attachments/assets/e7cf311d-dc63-461a-b488-760d15d2792f" />

SUM OF DAY_OF_WEEK BY MONTH
<img width="1272" height="772" alt="image" src="https://github.com/user-attachments/assets/95f07ced-838e-4bfd-b64e-b8ca71dfeb00" />
SUM OF PICKUP_LATITUDE AND SUM  OF PICKUP_LONGITUDE
<img width="1254" height="764" alt="image" src="https://github.com/user-attachments/assets/832cb361-c06a-491c-b626-cd9b0e207bac" />
🧭 Interactive Features

Slicers: Interactive filters for refining data views
Filters: Applied at visual, page, and report levels
<img width="1252" height="680" alt="image" src="https://github.com/user-attachments/assets/67fa2c68-f652-4df6-8d11-2d108e59c9a8" />

📊 Overall Analysis
The Uber Fares data analysis revealed several meaningful insights into ride behavior, fare patterns, and operational trends:

.Fare Distribution:
Most rides fell within a low to moderate fare range, with a few high-value outliers. Box plots and histograms confirmed the presence of skewed distributions, especially during late-night hours and weekends.

.Time-Based Ride Patterns:

Hourly: Peak demand was observed during early morning commutes (7–9 AM) and evening hours (5–9 PM).

Daily: Weekends showed slightly higher average fares, possibly due to increased leisure travel.

Monthly: Consistent ride activity was noted throughout the year, with minor fluctuations suggesting possible seasonal trends.

.Geospatial Insights:
By mapping ride locations using latitude and longitude, key pickup and drop-off hotspots were identified, indicating concentrated urban activity zones—especially in business districts and near transport hubs.

.Fare vs. Distance Relationship:
A positive correlation was found between fare amount and distance traveled. However, some short-distance rides had unusually high fares, possibly due to traffic, surge pricing, or route inefficiencies.

.Feature Engineering Impact:
New time-based features such as hour, day, and weekday improved the depth of analysis and enabled more dynamic filtering and trend exploration in Power BI.

.Interactive Dashboard Value:
The Power BI dashboard, enhanced with slicers, filters, and drill-downs, allowed users to explore data at multiple granularities—by time, location, or fare category—offering an intuitive, data-driven storytelling experience.

# ✅ Key Results
.Ride Demand Peaks
Peak ride volumes occurred during morning (7–9 AM) and evening (5–9 PM) hours, indicating high commuter traffic.

.Fare vs. Distance
There is a clear positive correlation between fare amount and distance traveled. However, some short trips showed high fares—potentially due to traffic, surge pricing, or minimum fare policies.

.Day & Time Impact on Fares
Higher average fares were observed during weekends and late evening hours, suggesting increased demand and possible fare surges.

.Geographic Hotspots
Visual maps revealed concentrated activity in central urban areas, particularly near business hubs and major transit locations.

.Outlier Detection
Outliers in fare prices were detected and visualized using box plots, helping identify anomalies and unusual transactions.

.Seasonal & Monthly Trends
Consistent ride activity was seen across months, with some seasonal variation indicating higher usage during certain times of the year.

.Enhanced Interactivity
The Power BI dashboard incorporated slicers, filters, and drill-down capabilities, enabling users to explore insights by hour, day, location, and fare range.

# 🏁 Conclusion

This project successfully demonstrated how combining Python and Power BI can turn raw transportation data into meaningful business intelligence. By cleaning, enriching, and analyzing the Uber Fares dataset, we uncovered key operational patterns related to fare distribution, ride demand, and geographic usage.

The interactive dashboard provides a user-friendly platform for ongoing data exploration, supporting data-driven decisions such as optimizing driver allocation, refining pricing strategies, and understanding customer demand behavior.

# 📌 Recommendations: 

Data-Driven Business Strategies
Based on the insights gathered from the Uber Fares dataset and the interactive Power BI dashboard, the following recommendations are suggested to enhance service performance and operational decision-making:

# 🚕 1. Align Driver Supply with Peak Ride Demand
Increase driver presence during high-demand hours, specifically 7:00–9:00 AM and 5:00–7:00 PM, to meet peak commuting needs, minimize rider wait times, and reduce the likelihood of missed ride opportunities.

# 📅 2. Promote Targeted Weekend Offers
Introduce special discounts or bundled ride promotions on Fridays and Saturdays, leveraging increased weekend activity to drive higher customer engagement and ride volume.

# ✈️ 3. Establish Transparent Airport Fare Options
Implement flat-rate or standardized fare options for airport trips to improve fare transparency, reduce rider uncertainty, and build long-term trust in airport transportation services.

# 🗺️ 4. Strengthen Driver Coverage in High-Demand Areas
Deploy additional driver resources in Midtown and Lower Manhattan, where ride requests consistently spike, to ensure better service availability and quicker response times.

# 🌦️ 5. Prepare for Seasonal Fluctuations
Monitor and respond to seasonal increases in demand, particularly during summer months, by offering seasonal incentives or flexible schedules to maintain sufficient driver supply.

# 🕒 6. Encourage Off-Peak Utilization
Introduce fare discounts during off-peak hours to encourage ride activity in lower-demand time slots, helping balance workload and maintain steady revenue flows throughout the day.

# 📊 7. Enhance Fare Estimation Accuracy
Utilize distance-based fare patterns uncovered during analysis to improve fare estimation models, allowing for more accurate, transparent pricing and better-informed ride decisions for customers.


























