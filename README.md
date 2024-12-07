# Aircraft Risk Analysis

## Overview
This project analyzes aviation accident data from 1962 to 2023 to identify low-risk aircraft and provide actionable insights. Using data from the National Transportation Safety Board (NTSB), it supports strategic decision-making by uncovering trends, risks, and safety recommendations for aviation operations.

## Table of Contents
1. [Overview](#overview)
2. [Business Understanding](#business-understanding)
3. [Data Understanding](#data-understanding)
4. [Key Findings](#key-findings)
5. [Recommendations](#recommendations)
6. [Repository Structure](#repository-structure)
7. [Installation/Setup](#installationsetup)
8. [Usage Instructions](#usage-instructions)
9. [Contact Information](#contact-information)

## Business Understanding

### Stakeholders
- **Aviation Division Head**: Strategic decision-making for aviation operations.
- **Business Executives**: Insights for aircraft acquisition and risk mitigation.
- **Operational Teams**: Guidance for enhancing safety protocols.

### Key Business Questions
1. Which aircraft types are associated with the lowest risks based on historical accident data?
2. What operational factors (e.g., flight phases, weather conditions) significantly impact accident severity?
3. Which regions and manufacturers require targeted safety interventions?

## Data Understanding

### Data Source
The dataset is sourced from the National Transportation Safety Board (NTSB) and includes aviation accidents and incidents from 1962 to 2023.

### Key Features
- **Accident Details**: Date, location, and severity.
- **Aircraft Specifications**: Manufacturer, model, and type.
- **Operational Context**: Flight purpose, weather conditions, and flight phases.
- **Injuries**: Counts of fatal, serious, and minor injuries.

### Challenges
- Missing values in columns like `Latitude`, `Longitude`, and `Airport.Code`.
- Inconsistent data types that required standardization (e.g., `Make` column).

## Key Findings
1. **Accident Trends**: Accidents have generally declined over the years, reflecting technological advancements and improved safety standards.
2. **Flight Phase Risks**:
   - **Cruise and takeoff phases** are associated with severe injuries and fatalities.
   - **Maneuvering** also poses significant risks.
3. **Weather Conditions**:
   - **IMC conditions** pose higher risks per flight.
   - **VMC conditions** account for more total injuries due to higher flight volumes.
4. **Manufacturer Insights**:
   - **Cessna (27,149 accidents)** and **Piper (14,870 accidents)** dominate accident counts.
   - **Boeing and Airbus** show lower accident rates due to advanced safety features.
5. **Regional Insights**:
   - The **United States** leads in accident counts, reflecting its large aviation sector.

## Recommendations
1. **Aircraft Selection**: Focus on modern commercial aircraft with strong safety records (e.g., Boeing, Airbus).
2. **Critical Flight Phases**: Prioritize training and automated systems for cruise, takeoff, and maneuvering.
3. **Weather Safety**: Enhance pilot training for IMC conditions and equip aircraft with advanced navigation tools.
4. **Personal Aviation Risks**: Improve training, maintenance, and safety education for recreational pilots.
5. **Regional Initiatives**: Focus safety improvements in high-accident regions like the U.S.
6. **Safety Technologies**: Invest in advanced systems like automated landing tools and collision avoidance technologies.

## Repository Structure
Aircraft_Risk_Analysis/ │ ├── README.md # Overview of the project ├── notebooks/ │ ├── Aircraft_Risk_Analysis_Notebook.ipynb # Final Jupyter Notebook ├── presentation/ │ ├── Aircraft_Risk_Analysis_Final_Recreated_Presentation.pptx # Final Presentation ├── visuals/ │ ├── updated_aircraft_manufacturers_no_standardized.png # Top 10 Aircraft Visualization │ ├── accidents_trends.png # Accident Trends Visualization │ ├── flight_phase_severity.png # Flight Phase Visualization │ ├── weather_severity.png # Weather Conditions Visualization │ ├── countries_accidents.png # Countries with Accidents Visualization ├── data/ │ ├── Aircraft_Risk_Analysis_Cleaned.csv # Cleaned dataset for reproducibility

## Installation/Setup
To replicate the project:
1. Ensure Python is installed (version >= 3.8 recommended).
2. Install necessary libraries:
   ```bash
   pip install -r requirements.txt
3. Download the dataset and place it in the data/ directory.

**Usage Instructions**
1. Open the Jupyter Notebook:
   jupyter notebook notebooks/Aircraft_Risk_Analysis_Notebook.ipynb
2. Follow the steps in the notebook to:
     Clean and preprocess the data.
     Perform analysis and generate visualizations.
3. Use the insights and visualizations to inform decisions.

**Contact Information**
For questions or feedback, reach out to:

Email: abumorgan@gmail.com
LinkedIn: https://www.linkedin.com/in/morgan-a-140948124/
