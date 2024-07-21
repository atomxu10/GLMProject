# Investigation of the effect of speed limit on the severity of road traffic accidents in the UK 2020 ![Language](https://img.shields.io/badge/language-R-blue)

## 1. Introduction
Road traffic accidents (RTAs) are a significant and ongoing problem in the United Kingdom. In 2020, 91,199 reported road accidents resulted in 19,746 serious or fatal injuries and 71,453 slight injuries, devastating consequences for those involved, their families, and society.

It continues to cause severe injuries and incur high costs despite government efforts to avoid them. This analysis investigates the relationship between accident severity and speed limit using data from the UK’s Department for Transport on RTAs in 2020 while also considering other variables that may affect this relationship.

The report will present findings from the generalized linear model and show that higher speed limits contribute to the severity of accidents.

## 2. Data
### 2.1 Data Introduction
The data utilized in this analysis consists of [three different datasets](https://github.com/atomxu10/GLMProject/tree/main/data) published by the DfT, namely Accidents, Vehicles, and Casualties, each providing a detailed account of different aspects of road accidents. The datasets provide a comprehensive and detailed understanding of the factors contributing to road accidents, including the characteristics of vehicles, drivers, casualties, driving conditions, and road features.

The Accidents dataset provides information on accident severity, speed limit, location, driving conditions, and casualties involved in each incident. It includes 36 variables, covering many details, such as light conditions, weather conditions, hazards, and police attendance.

The Vehicles dataset covers 27 variables that provide a detailed account of the vehicles and drivers involved in each accident. It includes information on the type of vehicle, manoeuvre performed during the accident, driver age and sex, the vehicle makes and model, fueling system, and engine capacity. The dataset has multiple rows of data per accident reference number, each representing a different vehicle involved in the accident.

The Casualties dataset includes 18 variables that convey information on the casualties involved in each accident. It contains details on the age and sex of each casualty, whether they were a pedestrian or in a vehicle. Their movement and location during the accident are also included if they were pedestrians. The dataset has multiple rows of data per accident reference number, each representing a different casualty involved in the accident.

### 2.2 Data Cleaning
For this analysis, the accident severity in the Accidents dataset was recorded as “fatal or serious” and “slight,” which differs from the original data, which recorded each severity separately as “fatal,” “serious,” and “slight.” Initial data cleaning consisted of converting -1 values into NAs as these values were known to be missing, given the information available from the data dictionary included with the three datasets of interest. Most of the data variables consisted of categorical values, indicating that they were composed of distinct groups. For example, the accident severity variable is a categorical variable in which each accident is either considered “fatal or serious” or “slight.” Many numerical variables detailed the number of vehicles and casualties involved in each accident, the ages of drivers, casualties, and vehicles in addition to their engine capacity.

The Vehicles and Casualties datasets comprised multiple rows per accident. Given the hierarchical nature of this data, statistical analysis would require using random effects models to account for the correlation of the data. Given this, the data in the two datasets was summarised into one row per accident reference number such that traditional methods of generalized linear models (GLMs) would be applicable. Not accounting for this and still using standard GLMs would have violated the assumptions of independence in the data, as each accident would be recorded in the data multiple times to account for the multiple vehicles and casualties involved in each accident. In order to summarise these data into a single row per accident reference, the following approach was used:

- Group the data by the accident reference number
- If the variable is numeric, summarise the mean, minimum and maximum values
- If the variable is categorical, summarise whether any row in the group had a specific level of interest


To illustrate, when a categorical variable describing the types of vehicles engaged in an accident was examined, a new variable was created to indicate whether a motorcycle was involved. If a row in the category contained information related to a motorcycle, the new variable was assigned a value of “Y” to indicate involvement or “N” to indicate the non-involvement of a motorcycle. This process was repeated for every category level to retain as much data as possible for the relevant categorical variables.

Once each dataset had been reduced to one row per accident, they were joined to the Accidents dataset by using the accident reference number to match the observations to the correct accident, resulting in a single data set containing all the information available from the three datasets. Finally, variables containing high levels of missing data, above 5%, were removed from further analysis as these often had other variables in the data set that reported similar information.

### 2.3  Exploratory Data Analysis
A total of 91,199 road traffic accidents were recorded in 2020. Among them, 21.7% were defined as ‘Fatal or serious,’ and 78.3% were defined as ‘Slight.’ More accidents occurred in the summer (23,753), autumn (25,733), and winter (26,265), and relatively few in the spring (15,448). However, the proportion of fatal or serious accidents in spring was higher than in other seasons, at 23.3%. During the day, accidents were mainly concentrated in the afternoon (12-18) and the morning (6-12), with 43.8% and 26.0%, respectively, while the evening hours (0-6) had the highest risk of a severe accident at 26.0%. 68% of accidents occurred in urban areas (versus 32% in rural areas). The majority of accidents occurred at roundabouts (73.4%).
Most accidents occurred on roads with a speed limit of 30mph (57.3%). The following highest percentage of accidents occurred on roads with speed limits of 60mph (12.5%) and 20mph (12.3%), followed by 40mph (8.6%), 70mph (5.1%), and 50mph (4.2%) (refer to Figure 1[left]). The severity of accidents also varied depending on the speed limit on the road. Roads with higher speed limits had a higher proportion of serious accidents, with those with a speed limit of 60mph having the highest percentage of severe accidents (32.7%). The severity of accidents decreased for roads with speed limits of 50mph (24.8%) and 70mph (24.6%), followed by 40mph (23%), 30mph (19.7%), and 20mph (15.7%) (refer to Figure 1[right]).





