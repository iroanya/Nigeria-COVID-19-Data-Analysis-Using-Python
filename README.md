# Nigeria-COVID-19-Data-Analysis-Using-Python.
# Data Scientist Microdegree Capstone Project
 Workbook: https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/blob/main/Ustacky%20Capstone%20Final.ipynb

# Data Collection.

The first thing I did was to import all needed libraries.


```python
import requests
import pandas as pd
import numpy as np
import urllib.request
import csv
import seaborn as sns
sns.set_style("darkgrid")
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import mpld3
from sklearn.cluster import KMeans
from sklearn import datasets
from bs4 import BeautifulSoup
from docx import Document
%matplotlib inline
plt.style.use('fivethirtyeight')  
import warnings
warnings.filterwarnings('ignore')
from bokeh.plotting import figure, show
from bokeh.models import HoverTool
from bokeh.palettes import Category10
import plotly.express as px

```

I proceeded to collect the required data from the website and other sources as directed. 

A. From John Hopkins GitHub repository.

Importing data from John Hopkins.



```python
urlj = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv'
response = requests.get(urlj)
soup = BeautifulSoup(response.text, 'html.parser')

John_Hopkins_confirmed_global = pd.read_csv(urlj)

urlj1 = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv"
John_Hopkins_recovered_global = pd.read_csv(urlj1)

urlj2 = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv"
John_Hopkins_deaths_global =pd.read_csv(urlj2)

```

B. From NCDC website. However, I have problems scraping data from the website as the COVID-19 data on the NCDC website appears not to be functional. I had to copy saved data from the website to my PC and used that for the dataframe. See attached document.


```python
doc = Document("C:\\Users\\ogbo9\\Documents\\Source Code\\NCDC Covid19 Data.docx")

# Extract the data from tables
data = []
header = None
for table in doc.tables:
    for i, row in enumerate(table.rows):
        if i == 0:  # First row is the header
            header = [cell.text for cell in row.cells]
        else:
            row_data = [cell.text for cell in row.cells]
            data.append(row_data)

# Create a dataframe with the extracted data and header
Nigera_NCDC_Data = pd.DataFrame(data, columns=header)
```

C. I took data from the Ustacky GitHub repository to complete the data collection process.


```python
urlu = "https://raw.githubusercontent.com/Ustacky-dev/Nigeria-COVID-19-Data-Analysis-Using-Python/main/covid_external.csv"
Nigeria_covid_external = pd.read_csv(urlu)

urlu1 = "https://raw.githubusercontent.com/Ustacky-dev/Nigeria-COVID-19-Data-Analysis-Using-Python/main/covidnig.csv"
Nigeria_covidnig_data = pd.read_csv(urlu1)

urlu2 = "https://raw.githubusercontent.com/Ustacky-dev/Nigeria-COVID-19-Data-Analysis-Using-Python/main/Budget%20data.csv"
Nigeria_budget_data = pd.read_csv(urlu2)

urlu3 = "https://raw.githubusercontent.com/Ustacky-dev/Nigeria-COVID-19-Data-Analysis-Using-Python/main/RealGDP.csv"
Nigeria_realGDP = pd.read_csv(urlu3)
```

# View The Data.

I used the standard head() and info() method to view the information in the dataframes.


```python
Nigeria_covidnig_data.head(2)
```
States Affected	No. of Cases (Lab Confirmed)	No. of Cases (on admission)	No. Discharged	No. of Deaths
0	Lagos	26,708	2,435	24,037	236
1	FCT	9,627	2,840	6,694	93

# Data Cleaning and Preparation

To Clean the data from the NCDC website to remove comma.


```python
columns = ['No. of Cases (Lab Confirmed)', 'No. of Cases (on admission)', 'No. Discharged', 'No. of Deaths']

for col in columns:
    Nigera_NCDC_Data[col] = Nigera_NCDC_Data[col].astype(str) 
    
columns = ['No. of Cases (Lab Confirmed)', 'No. of Cases (on admission)', 'No. Discharged', 'No. of Deaths']

for col in columns:
    Nigera_NCDC_Data[col] = Nigera_NCDC_Data[col].str.replace(',', '')
    Nigera_NCDC_Data[col] = pd.to_numeric(Nigera_NCDC_Data[col])

```

To rename the columns in Nigera_NCDC_Data for easy entry.


```python
Nigera_NCDC_Data.rename(columns={'No. of Cases (Lab Confirmed)': 'Cases', 
                    'No. of Cases (on admission)': 'Hospitalized', 
                    'No. Discharged': 'Discharged', 
                    'No. of Deaths': 'Deaths'}, inplace=True)

```

To extract data for Nigeria in the John Hopkins Repository, I first joined the extracted dfs in a new df, John_Hopkins_merged_global using the concat method.


```python
John_Hopkins_merged_global = pd.concat([John_Hopkins_confirmed_global, John_Hopkins_recovered_global, John_Hopkins_deaths_global])

```

Then from John_Hopkins_merged_global dataframe I extract all data on Nigeria and named it John_Hopkins_Nigeria_data.



```python
John_Hopkins_Nigeria_data = John_Hopkins_merged_global[John_Hopkins_merged_global['Country/Region'] == 'Nigeria']
```

To create and replace the numbered rows with desired row names.


```python
new_row_names = ['Infections', 'Discharged', 'Deaths']  
```

Update the row names


```python
John_Hopkins_Nigeria_data.index = new_row_names

```

# Analysis.

After cleaning the data I used the dataframe to do the following.

A. Get information for the highest confirmed cases.
Using DataFrame from NCDC website I named 'Nigera_NCDC_Data' I want to plot the top ten rows based on the values in the column 'Cases'


```python
top_ten = Nigera_NCDC_Data.nlargest(10, 'Cases')

# Create a bar plot

top_ten.plot(kind='bar', x='States Affected', y='Cases')

# Show the plot

plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/5a4ae6e1-ecbd-43c1-b381-7f93e0b2487b)



B. Retrieve information on the highest discharged cases.

Using DataFrame from NCDC website I named 'Nigera_NCDC_Data' I want to plot the top ten rows based on the values in the column 'Discharged'


```python
top_ten = Nigera_NCDC_Data.nlargest(10, 'Discharged')

# Create a bar plot
top_ten.plot(kind='bar', x='States Affected', y='Discharged')

# Show the plot
plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/85172445-b0c7-447c-bdfd-156df0f60574)

  
C. Get information on the States with the highest deaths. 
Using DataFrame from NCDC website I named 'Nigera_NCDC_Data' I want to plot the top ten rows based on the values in the column 'Deaths'


```python
top_ten = Nigera_NCDC_Data.nlargest(10, 'Deaths')

# Create a bar plot
top_ten.plot(kind='bar', x='States Affected', y='Deaths')

# Show the plot
plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/e562904b-8540-4f76-ae7e-1febaf7aac3a)

D. To generate a plot for the total, confirmed, discharged and death cases from the John Hopkins dataframe for Nigeria.


```python
rows_to_plot = ['Infections', 'Discharged', 'Deaths']

# Remove the first four columns because they are not numeric
John_Hopkins_Nigeria_data = John_Hopkins_Nigeria_data.iloc[:, 4:]

# Create a new figure with a specific size (width=10, height=6)
plt.figure(figsize=(15, 10))

# To plot the selected rows
for row in rows_to_plot:
    plt.plot(John_Hopkins_Nigeria_data.columns, John_Hopkins_Nigeria_data.loc[row])

# Customize the chart
plt.legend(rows_to_plot)
plt.xlabel('Daily')
plt.ylabel('Rates (Log Scale)')
plt.title('Nigeria Covid19')

# Use a logarithmic scale for the y-axis
plt.yscale('log')

plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/26919edd-05a5-47c0-827f-23e6dfc6a983)

```python
John_Hopkins_Nigeria_data_trans = John_Hopkins_Nigeria_data.transpose()

# Calculate the cumulative sum for each title
John_Hopkins_Nigeria_data_trans_cumulative = John_Hopkins_Nigeria_data.cumsum(axis=1)

# Create the plot
plt.figure(figsize=(30, 10))

for title in John_Hopkins_Nigeria_data_trans_cumulative.index:
    plt.plot(John_Hopkins_Nigeria_data_trans_cumulative.columns, John_Hopkins_Nigeria_data_trans_cumulative.loc[title], marker='o', label=title)

plt.title('Cumulative Sum of Daily Rates')
plt.ylabel('Day')
plt.xlabel('Cumulative Sum')

plt.yscale('log')

plt.legend()
plt.grid(True)
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/ef5c8b58-d1d0-4b00-9185-1fd648423ab5)


E. To calculate the daily infection rate rate using the diff() method.


```python
John_Hopkins_Nigeria_data_transposed = John_Hopkins_Nigeria_data.T

# Calculate the daily difference in 'Infections' and store it in a new column 'daily_infections'
John_Hopkins_Nigeria_data_transposed['daily_infections'] = John_Hopkins_Nigeria_data_transposed['Infections'].diff()

# Find the date with the highest infection rate
date_highest_infection = John_Hopkins_Nigeria_data_transposed['daily_infections'].idxmax()

# Find the highest infection rate
highest_infection_rate = John_Hopkins_Nigeria_data_transposed['daily_infections'].max()

# Plot daily infections
plt.figure(figsize=(15,8))
John_Hopkins_Nigeria_data_transposed['daily_infections'].plot(kind='line', title='Daily Infections Over Time')
plt.xlabel('Date')
plt.ylabel('Daily Infections')

# Highlight the date with the highest infection rate
plt.axvline(x=date_highest_infection, color='r', linestyle='--', label=f'Highest Infection Rate ({highest_infection_rate}) on {date_highest_infection}')
plt.legend()

plt.show()

![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/bd0350d1-d6df-423c-88ea-e2445a18a97d)

F. Having isolated the date with the highest infection rate, I tried to see if this also reflected the highest daily death. This was not so.


```python
# Calculate the daily difference in 'Infections' and store it in a new column 'daily_infections'
John_Hopkins_Nigeria_data_transposed['daily_deaths'] = John_Hopkins_Nigeria_data_transposed['Deaths'].diff()

# Find the date with the highest infection rate
date_highest_death = John_Hopkins_Nigeria_data_transposed['daily_deaths'].idxmax()

# Find the highest infection rate
highest_death_rate = John_Hopkins_Nigeria_data_transposed['daily_deaths'].max()

# Plot daily infections
plt.figure(figsize=(15,8))
John_Hopkins_Nigeria_data_transposed['daily_deaths'].plot(kind='line', title='Daily Deaths Over Time')
plt.xlabel('Date')
plt.ylabel('Daily Deaths')

# Highlight the date with the highest death rate
plt.axvline(x=date_highest_death, color='r', linestyle='--', label=f'Highest Death Rate ({highest_death_rate}) on {date_highest_death}')
plt.legend()

plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/eabd36fe-9baf-4558-b02a-611720fc35ec)


Nigeria recorded the highest infection rate of 6,158 on December 22, 2021 and the highest death rate of 93 on August 29, 2021. This shows that a high daily infection rate did not necessarily lead to a higher death rate afterwards.

G Combining external dataset and NCDC data


```python
# Merge the datasets on the 'states' column
merged_budget_NCDC_data = pd.merge(Nigeria_budget_data, Nigera_NCDC_Data, left_on='states', right_on='States Affected')


```

G. To determine the relationship between the external dataset and and NCDC data


```python
# Creating a new df by merging the DataFrames Nigeria_covid_external and Nigeria_NCDC_Data on the column states
merged_Nigera_data = pd.merge(Nigeria_covid_external, Nigera_NCDC_Data, left_on='states', right_on = 'States Affected')
merged_Nigera_data_top10 = merged_Nigera_data.sort_values('Cases', ascending=False)
```


```python
# First using a regression plot to generate linear relationship - Cases and Population density
plt.figure(figsize=(15,8))

sns.regplot(x ='Population Density', y ='Cases', data=merged_Nigera_data_top10)

plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/6455c38b-6b43-4764-b9c8-230dc38598c9)


```python
# Plotting columns 'Population_Density' against the log of 'Cases'
plt.figure(figsize=(30,8))
plt.scatter(merged_Nigera_data['Population Density'], np.log(merged_Nigera_data['Cases']))
plt.xlabel('Population Density')
plt.ylabel('Cases')
plt.title('Population Density vs Cases')
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/8360a6be-0d14-4e32-ae15-d927af48e1f2)


```python
# Plotting columns 'Population_Density' against the log of 'Cases'
plt.figure(figsize=(20,10))
plt.scatter(merged_Nigera_data_top10['Population Density'], np.log(merged_Nigera_data_top10['Cases']))
plt.xlabel('Population Density')
plt.ylabel('Cases')
plt.title('Population Density vs Cases')
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/7a4b2e0c-b3d4-4377-bd43-bce262f435fd)


The cases numbers appear to be higher in areas with high population densities with few exceptions including Oyo, Delta, Ogun and Kano states.


```python
# Generate line plots to compare two columns - Overall CCVI and Cases.
# Create a figure and a set of subplots
merged_Nigera_data_top10 = merged_Nigera_data.nlargest(10, 'Cases')
merged_Nigera_data_top10.set_index('states', inplace=True)
fig, ax1 = plt.subplots(figsize =(15, 8))

# Plot 'Overall CCVI Index' on the first y-axis
color = 'tab:green'
ax1.set_ylabel('Overall CCVI Index', color=color)
ax1.plot(merged_Nigera_data_top10.index, merged_Nigera_data_top10['Overall CCVI Index'], color=color)
ax1.tick_params(axis='y', labelcolor=color)

# Create a second y-axis for 'Cases'
ax2 = ax1.twinx()
color = 'tab:purple'
ax2.set_ylabel('Cases', color=color)
ax2.plot(merged_Nigera_data_top10.index, merged_Nigera_data_top10['Cases'], color=color)
ax2.tick_params(axis='y', labelcolor=color)

# Show the plot
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/9336269a-9bf7-42f2-8c59-27899841876e)

From the plot above, overall CCVI did not appear to have had any influence on the cases recorded.


```python
fig, ax1 = plt.subplots(figsize=(10, 6))

# Plot 'Population Density' on the first y-axis
color = 'tab:green'
ax1.set_ylabel('Population Density', color=color)
ax1.plot(merged_Nigera_data_top10['Population Density'], color=color)
ax1.tick_params(axis='y', labelcolor=color)

# Create a second y-axis for 'Cases'
ax2 = ax1.twinx()
color = 'tab:purple'
ax2.set_ylabel('Cases', color=color)
ax2.plot(merged_Nigera_data_top10['Cases'], color=color)
ax2.tick_params(axis='y', labelcolor=color)

# Show the plot
plt.show()

```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/1b78f7f9-8d87-405c-86cb-95d3f1744414)


Except for Kano, Oyo, Delta and Ogun, the population density had a great influence on the case numbers.

H. Comparing the NCDC Data with Budget data. First was to see the percentage difference on the budget.

Second was to merge the datasets (budget with the NCDC dataset) on the 'states' column

No indication that the number of cases has any direct effect on the budget cuts.


```python
Nigeria_budget_data['percentage_difference'] = ((Nigeria_budget_data['Revised_budget (Bn)'] - Nigeria_budget_data['Initial_budget (Bn)']) / Nigeria_budget_data['Initial_budget (Bn)']) * 100
```


```python
merged_budget_NCDC_data = pd.merge(Nigeria_budget_data, Nigera_NCDC_Data, left_on='states', right_on='States Affected')
```


```python
# Create a line plot compare.

fig, ax1 = plt.subplots(figsize =(30, 12))

# Plot 'Cases' on the first y-axis
color = 'purple'
ax1.set_xlabel('States')
ax1.set_ylabel('Cases', color=color)
ax1.plot(merged_budget_NCDC_data['States Affected'], merged_budget_NCDC_data['Cases'], color=color)
ax1.tick_params(axis='y', labelcolor=color)

# Create a second y-axis for 'Percentage Difference'
ax2 = ax1.twinx()
color = 'green'
ax2.set_ylabel('Percentage Difference', color=color)
ax2.plot(merged_budget_NCDC_data['States Affected'], merged_budget_NCDC_data['percentage_difference'], color=color)
ax2.tick_params(axis='y', labelcolor=color)

# Display the plot
plt.title('Relationship between Nigera_NCDC_Data and Nigeria_budget_data')
plt.show()

![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/3b3506c8-6a04-44a2-a748-bf73bd464ab3)

```

I. The final step was compare the real GDP data for seven years starting from 2014  to visualize the effect of the pandemic on the economy.


```python
Nigeria_realGDP_melt = pd.melt(Nigeria_realGDP, id_vars='Year', var_name='Quarter', value_name='Value')

# Create a bar plot
plt.figure(figsize=(12,6))
sns.barplot(x='Year', y='Value', hue='Quarter', data=Nigeria_realGDP_melt)
# barplot = sns.barplot(x='Year', y='Value', hue='Quarter', data=dfu3_melt)


plt.title('Bar Plot')
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/1cbd5622-48b3-457d-ae55-05c7f71e46d8)


```python
# Reshape the DataFrame with pd.melt()
Nigeria_realGDP_melt = pd.melt(Nigeria_realGDP, id_vars='Year', var_name='Quarter', value_name='Value')

# Create a bar plot
plt.figure(figsize=(16,6))
barplot = sns.barplot(x='Year', y='Value', hue='Quarter', data=Nigeria_realGDP_melt)

# Draw a horizontal line at the value of Q2 in year 2020
q2_2020_value = Nigeria_realGDP.loc[Nigeria_realGDP['Year'] == 2020, 'Q2'].values[0]
plt.axhline(y=q2_2020_value, color='black', linestyle='--')

# Move the legend to lower left and set labels
barplot.legend(loc='lower left', labels=['Q1', 'Q2', 'Q3', 'Q4'])

plt.title('Bar Plot')
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/b18fda0c-3c7d-42a8-9aa7-e9b1323a868f)


Then finally was to compare the yearly quarters separately. The effect was clearly noticeable.


```python
# Reshaping the DataFrame with pd.melt()
Nigeria_realGDP_melt = pd.melt(Nigeria_realGDP, id_vars='Year', var_name='Quarter', value_name='Value')

# Create a figure and axes for 4 subplots (one for each quarter)
fig, axs = plt.subplots(4, figsize=(12,24))

quarters = ['Q1', 'Q2', 'Q3', 'Q4']
for i, ax in enumerate(axs):
    # Create a bar plot for each quarter
    quarter_data = Nigeria_realGDP_melt[Nigeria_realGDP_melt['Quarter'] == quarters[i]]
    sns.barplot(x='Year', y='Value', data=quarter_data, ax=ax)
    
    # Draw a horizontal line at the value of each quarter in year 2020
    q_2020_value = Nigeria_realGDP.loc[Nigeria_realGDP['Year'] == 2020, quarters[i]].values[0]
    ax.axhline(y=q_2020_value, color='black', linestyle='--')
    
    ax.set_title(quarters[i])

plt.tight_layout()
plt.show()
```
![image](https://github.com/iroanya/Nigeria-COVID-19-Data-Analysis-Using-Python/assets/142989933/28c2c516-c179-4df1-9836-c8f305596cbf)



#           Observations.

The external dataset and the NCDC data represent health data from all the states of Nigeria. Combining these two dataframes, the following observations were made. 

1. There were some problems noticed on the NCDC data. Eg, some states recorded zero hospitalizations but with a number of discharged cases. How they were discharged was not captured for clarity purposes.
 
2. The vulnerability index did not influence the COVID-19 cases recorded in the states. Lagos with the least CCVI of 0.0 had more cases than all the states in the top ten, including Kaduna with the highest CCVI of 0.7.
   
3. However, when the populations are compared, the case numbers appear to be more on the states with with higher populations except for Kano state.
   
4. Edo state recorded 7,928 confirmed cases, zero hospitalizations, but with a mortality rate of almost 5%. This is the highest mortality rate, making Edo State an outlier.
 
5. Using a regression plot to plot the population density and infections rate, it was observed that Lagos and Rivers were the only states with high population densities and high infections rates. FCT and Kano were outliers with high population densities and infection rates below the regression line (see attached graph in the workbook).

6. The budget cuts has no direct correlation with the cases numbers. Cross River State had the highest cut.

7. Nigeria recorded the highest infection rate of 6,158 on December 22, 2021 and the highest death rate of 93 on August 29, 2021. This shows that a high daily infection rate did not necessarily lead to a higher death rate afterwards.






#          Economic Effects.

Plotting bar charts from the extracted GDP data. The following were observed. See attached plots.

1. From 2014, the GDP grew from the first quarter and climaxed by the fourth quarter of every year.
   
2. There was an observable difference on the general improvements year on year, except for 2015 and 2016 which returned almost identical data.
   
3. The effect of the pandemic is most noticeable when data from the first quarters are compared. 2020 outperformed the first quarters of the previous years. At the second quarter, which was at the very peak of the pandemic with shutdowns and zero economic activities, the GDP reflected this by going down.
   
4. However, by the third quarter, after the lockdowns due to the pandemic and reopening of businesses, the GDP rebounded.


#            Conclusion.

From the studied data it can be deduced that the pandemic had a negative effect on the economy. However, the economy was resilient enough to rebound as soon as the pandemic waned. This recovery speed can only be attributed to an economy that is very robust.

The level of budget cuts was not directly influenced by the infection rates but by the adverse effects of the shutdown.

Again, population densities and CCVI did not affect the overall trend of the pandemic spread. It is worthy of note that the Lagos State health system worked wonderfully well in the treatment and discharge of confirmed cases, thereby averting a high death toll.

The daily recorded infection rate did not translate to a higher mortality rate. Death rates were higher at the beginning of the pandemic and waned later on. This can be ascribed to the fact that as the medical response became more effective the survival rate improved.


```python

