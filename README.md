# Nigeria-COVID-19-Data-Analysis-Using-Python To be edited later.
Data Collection
I collected the required data from the website and other sources as directed. As described in the project instructions, you will perform a web scrap to obtain data from the NCDC website, import Task 1 - Data Collection
Here you will obtain the required data for the analysis. As described in the project instructions, you will perform a web scrap to obtain data from the NCDC website, import data from the John Hopkins repository, and import the provided external data.
Task 2 - View the data
Obtain basic information about the data using the head() and info() method..
Task 3 - Data Cleaning and Preparation
From the information obtained above, you will need to fix the data format.


Examples: * Convert to appropriate data type. * Rename the columns of the scraped data. * Remove comma(,) in numerical data * Extract daily data for Nigeria from the Global daily cases data
TODO A - Clean the scraped data
TODO B - Get a Pandas DataFrame for Daily Confirmed Cases in Nigeria. Columns are Date and Cases

TODO C - Get a Pandas DataFrame for Daily Recovered Cases in Nigeria. Columns are Date and Cases

TODO D - Get a Pandas DataFrame for Daily Death Cases in Nigeria. Columns are Date and Cases

Task 4 - Analysis
Here you will perform some analyses on the datasets. You are welcome to communicate findings in charts and summary.


We have included a few TODOs to help with your analysis. However, do not let this limit your approach, feel free to include more, and be sure to support your findings with chart and summaryTask 4 - Analysis
Here you will perform some analyses on the datasets. You are welcome to communicate findings in charts and summary.


We have included a few TODOs to help with your analysis. However, do not let this limit your approach, feel free to include more, and be sure to support your findings with chart and summary
TODO A - Generate a plot that shows the Top 10 states in terms of Confirmed Covid cases by Laboratory test

TODO B - Generate a plot that shows the Top 10 states in terms of Discharged Covid cases. Hint - Sort the values

TODO D - Plot the top 10 Death cases

TODO E - Generate a line plot for the total daily confirmed, recovered and death cases in Nigeria

TODO F -

Determine the daily infection rate, you can use the Pandas diff method to find the derivate of the total cases.
Generate a line plot for the above
TODO G -

Calculate maximum infection rate for a day (Number of new cases)
Find the date
TODO H - Determine the relationship between the external dataset and the NCDC COVID-19 dataset. Here you will generate a line plot of top 10 confirmed cases and the overall community vulnerability index on the same axis. From the graph, explain your observation.


Steps * Combine the two dataset together on a common column(states) * Create a new dataframe for plotting. This DataFrame will contain top 10 states in terms of confirmed cases i.e sort by confirmed cases. ** Hint: Check out Pandas [nlargest](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.nlargest.html) function. This [tutorial](https://cmdlinetips.com/2019/03/how-to-select-top-n-** * Plot both variable on the same axis. Check out this [tutorial](http://kitchingroup.cheme.cmu.edu/blog/2013/09/13/Plotting-two-datasets-with-very-different-scales/)
TODO I - Determine the relationship between the external dataset and the NCDC COVID-19 dataset.

Here you will generate a regression plot between two variables to visualize the linear relationships - Confirmed Cases and Population Density.
Hint: Check out Seaborn Regression Plot.

Provide a summary of your observation
TODO J -

Provide more analyses by extending TODO G & H. Meaning, determine relationships between more features.
Provide a detailed summary of your findings.
Note that you can have as many as possible.
TODO L -
Determine the effect of the Pandemic on the economy. To do this, you will compare the Real GDP value Pre-COVID-19 with Real GDP in 2020 (COVID-19 Period, especially Q2 2020)


Steps * From the Real GDP Data, generate a `barplot` using the GDP values for each year & quarters. For example: On x-axis you will have year 2017 and the bars will be values of each quarters(Q1-Q4). You expected to have subplots of each quarters on one graph.
Hint: Use [Pandas.melt](https://pandas.pydata.org/docs/reference/api/pandas.melt.html) to create your plot DataFrame * Set your quarter legend to lower left. * Using `axhline`, draw a horizontal line through the graph at the value of Q2 2020. * Write out your observation
Note: Do not limit your analysis to the provided TODOs. Perform more analyses e.g
Check for more external dataset
Ask more questions & find the right answers by exploring the data
