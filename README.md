# Los-Angeles-Crime-Data-Analysis
This project supports the Los Angeles Police Department (LAPD) by analyzing crime data to identify patterns in criminal behavior. The insights help allocate resources effectively to tackle various crimes in different areas.
# Los Angeles Crime Data Analysis 🚔

Los Angeles, California 😎. The City of Angels. Tinseltown. The Entertainment Capital of the World! 

Known for its warm weather, palm trees, sprawling coastline, and Hollywood, along with producing some of the most iconic films and songs. However, as with any highly populated city, it isn't always glamorous and there can be a large volume of crime. That's where this analysis comes in!

This project supports the Los Angeles Police Department (LAPD) by analyzing crime data to identify patterns in criminal behavior. The insights help allocate resources effectively to tackle various crimes in different areas.

## 📊 Dataset Overview

**Data Source**: Los Angeles Open Data (Modified Version)  
**File**: `crimes.csv`  
**Coverage**: Crime incidents across LA's 21 Geographic Areas/Patrol Divisions

### **Dataset Schema**

| Column | Description |
|--------|-------------|
| `'DR_NO'` | Division of Records Number: Official file number made up of a 2-digit year, area ID, and 5 digits |
| `'Date Rptd'` | Date reported - MM/DD/YYYY |
| `'DATE OCC'` | Date of occurrence - MM/DD/YYYY |
| `'TIME OCC'` | In 24-hour military time |
| `'AREA NAME'` | The 21 Geographic Areas or Patrol Divisions with landmark/community names |
| `'Crm Cd Desc'` | Indicates the crime committed |
| `'Vict Age'` | Victim's age in years |
| `'Vict Sex'` | Victim's sex: `F`: Female, `M`: Male, `X`: Unknown |
| `'Vict Descent'` | Victim's descent (A-Z codes for various ethnicities) |
| `'Weapon Desc'` | Description of the weapon used (if applicable) |
| `'Status Desc'` | Crime status |
| `'LOCATION'` | Street address of the crime |

### **Victim Descent Codes**
- **A** - Other Asian, **B** - Black, **C** - Chinese, **D** - Cambodian
- **F** - Filipino, **G** - Guamanian, **H** - Hispanic/Latin/Mexican
- **I** - American Indian/Alaskan Native, **J** - Japanese, **K** - Korean
- **L** - Laotian, **O** - Other, **P** - Pacific Islander, **S** - Samoan
- **U** - Hawaiian, **V** - Vietnamese, **W** - White, **X** - Unknown, **Z** - Asian Indian

## 🔍 Analysis Overview

This comprehensive crime analysis explores three critical questions for law enforcement resource allocation:

**1.** **Peak Crime Hours**: What time of day sees the highest crime frequency?  
**2.** **Night Crime Hotspots**: Which area has the most crimes during late-night hours (10pm-3:59am)?  
**3.** **Victim Demographics**: How are crimes distributed across different age groups?

## 🔧 Complete Analysis Code

```python
# Import required libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Read in and preview the dataset
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes.head()

## Which hour has the highest frequency of crimes? Store as an integer variable called peak_crime_hour
# Extract the first two digits from "TIME OCC", representing the hour,
# and convert to integer data type
crimes["HOUR OCC"] = crimes["TIME OCC"].str[:2].astype(int)

# Preview the DataFrame to confirm the new column is correct
crimes.head()

# Produce a countplot to find the largest frequency of crimes by hour
sns.countplot(data=crimes, x="HOUR OCC")
plt.show()

# Midday has the largest volume of crime
peak_crime_hour = 12

## Which area has the largest frequency of night crimes (crimes committed between 10pm and 3:59am)? 
## Save as a string variable called peak_night_crime_location

# Filter for the night-time hours
# 0 = midnight; 3 = crimes between 3am and 3:59am, i.e., don't include 4
night_time = crimes[crimes["HOUR OCC"].isin([22,23,0,1,2,3])]

# Group by "AREA NAME" and count occurrences, filtering for the largest value and saving the "AREA NAME"
peak_night_crime_location = night_time.groupby("AREA NAME", 
                                               as_index=False)["HOUR OCC"].count().sort_values("HOUR OCC",
                                                                                               ascending=False).iloc[0]["AREA NAME"]

# Print the peak night crime location
print(f"The area with the largest volume of night crime is {peak_night_crime_location}")

## Identify the number of crimes committed against victims by age group (0-17, 18-25, 26-34, 35-44, 45-54, 55-64, 65+) 
## Save as a pandas Series called victim_ages

# Create bins and labels for victim age ranges
age_bins = [0, 17, 25, 34, 44, 54, 64, np.inf]
age_labels = ["0-17", "18-25", "26-34", "35-44", "45-54", "55-64", "65+"]

# Add a new column using pd.cut() to bin values into discrete intervals
crimes["Age Bracket"] = pd.cut(crimes["Vict Age"],
                               bins=age_bins,
                               labels=age_labels)

# Find the category with the largest frequency
victim_ages = crimes["Age Bracket"].value_counts()
print(victim_ages)
```

## 📈 Key Findings

### **Temporal Crime Patterns**
- **Peak Crime Hour**: **12 PM (Noon)** - Highest frequency of criminal incidents
- **Crime Distribution**: Clear hourly patterns visible in the analysis chart

### **Geographic Crime Hotspots**
- **Top Night Crime Area**: **Central** division experiences the highest volume of late-night crimes (10pm-3:59am)
- **Strategic Insight**: Night patrol resources should be prioritized in Central area

### **Victim Age Demographics**
**Crime frequency by age group**:
- **26-34 years**: 47,470 victims (highest)
- **35-44 years**: 42,157 victims  
- **45-54 years**: 28,353 victims
- **18-25 years**: 28,291 victims
- **55-64 years**: 20,169 victims
- **65+ years**: 14,747 victims
- **0-17 years**: 4,528 victims (lowest)

## 🛠️ Technical Highlights

### **Data Engineering Techniques**
- **String manipulation**: Extracting hour data from military time format
- **Data type conversion**: Converting string time to integer for analysis
- **Boolean filtering**: Isolating night-time crime incidents
- **Age binning**: Creating meaningful age categories using `pd.cut()`

### **Statistical Analysis Methods**
- **Frequency analysis** using `value_counts()`
- **Groupby operations** for geographic aggregation
- **Time-based filtering** for temporal pattern identification
- **Categorical data binning** for demographic analysis

### **Data Visualization**
- **Count plots** using Seaborn for crime frequency visualization
- **Hourly distribution analysis** showing temporal crime patterns
- **Clear visual representation** of peak crime times

## 📋 Requirements

```python
pandas
numpy
matplotlib
seaborn
```

## 🚀 Getting Started

**1.** Clone this repository  
**2.** Install required packages: `pip install pandas numpy matplotlib seaborn`  
**3.** Ensure the crime dataset is located at `crimes.csv`  
**4.** Run the analysis script to reproduce all findings

## 📊 Strategic Implications for LAPD

### **Resource Allocation Recommendations**
- **Noon Patrol Enhancement**: Increase officer presence during 12 PM peak hours
- **Central Area Night Focus**: Deploy additional night shift resources to Central division
- **Age-Targeted Programs**: Develop crime prevention strategies for the 26-44 age demographic

### **Operational Insights**
- **Temporal Patterns**: Crime shows clear hourly variations requiring adaptive patrol scheduling
- **Geographic Concentration**: Night crimes cluster in specific areas, enabling targeted deployment
- **Demographic Trends**: Adult working-age populations face highest victimization rates

## 🔬 Research Applications

This analysis framework can be extended for:
- **Seasonal crime pattern analysis**
- **Crime type-specific geographic mapping**
- **Weapon usage correlation studies**  
- **Victim demographic cross-analysis**
- **Crime status and resolution rate studies**

## 🤝 Contributing

Feel free to fork this project and explore additional research questions such as:
- Crime trends by day of week and month
- Weapon type analysis by area and time
- Crime clearance rates across different divisions
- Correlation between victim demographics and crime types

## 📝 License

This project is available under the MIT License. Data courtesy of Los Angeles Open Data.
# Expected Results
<img width="1169" height="865" alt="image" src="https://github.com/user-attachments/assets/e0c95d7a-1741-4e38-9e9b-f8b837e09e35" />

```The area with the largest volume of night crime is Central
26-34    47470
35-44    42157
45-54    28353
18-25    28291
55-64    20169
65+      14747
0-17      4528
Name: Age Bracket, dtype: int64```
