# Nigeria-COVID-19-Data-Analysis-Using-Python To be edited later.
Data Collection.

I collected the required data from the website and other sources as directed. 

A. From John Hopkins GitHub repository.

B From NCDC website. However, I have problems scraping data from the website as the COVID19 data on the NCDC website appears not to be functional. I had to copy saved data to my pc and used that for the dataframe. 

C. I took data from the Ustacky Github repository to complete the data collection process.

View The Data.

I used the standard head() and info() method to view the information in the dataframes.

Data Cleaning and Preparation

Refer to included workbook, Ustacky Capstone final for data cleaning methods used.

Analysis.

After cleaning the data I the dataframe to do the following.

A. Get information for the highest confirmed cases.

B. Retrieve information on the highest discharged cases.

C. Get information on the States with the highest deaths. 

D. Combine external dataset and the NCDC data to form a dataframe.

Observations.

The external dataset and the NCDC data represent health data from the all the states of Nigeria. Combining these two dataframes, the following observations were made. 

1. There were some problems noticed on the NCDC data. Eg, some states recorded zero hospitalizations but with a number of discharged cases. How they were discharged was not captured for clarity purposes.
 
2. The vulnerabilty index did not influence the covid-19 cases recorded in the states. Lagos with the least CCVI of 0.0 had more cases than all the top ten states including Kaduna with the highest CCVI of 0.7.
   
3. However, when the populations are compared, the case density appears to be more on the states with with higher populations except Kano state.
   
4. Edo state recorded 7,928 confirmed cases, zero hospitalizations, but with a mortality rate of almost 5%. This is the highest mortlity rate, making Edo State an outlier.
 
5. Using a regression plot to plot the population density and infections rate, it was observed that Lagos and Rivers were outliers with high population densities and infections. FCT and Kano were also outliers with high population densities and infection rates below the regression line (see attached graph in the workbook).

Economic Effects.

Plotting bar charts from the extracted GDP data. The following were observed. See attached workbook for the plots.

1. From 2014, the GDP grew from the first quarter and climaxed by the fourth quarter of every year.
   
2. There was an observable difference on the general improvements year on year, except for 2015 and 2016 which returned almost identical data.
   
3. The effect of the pandemic is most noticeable when data from the first quarters are compare. 2020 outperformed the first quarters of the previous years. As soon as the effects of the pandemic kicked in the second quarter, the GDP crashed.
   
4. At the third quarter, after the peak of thye pandemic, the GDP rebounded.

Conclusion.

From the studied data it can be deduced that the pandemic had a negative effect of the economy. However, the economy was resilient enough to rebound as soon as the pandemic waned. This recovery can only be attributed to a very robust economy.
   

