
# Electric Vehicle Analysis Project

## **Table of Contents**
1. [Project Overview](#project-overview)
2. [Dataset Description](#dataset-description)
3. [Initial Data Inspection](#initial-data-inspection)
4. [Data Cleaning](#data-cleaning)
5. [Data Visualization](#data-visualization)
   - [8. Vehicle Class Distribution](#vehicle-class-distribution)
   - [9. Make and Model Distribution](#make-and-model-distribution)
   - [10. Battery Capacity vs. Electric Range](#battery-capacity-vs-electric-range)
   - [11. Choropleth Map of Electric Vehicles by State](#choropleth-map-of-electric-vehicles-by-state)
   - [12. Vehicle Distribution by State](#vehicle-distribution-by-state)
   - [13. Vehicle Distribution by City](#vehicle-distribution-by-city)
6. [Insights and Conclusions](#insights-and-conclusions)
7. [Future Work](#future-work)
8. [Acknowledgments](#acknowledgments)

## **Project Overview**
This project analyzes a dataset of electric vehicles to gain insights into their distribution, characteristics, and trends over the years. By employing various data visualization techniques, we aim to identify patterns and relationships within the data that can inform stakeholders in the automotive industry.

## **Dataset Description**
The dataset used in this project contains the following columns:
- VIN
- County
- City
- State
- Postal Code
- Model Year
- Make
- Model
- Electric Vehicle Type
- Clean Alternative Fuel Vehicle (CAFV) Eligibility
- Electric Range
- Base MSRP
- Legislative District
- DOL Vehicle ID
- Vehicle Location
- Electric Utility
- 2020 Census Tract

## **Initial Data Inspection**
Initial inspections of the dataset reveal its structure and provide insights into the types of data contained within. This step helps to identify any immediate data quality issues that may need addressing.

```python
import pandas as pd

# Load the dataset
df = pd.read_csv('electric_vehicles.csv')

# Display the first few rows of the dataset
print(df.head())
```

## **Data Cleaning**
Cleaning the data involves handling missing values, correcting data types, and removing any unnecessary columns to ensure that our analysis is based on accurate and relevant information.

## **Data Visualization**
We use various visualization techniques to explore the dataset.

### **8. Vehicle Class Distribution**
Analyzing the different types of vehicle classes present in the dataset.

```python
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))
sns.countplot(y=df['Electric Vehicle Type'], order=df['Electric Vehicle Type'].value_counts().index, palette='coolwarm')
plt.title('Distribution of Electric Vehicle Types')
plt.show()
```

### **9. Make and Model Distribution**
We analyze the distribution of electric vehicles by their make and model to identify popular manufacturers.

```python
plt.figure(figsize=(12,6))
sns.countplot(x='Make', data=df, order=df['Make'].value_counts().index, palette='viridis')
plt.title('Distribution of Electric Vehicles by Make')
plt.xlabel('Make')
plt.ylabel('Count of Electric Vehicles')
plt.xticks(rotation=45)
plt.show()
```

### **10. Battery Capacity vs. Electric Range**
We explore the relationship between battery capacity and electric range.

```python
plt.figure(figsize=(10,6))
sns.scatterplot(x='Battery Capacity (kWh)', y='Electric Range (mi)', data=df, hue='Make', palette='tab10')
plt.title('Battery Capacity vs. Electric Range')
plt.xlabel('Battery Capacity (kWh)')
plt.ylabel('Electric Range (mi)')
plt.show()
```

### **11. Choropleth Map of Electric Vehicles by State**
Visualizing the distribution of electric vehicles across different states.

```python
import geopandas as gpd

# Load US states geometries
us_states = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))
us_states = us_states[us_states['continent'] == 'North America']

# Group data by state and count the number of electric vehicles
state_counts = df['State'].value_counts().reset_index()
state_counts.columns = ['State', 'Count']

# Merge the counts with the geometry
us_states = us_states.merge(state_counts, left_on='name', right_on='State', how='left').fillna(0)

# Plotting the choropleth map
plt.figure(figsize=(12, 8))
us_states.boundary.plot(color='k')
us_states.plot(column='Count', cmap='Blues', legend=True, ax=plt.gca(),
               legend_kwds={'label': "Number of Electric Vehicles by State", 'orientation': "horizontal"})
plt.title('Choropleth Map of Electric Vehicles by State')
plt.show()
```

### **12. Vehicle Distribution by State**
Analyzing the distribution of electric vehicles across different states.

```python
plt.figure(figsize=(12, 6))
sns.countplot(x='State', data=df, order=df['State'].value_counts().index, palette='viridis')
plt.title('Distribution of Electric Vehicles by State')
plt.xlabel('State')
plt.ylabel('Count of Electric Vehicles')
plt.xticks(rotation=45)
plt.show()
```

### **13. Vehicle Distribution by City**
Examining the distribution of electric vehicles at the city level.

```python
plt.figure(figsize=(12, 6))
sns.countplot(y='City', data=df, order=df['City'].value_counts().head(10).index, palette='magma')
plt.title('Top 10 Cities by Electric Vehicle Count')
plt.xlabel('Count of Electric Vehicles')
plt.ylabel('City')
plt.show()
```

## **Insights and Conclusions**
- This project identifies key trends in the electric vehicle market, including the distribution of vehicle types, manufacturers, and geographical patterns. 
- The analysis indicates that certain states and cities have a significantly higher concentration of electric vehicles, suggesting where marketing efforts and infrastructure improvements could be focused.

## **Future Work**
- Further analysis could include examining the impact of government policies on electric vehicle adoption.
- Conducting a detailed cost-benefit analysis of electric vehicle ownership in different states.
- Expanding the dataset to include more recent years and additional vehicle characteristics for deeper insights.

## **Acknowledgments**
- Thank you to the dataset providers and the open-source community for providing the tools and libraries that made this analysis possible.

