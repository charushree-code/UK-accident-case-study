# Case Study - UK Road Accident Using Python, SQL, and Tableau

## Introduction
The case study involves a dataset of UK road accidents maintained by the UK government. This comprehensive repository is updated annually and is available to the public via the Open Data website. The analysis will follow the 6 phases of the Data Analysis process: Ask, Prepare, Process, Analyze, and Act (APPAA).

## ASK
In this case study, we will explore the UK Road Accident dataset to analyze different factors that contribute to traffic accidents. The dataset includes a range of variables such as geographic locations, weather conditions, vehicle types, and the number of casualties involved. The following questions will guide our analysis:

- What are the most common types of vehicles involved in accidents?
- What are the most frequent weather conditions associated with accidents?
- How do the number of casualties vary across different types of accidents?
- Which geographic areas have the highest occurrence of accidents?
- How does the time of day impact the frequency and severity of accidents?
- What is the distribution of accidents across different days of the week?
- Which light conditions (e.g., daylight, darkness with street lights) are most associated with severe accidents?
- Is there a correlation between the number of vehicles involved and the severity of accidents?
- How does the occurrence of accidents vary between urban and rural areas?

By answering these questions, we can gain valuable insights into the factors that contribute to road accidents and identify areas where interventions may be necessary to reduce the occurrence of accidents.

## PREPARE
The dataset contains an array of information ranging from geographic locations to weather conditions, vehicle types, and the number of casualties involved. This comprehensive data provides an excellent opportunity for in-depth analysis and research, and the Department of Transport is responsible for publishing it.

## PROCESS
### Cleaning and Preparation of Data for Analysis
**Using Python:**

1. **Read the CSV file**
    ```python
    import pandas as pd
    df = pd.read_csv(r'...\data.csv')
    ```

2. **Check the columns**
    ```python
    df.columns
    ```

3. **Calculate the percentage of missing values for each column in the dataframe**
    ```python
    df.isnull().sum() / len(df) * 100
    ```

4. **Clean the date column**
    ```python
    df['Accident_Date'] = df['Accident_Date'].str.replace('-', '/')
    df['Accident_Date'] = pd.to_datetime(df['Accident_Date'], format='%d/%m/%Y')
    ```

5. **Clean longitude and latitude columns**
    ```python
    df['Longitude'] = df['Longitude'].replace('NA', pd.NA)
    df['Latitude'] = df['Latitude'].replace('NA', pd.NA)
    ```

6. **Drop rows with NaN**
    ```python
    df = df.dropna(subset=['Longitude', 'Latitude', 'Road_Type', 'Road_Surface_Conditions', 'Weather_Conditions'])
    ```

7. **Save the cleaned data to a new CSV file**
    ```python
    df.to_csv('cleaned_data.csv', index=False)
    ```

## ANALYZE
### Using MySQL:
1. **Most Common Types of Vehicles Involved in Accidents**
    ```sql
    SELECT Vehicle_Type, COUNT(*) AS count
    FROM accidents
    GROUP BY Vehicle_Type
    ORDER BY count DESC;
    ```

2. **Most Frequent Weather Conditions Associated with Accidents**
    ```sql
    SELECT Weather_Conditions, COUNT(*) AS count
    FROM accidents
    GROUP BY Weather_Conditions
    ORDER BY count DESC;
    ```

3. **Number of Casualties Vary Across Different Types of Accidents**
    ```sql
    SELECT Accident_Severity,
           AVG(Number_of_Casualties) AS avg_casualties,
           MIN(Number_of_Casualties) AS min_casualties,
           MAX(Number_of_Casualties) AS max_casualties
    FROM accidents
    GROUP BY Accident_Severity;
    ```

4. **Geographic Areas with the Highest Occurrence of Accidents**
    ```sql
    SELECT [District Area], COUNT(*) AS count
    FROM accidents
    GROUP BY [District Area]
    ORDER BY count DESC;
    ```

5. **Impact of Time of Day on the Frequency and Severity of Accidents**
    ```sql
    SELECT strftime('%H', Accident_Date) AS hour,
           COUNT(*) AS count,
           AVG(Number_of_Casualties) AS avg_casualties
    FROM accidents
    GROUP BY hour
    ORDER BY hour;
    ```

6. **Distribution of Accidents Across Different Days of the Week**
    ```sql
    SELECT strftime('%w', Accident_Date) AS day_of_week,
           COUNT(*) AS count
    FROM accidents
    GROUP BY day_of_week
    ORDER BY day_of_week;
    ```

7. **Light Conditions Associated with Severe Accidents**
    ```sql
    SELECT Light_Conditions,
           COUNT(*) AS count,
           SUM(CASE WHEN Accident_Severity = 'Serious' THEN 1 ELSE 0 END) AS serious_accidents
    FROM accidents
    GROUP BY Light_Conditions
    ORDER BY serious_accidents DESC;
    ```

8. **Correlation Between the Number of Vehicles Involved and the Severity of Accidents**
    ```sql
    SELECT Number_of_Vehicles,
           AVG(CASE WHEN Accident_Severity = 'Serious' THEN 1 ELSE 0 END) AS serious_accident_rate
    FROM accidents
    GROUP BY Number_of_Vehicles
    ORDER BY Number_of_Vehicles;
    ```

9. **Occurrence of Accidents in Urban vs Rural Areas**
    ```sql
    SELECT Urban_or_Rural_Area, COUNT(*) AS count
    FROM accidents
    GROUP BY Urban_or_Rural_Area
    ORDER BY count DESC;
    ```

## VISUALIZATION
### Dynamic Dashboard Using Tableau
The visualization of data has become an integral part of data analysis. Tableau is one of the most popular and user-friendly data visualization tools widely used in the industry. With Tableau, users can easily create interactive dashboards, charts, and graphs that help to visualize complex data sets in a meaningful way. 

To fully experience the power of Tableau, users can click on the provided link to view the visualization and explore the data in detail. 

[Click here!](#)

## ACT
### Insights from the Analysis:
**1. Most Common Types of Vehicles Involved in Accidents:**
- Query shows that cars are the most common vehicles involved in accidents, highlighting the need for targeted safety campaigns for car drivers. Vans and goods vehicles under 3.5 tonnes, as well as buses or coaches, also have high accident rates, necessitating better safety training and maintenance protocols for commercial drivers.
- Motorcycles, despite being less common on the roads, have a significant number of accidents, pointing to their vulnerability and the need for enhanced safety measures, such as promoting protective gear and increasing awareness among all road users.


**2. Most Frequent Weather Conditions Associated with Accidents:**
- The data shows that the vast majority of accidents occur in fine weather conditions without high winds, with over half a million incidents recorded. This suggests that while adverse weather conditions can contribute to accidents, many accidents happen in seemingly safe weather, likely due to factors such as driver complacency or higher traffic volumes.
- Rainy conditions without high winds are the second most common, indicating that wet roads pose a significant risk even without strong winds. This is followed by a miscellaneous category ("Other"), which could include various less common weather conditions.
- Accidents during high winds, whether combined with rain or fine conditions, and snowing without high winds, though less frequent, still account for a considerable number of incidents. Fog or mist and snowing with high winds are the least frequent conditions but still present substantial risks due to reduced visibility and slippery surfaces


**3. Number of Casualties Vary Across Different Types of Accidents:**
- **Fatal Accidents**: Fatal accidents have the highest average number of casualties (1.90), with a minimum of 1 casualty and a maximum of 68. This indicates that fatal accidents often involve multiple casualties, highlighting the severe impact these incidents have on human life.
- **Serious Accidents**: Serious accidents have an average of 1.47 casualties, with casualties ranging from 1 to 45. This shows that while serious accidents are less catastrophic than fatal ones, they still often result in multiple casualties, underscoring the need for effective emergency response and medical care.
- **Slight Accidents**: Slight accidents have the lowest average number of casualties (1.33), with a range from 1 to 47. Although these accidents are less severe, they still result in multiple casualties in some cases, emphasizing the importance of preventive measures even for minor accidents.


**4. Geographic Areas with the Highest Occurrence of Accidents:**
- The data indicates that urban areas, particularly major cities, have the highest occurrence of road accidents. Birmingham tops the list with 12,980 accidents, followed by Leeds with 8,785, Manchester with 6,217, Bradford with 6,155, and Westminster with 5,660. These areas are characterized by high population density, heavy traffic, and complex road networks, which contribute to the higher number of accidents.
- In contrast, rural and less populated areas such as Clackmannshire, Teesdale, Shetland Islands, Orkney Islands, and Clackmannanshire report significantly fewer accidents, with Clackmannanshire having the lowest count at 86 incidents. This disparity highlights the influence of traffic volume and urban infrastructure on accident rates.


**5. Impact of Time of Day on the Frequency and Severity of Accidents:**
Accidents occurring at midnight (Hour 0) have been recorded 642,796 times in the dataset. On average, these accidents involve approximately 1.36 casualties per accident.
Conclusions:
- **Frequency of Accidents**: Accidents at midnight (Hour 0) are quite frequent, based on the high count of 642,796 incidents. This indicates that midnight might be a period with increased traffic activity or other contributing factors leading to accidents.
- **Severity of Accidents**: The average number of casualties per accident (1.36) suggests that while accidents are frequent at this hour, they tend to result in relatively fewer casualties on average compared to other times of the day.
- **Possible Explanations**: Accidents at midnight could be influenced by factors such as reduced visibility (especially if light conditions are poor), fatigue among drivers, and potentially higher incidents of impaired driving. However, without additional data on specific circumstances (such as exact causes like drunk driving, speeding, etc.), these are speculative interpretations.
**6. Distribution of Accidents Across Different Days of the Week:**
- **Weekend vs. Weekday Comparison**: There is a noticeable increase in the number of accidents on Saturdays (104,272) compared to Sundays (86,931), indicating that Saturdays have the highest incidence of accidents among the weekend days.
- **Midweek Peaks**: Wednesdays and Thursdays (both around 96,000 accidents) show slightly higher counts compared to Mondays and Fridays, which suggests that midweek days might experience relatively higher accident rates.
- **Lowest Day**: Mondays (70,782) have the lowest count of accidents among the weekdays, potentially indicating a slight decrease in accidents at the start of the workweek.
- **Overall Trend**: The distribution generally follows a pattern where weekends (Saturday and Sunday) see higher accident counts compared to weekdays, with Saturday recording the highest number.


**7. Light Conditions Associated with Severe Accidents:**
- Darkness with no lighting has the highest percentage (19.19%) of serious accidents among all light conditions, indicating a significant association between accidents occurring in conditions where there is no artificial lighting.
- Daylight has a relatively lower percentage (12.64%) of serious accidents compared to darkness conditions.
Therefore, accidents occurring in darkness with no lighting are most strongly associated with severe accidents, suggesting that low visibility conditions significantly increase the risk of serious incidents on the road.


**8. Correlation Between the Number of Vehicles Involved and the Severity of Accidents:**
- Correlation: The data suggests a moderate correlation between the number of vehicles involved and the severity of accidents. Generally, as the number of vehicles increases, the serious accident rate tends to increase as well. This trend underscores the potential for increased risk and severity when multiple vehicles are involved in accidents.
- Exceptions: The presence of 0% serious accident rates for certain numbers of vehicles (like 11, 14, 15, 28, 32) suggests that there are factors beyond just the number of vehicles that influence accident severity. These could include specific types of collisions, road conditions, or other contextual factors not captured in this dataset.

**9. Occurrence of Accidents in Urban vs Rural Areas:**
- Urban vs. Rural: The majority of accidents (about 63.59%) occur in urban areas, compared to about 36.39% in rural areas.
- Unallocated Data: The presence of only 3 unallocated cases doesn't significantly affect the overall percentages but highlights the completeness of the data.


## References
Dataset Source: Department of Transportation  


---
