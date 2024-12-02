# Final Project Submission

Please fill out:
* Student name: Morgan Abukuse Amunga
* Student pace: full time - Remote
* Scheduled project review date/time: 
* Instructor name: Lucille Kaleha
* Blog post URL:


# Project Overview

This project focuses on analyzing aviation accident data from 1962 to 2023, sourced from the National Transportation Safety Board (NTSB). The primary goal is to identify low-risk aircraft and provide actionable insights to improve aviation safety and operational decision-making for a business venturing into the aviation industry.


# Business Understanding

## Stakeholders

***1. Aviation Division Head***

The primary stakeholder responsible for operational and strategic decisions in the aviation business

***2. Business Executive***

Require actionable insights for aircraft acquisition and safety strategies.

***3. Operational Teams***

Interested in insights to enhance safety protocols and mitigate risks during operations.

## Key Business Questions

1. Which aircraft types are associated with the lowest risks based on historical accident data?
2. What operational factors (e.g., flight phases, weather conditions) significantly impact accident severity?
3. Which regions and manufacturers require targeted safety interventions to reduce accident risks?


# Data Understanding and Analysis

## Source of Data

The data comes from the National Transportation Safety Board (NTSB) and includes civil aviation accidents and incidents from 1962 to 2023.

It has the following Key features;

a. Accident Details: Date, location, and severity of accidents.

b. Aircraft Specifications: Manufacturer, model, and type.

c. Operational Context: Flight purpose, weather conditions, and flight phases.

d. Injuries: Counts of fatal, serious, and minor injuries.


## Exploration of Aviation Dataset

My goal here is to understand the structure of the dataset. This includes columns, data types, and missing values. 
Also to identify any immediate data quality issues eg inconsistent types, missing values e.t.c

## Loading the necessary libraries

I will import pandas library as below;

import pandas as pd

## Loading the aviation dataset

After importing the pandas library, my next step is now to load aviation data as shown.

aviation_data = pd.read_csv('AviationData.csv')
aviation_data

## Let us check the Dataset shape(rows and columns)

print("Dataset Shape:",aviation_data.shape)

The dataset has 90348 rows and 31 columns.

## Display first 5 rows of our dataset

This will help me to understand the details of our first 5 rows with all the 31 columns.

aviation_data.head()

## Checking for the dataset information, column names and data types

aviation_data.info()

The dataset has a entries which varies across. Also we have object and float64 data types 

aviation_data.dtypes

## Checking for missing values

aviation_data.isnull().sum()

# Checking for summary statistcics 

aviation_data.describe()

# Points to Note

### 1.Missing Values

  Schedule, Air.carrier, Aircraft.Category, FAR.Description, and Longitude have the highest number of missing values while      
  Investigation.Type does not have a missing value.

### 2.There are key inconsistencies in our dataset. This is as follows;
    
   a. Longitude and Latitude should be numeric(float64)
   
   b. Event.Date and Publication.Date should be datetime
   
   c. Number of engines and injuries are integers like column could be integers after imputing missing values.
   
   d. Categorical data stored as objects could be converted to category for better memory optimization and performance.

### 3.Risk Indicators

   a. Accident Frequency. (Say how any accidents occurred annually or by location(State or country), identify the trends of 
       accident occurrence over time)
       
   b. Injury severity.(Distribution of Total.Fatal.Injuries, total serious injuries and total minor injuries)
   
   c. Aircraft damage. Categories of aircraft damage and relationship between damage type and injury severity



I will use this point for;

   1.Risk Evaluation. Say which aircraft types,flight phases or conditions lead to higher risks to guide  in decision    
        making.
         
   2.For business recommendations. Will help the stakeholders focus on low risk aircraft,regions and conditions.

# Data Cleaning and Preparation

I will drop some columns ***(High-missing-value columns like Latitude, Longitude, Airport.Code, etc.)***, perform imputation ***(Filled missing injury values with 0 and categorical values with "Unknown")***, standardize(ensure consistency in key features) and analyse the dimensions ***(Accident trends over time.
Severity of injuries by flight phase, weather condition, and aircraft type.
Manufacturer and regional analysis)***

### Columns with missing values 

missing_values = aviation_data.isnull().sum()
missing_values

### Percentage of Missing Values for each column 

missing_percentage = (missing_values / len(aviation_data)) * 100
missing_percentage

Combining the Results into a Dataframe for Readability

missing_data_summary = pd.DataFrame({
  "Missing Values": missing_values,  
  "Percentage Missing": missing_percentage
}).sort_values(by = "Percentage Missing",ascending = False)
missing_data_summary

Columns with more than 50%

critical_missing_columns = missing_data_summary[missing_data_summary["Percentage Missing"] > 50]
critical_missing_columns

These missing values can skew analysis or cause errors in calculations.

## Addressing the Missing Values(Imputation and dropping columns)

columns_to_drop = ['Latitude', 'Longitude', 'Airport.Code', 'Airport.Name', 'Aircraft.Category',
                   'FAR.Description', 'Schedule', 'Air.carrier']

Drop Columns with more that 50% missing values

aviation_data_cleaned = aviation_data.drop(columns=columns_to_drop, errors='ignore')

Imputing Missing Values

### Dropping high-missing-value and irrelevant columns
columns_to_drop = ['Latitude', 'Longitude', 'Airport.Code', 'Airport.Name', 'Report.Status', 'Publication.Date']
aviation_data_cleaned = aviation_data_cleaned.drop(columns=columns_to_drop, errors='ignore')

# Imputing remaining missing values
## For low to moderate missing columns
low_missing_columns = ['Make', 'Model', 'Amateur.Built', 'Location', 'Country',
                       'Registration.Number', 'Injury.Severity', 'Number.of.Engines', 'Engine.Type']
for col in low_missing_columns:
    aviation_data_cleaned[col] = aviation_data_cleaned[col].fillna('Unknown')

## Verify the cleaning process
print("Remaining Missing Values After Final Cleaning:")
print(aviation_data_cleaned.isnull().sum())


## Analyzing Accident Frequency

This will help us in identifying the trends in accident frequency over time and by location,helping stakeholders understand patterns and potential risk factors

# Extract Year from Event.Date
## Ensuring Event.Date is a datetime object
aviation_data_cleaned['Event.Date'] = pd.to_datetime(aviation_data_cleaned['Event.Date'], errors='coerce')
aviation_data_cleaned['Year'] = aviation_data_cleaned['Event.Date'].dt.year

## Count accidents per year
accidents_per_year = aviation_data_cleaned['Year'].value_counts().sort_index()

## Visualize accidents over time
![Project Logo](visuals/accidents_per_year.png)


## Analyze accidents by location (Country)
accidents_by_country = aviation_data_cleaned['Country'].value_counts()

## Display the top 10 countries with the most accidents
print("Top 10 Countries with Most Accidents:")
print(accidents_by_country.head(10))

## Visualize accidents by location


![Project Logo](visuals/top_10_countries_with_most_accidents.png)
# Trends in accidents over time help identify periods of higher risk.
## Locations with high accident counts highlight regions needing more attention.

We will be looking at;

1.Trends over time to deterine if accidents are increasing or decreasing and if there are any spikes or anomalies during specific years.

2.High risk locations. Which countries reported the most accidents and any patterns based on geography or operational conditions.

## Analyzing Severity and Risk Indicators

The goal is to understand the distribution of injury severity,helping stakeholders assess the impact of different accidents. I'll be keen to look at injury severity(which injury type(fatal,serious,minor) is most common, are there outliers and flight phases(phases with highest number injuries and any operational patterns contributing to risks ).  

import matplotlib.pyplot as plt
import numpy as np

## Selecting injury-related columns
injury_columns = ['Total.Fatal.Injuries', 'Total.Serious.Injuries', 'Total.Minor.Injuries']

## Grouping by flight phase and summing the injuries

![Project Logo](visuals/injury_severity_by_flight_phase.png)

## Trends by Aircraft Type 

# Normalize the 'Make' column: Convert to uppercase to handle case sensitivity
aviation_data_cleaned['Make'] = aviation_data_cleaned['Make'].str.upper()

# Correct common inconsistencies (e.g., misspellings)
aviation_data_cleaned['Make'] = aviation_data_cleaned['Make'].replace({
    "BOIENG": "BOEING",
    # Add other corrections here if necessary
})

# Count accidents by aircraft make
accidents_by_aircraft = aviation_data_cleaned['Make'].value_counts()

## Display the top 10 aircraft manufacturers with the most accidents
print("Top 10 Aircraft Manufacturers with Most Accidents:")
print(accidents_by_aircraft.head(10))

## Visualize the top 10 aircraft manufacturers

![Project Logo](visuals/top_10_aircraft_manufacturers_with_most_accidents.png)

## Weather Conditions and Accident Severity

import matplotlib.pyplot as plt
import numpy as np

## Analyze accidents by weather condition
weather_severity = aviation_data_cleaned.groupby('Weather.Condition')[injury_columns].sum()

![Project Logo](visuals/severity_of_injuries_by_weather_condition.png)
## Accident Purpose 

import matplotlib.pyplot as plt
import numpy as np

## Analyze accidents by purpose of flight
purpose_severity = aviation_data_cleaned.groupby('Purpose.of.flight')[injury_columns].sum()

![Project Logo](visuals/severity_of_injuries_by_purpose_of_flight.png)
## Observation from Analysis

1. Accident Trends Over Time

     Accidents have fluctuated over the decades, with spikes observed in earlier years.
     The number of accidents appears to decrease in recent years, reflecting improvements in aviation safety, technology, and        regulations.


2. Severity by Flight Phase

   Cruise Phase accounts for the highest number of fatalities and serious injuries. Despite fewer occurrences   
   compared to takeoff or landing, cruise-phase accidents tend to be more catastrophic.

   Takeoff has significant injury counts across all levels, including fatalities, highlighting the risks of this critical 
   phase.

   Maneuvering also records a high number of fatalities, indicating challenges during non-standard flight maneuvers.

   Prioritizing safety enhancements during the cruise, takeoff, and maneuvering phases is crucial.


3. Severity by Weather Condition
   
   IMC (Instrument Meteorological Conditions): Accidents during adverse weather result in more severe injuries, with   
   fatalities and minor injuries.
   VMC (Visual Meteorological Conditions): While VMC conditions account for more total injuries, this is likely due to the 
   higher volume of flights under these conditions.
   Weather-related risks can be mitigated through enhanced pilot training and advanced avionics for flying in IMC conditions.


4. Severity by Purpose of Flight

     Personal Flights: These account for a majority of severe injuries and fatalities, reflecting the variability in pilot 
     experience and regulatory oversight in this sector.
     Commercial Flights: Though less frequent, accidents in commercial aviation can result in significant injuries due to higher      passenger loads.
     Targeting safety in personal flights offers the greatest potential for reducing aviation accidents.


5. Top 10 Aircraft Manufacturers with Most Accidents

    Cessna and Piper dominate accident records. These manufacturers are prevalent in personal and recreational aviation.
    Commercial Manufacturers: Boeing and Airbus report fewer accidents, reflecting their advanced safety features and  
    operational standards.
    Selecting modern commercial aircraft with proven safety records is key to reducing risk.


6. Top 10 Countries with Most Accidents

   The United States leads in accident counts, which is expected due to its large aviation sector and comprehensive reporting  
   systems.
   Other countries with high accident counts may reflect regional safety challenges or reporting inconsistencies.
   Focusing safety efforts in high-accident regions can yield significant impact.


# Recommendations

### 1. Aircraft Selection

Focus on acquiring modern commercial aircraft from manufacturers with a strong safety track record (e.g., Boeing, Airbus) while avoiding older or recreational aircraft models from high-risk manufacturers (e.g., Cessna, Piper) unless supported by enhanced safety measures.

### 2. Prioritize Critical Flight Phases

Implement targeted training and automated safety systems for high-risk flight phases, particularly cruise, takeoff, and maneuvering.

### 3. Enhance Weather-Related Safety

Develop stringent protocols and pilot training programs for operating in adverse weather conditions (IMC), supported by advanced avionics systems such as predictive weather radar and real-time navigation aids.

### 4. Target Personal Aviation Risks

Invest in initiatives to enhance safety in personal aviation, including;

a. Improved training for recreational pilots.

b. Stricter maintenance standards for personal and recreational aircraft.

c. Educational programs on weather and operational safety for amateur pilots.

### 5. Regional Safety Initiatives

Focus safety improvement efforts in regions with the highest accident counts, particularly in the United States and other countries with notable risks.

### 6. Invest in Technology-Driven Safety

Equip aircraft with modern safety technologies, including:
Automated emergency landing systems.
Enhanced collision avoidance systems.
Real-time engine and system monitoring tools.

# Conclusion

## 1. Accident Trends

Accidents have generally declined over the years due to technological advancements and safety improvements.


## 2. High-Risk Factors

a. Cruise and takeoff phases are associated with severe injuries and fatalities.

b. IMC conditions pose higher risks per flight, while VMC conditions account for more injuries due to flight volume.


## 3. Manufacturer Insights

a. Cessna (27,149 accidents) and Piper (14,870 accidents) dominate accident counts, emphasizing risks in personal aviation.

b. Manufacturers like Boeing and Airbus exhibit lower accident rates, reflecting advanced safety features and operational standards.