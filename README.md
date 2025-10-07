**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**

Name : Tejasree.K
Reg no:212224240168

```
# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Load dataset
file_path = 'delhi_pm25_aqi.csv'  # Upload this file to Colab environment
df = pd.read_csv(file_path)

# 2. Inspect dataset
print("Dataset Shape:", df.shape)
print("Columns:", df.columns)
print("\nSample Data:\n", df.head())
print("\nData Types:\n", df.dtypes)
print("\nNull Value Counts:\n", df.isnull().sum())

# 3. Data Cleaning
# Parse datetime column
df['period.datetimeFrom.utc'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')

# Filter PM2.5 values > 0 (valid)
df = df[df['value'] > 0]

# 4. Add date, month, hour columns for analysis
df['date'] = df['period.datetimeFrom.utc'].dt.date
df['month'] = df['period.datetimeFrom.utc'].dt.month
df['hour'] = df['period.datetimeFrom.utc'].dt.hour

# 5. Monthly boxplots of PM2.5
plt.figure(figsize=(12, 6))
sns.boxplot(x='month', y='value', data=df)
plt.title('Monthly Boxplot of PM2.5 in Delhi')
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.show()

# 6. Monthly average PM2.5
monthly_avg = df.groupby('month')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
plt.plot(monthly_avg['month'], monthly_avg['value'], marker='o')
plt.title('Monthly Average PM2.5 in Delhi')
plt.xlabel('Month')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(True)
plt.show()

# 7. Days exceeding WHO PM2.5 limit (25 µg/m³)
daily_avg = df.groupby('date')['value'].mean().reset_index()
days_exceeding = daily_avg[daily_avg['value'] > 25]
print(f'Total days analyzed: {len(daily_avg)}')
print(f'Days exceeding WHO limit: {len(days_exceeding)}')
print(f'Percentage exceeding WHO limit: {len(days_exceeding)/len(daily_avg)*100:.2f}%')

# 8. Average PM2.5 vs hour of day
hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(12, 6))
plt.plot(hourly_avg['hour'], hourly_avg['value'], marker='o', color='green')
plt.title('Average PM2.5 vs Hour of Day in Delhi')
plt.xlabel('Hour of Day')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(True)
plt.show()

# 9. Top 5 worst-polluted days
top5_days = daily_avg.nlargest(5, 'value')
print("Top 5 Worst Polluted Days (Date & Avg PM2.5):")
print(top5_days)
```
**Output**

<img width="1325" height="581" alt="image" src="https://github.com/user-attachments/assets/034527b5-f177-43b5-a154-3c9b1bcfba4f" />

<img width="1294" height="714" alt="image" src="https://github.com/user-attachments/assets/5daacdda-4ea1-4233-8fba-17e8ade338c9" />

<img width="1363" height="780" alt="image" src="https://github.com/user-attachments/assets/2ef1b79e-3192-4dcf-b828-ca8555481d64" />

<img width="1328" height="836" alt="image" src="https://github.com/user-attachments/assets/35e36244-b8e7-4cf9-9ae9-c8f574b60b5b" />



**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2)  # write other insights

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


