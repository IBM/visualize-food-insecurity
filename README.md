## Visualizing Food Insecurity with Pixie Dust and Watson Analytics

This code pattern will guide you through downloading, cleaning and visualizing data using different tools. In particular this code pattern showcases food insecurity in the US, along with its associated factors.

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating our findings or communicating them ineffectively to non data science audiences. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that might not have had the same opportunity to work on a project of the same subject. By visualizing our findings and those insights that have the most power to do social good, we can bring awareness and maybe even change. This code pattern walks you through how to do just that, with IBM's Data Science Experience (DSX), Pandas, Pixie Dust and Watson Analytics.

For this particular code pattern, food insecurity throughout the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. For some context, this problem is a more and more relevant problem for the United States as obesity and diabetes rise and two out of three adult Americans are considered obese, one third of American minors are considered obsese, nearly ten percent of Americans have diabetes and nearly fifty percent of the African American population have heart disease. Even more, cardiovascular disease is the leading global cause of death, accounting for 17.3 million deaths per year, and rising. Native American populations more often than not do not have grocery stores on their reservation... and all of these trends are on the rise. The problem lies not only in low access to fresh produce, but food culture, low education on healthy eating as well as racial and income inequality.

The government data that I use in this code pattern has been conveniently combined into a dataset for our use, which you can find in this repo under combined_data.csv. You can find the original, government data from the US Bureau of Labor Statistics https://www.bls.gov/cex/ and The United States Department of Agriculture https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/.

![](doc/source/images/Architecture.png)

## Flow

1. Open DSX and create a notebook.
2. Download the data in DSX and explore it.
3. Load Pixie Dust and use for visualizations.
4. Download dataframe as a csv from DSX.
5. Upload the csv to Watson Analytics and visualize.

## Included components

* [IBM Data Science Experience](https://www.ibm.com/bs-en/marketplace/data-science-experience): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.
* [IBM Watson Analytics](https://www.ibm.com/watson-analytics): NEED BLURB HERE
* [Jupyter Notebook](http://jupyter.org/): An open source web application that allows you to create and share documents that contain live code, equations, visualizations, and explanatory text.
* [PixieDust](https://github.com/ibm-watson-data-lab/pixiedust): Provides a Python helper library for IPython Notebook.
* [Watson Discovery](https://www.ibm.com/watson/developercloud/discovery.html): A cognitive search and content analytics engine for applications to identify patterns, trends, and actionable insights.

## Featured technologies

* [Cloud](https://www.ibm.com/developerworks/learn/cloud/): Accessing computer and information technology resources through the Internet.
* [Data Science](https://medium.com/ibm-data-science-experience/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Python](https://www.python.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.
* [pandas](http://pandas.pydata.org/): A Python library providing high-performance, easy-to-use data structures.

# Watch the Video

!!! COMING

# Steps

This pattern runs through the steps below. Check out the notebook for the code!

1. Log in to DSX (or create an account if this is your first time.) Here's a tutorial on getting started with DSX: https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html.
2. Upload your data as a data asset into DSX.
3. Start a notebook and load the data (in the right hand corner select the '1001' button and select import as a pandas dataframe.
4. Explore the data in python and visualize the correlations.
5. Remove irrelevant variables as well as 0 and NaN values and create a new, smaller dataframe.
6. Create a heatmap of the correlations in the new dataframe.
7. Explore and visualize these connections using Pixie Dust, matplotlib and seaborn.
8. Download our new dataframe from DSX.
9. Upload our new dataframe csv into Watson Analytics.
10. Check out the discoveries that Watson Analytics offers.
11. Suggest different relationships to visualize in the display section of Watson Analytics.


# Sample output

Use a final _Analyze the results_ or _Conclusion_ step above, plus sample output to wrap up the code pattern for a developer running it and also for people that are only going to read the README. Sample output format will vary depending on the code pattern technology used.

# Troubleshooting

Please follow the example format or contribute templates or suggestions. Break out troubleshooting into it's own .MD file when the section gets long.

# Links

* [Create Data Science Experience Notebooks](https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html)
* [Bureau of Labor Statistics](https://www.bls.gov/cex/)
* [Food Environment Atlas](https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/)

# Learn more

* **Data Analytics Code Patterns**: Enjoyed this Code Pattern? Check out our other [Data Analytics Code Patterns](https://developer.ibm.com/code/technologies/data-science/)
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **Data Science Experience**: Master the art of data science with IBM's [Data Science Experience](https://datascience.ibm.com/)
* **Spark on IBM Cloud**: Need a Spark cluster? Create up to 30 Spark executors on IBM Cloud with our [Spark service](https://console.bluemix.net/catalog/services/apache-spark)

# License
[Apache 2.0](LICENSE)
