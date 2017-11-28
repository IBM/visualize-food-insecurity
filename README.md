
## Visualizing Food Insecurity with Pixie Dust and Watson Analytics
_IBM code pattern showing how to visualize US Food Insecurity with Pixie Dust and Watson Analytics._

This code pattern will guide you through downloading, cleaning and visualizing data using different tools. In particular this code pattern showcases food insecurity in the US, along with its associated factors.

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating our findings or communicating them ineffectively to non data science audiences. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that might not have had the same opportunity to work on a project of the same subject. By visualizing our findings and those insights that have the most power to do social good, we can bring awareness and maybe even change. This code pattern walks you through how to do just that, with IBM's Data Science Experience (DSX), Pandas, Pixie Dust and Watson Analytics.

For this particular code pattern, food insecurity throughout the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. For some context, this problem is a more and more relevant problem for the United States as obesity and diabetes rise and two out of three adult Americans are considered obese, one third of American minors are considered obsese, nearly ten percent of Americans have diabetes and nearly fifty percent of the African American population have heart disease. Even more, cardiovascular disease is the leading global cause of death, accounting for 17.3 million deaths per year, and rising. Native American populations more often than not do not have grocery stores on their reservation... and all of these trends are on the rise. The problem lies not only in low access to fresh produce, but food culture, low education on healthy eating as well as racial and income inequality.

The government data that I use in this code pattern has been conveniently combined into a dataset for our use, which you can find in this repo under combined_data.csv. You can find the original, government data from the US Bureau of Labor Statistics https://www.bls.gov/cex/ and The United States Department of Agriculture https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/.

![alt text](https://github.com/MadisonJMyers/Visualizing-Food-Insecurity-with-Pixie-Dust-and-Watson-Analytics/blob/master/doc/source/images/Architecture.png)

## Flow

1. Open DSX and create a notebook.
2. Download the data in DSX and explore it.
3. Load Pixie Dust and use for visualizations.
4. Download dataframe as a csv from DSX.
5. Upload the csv to Watson Analytics and visualize.

## Included components

* [IBM Data Science Experience](https://www.ibm.com/bs-en/marketplace/data-science-experience): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.
* [Jupyter Notebook](http://jupyter.org/): An open source web application that allows you to create and share documents that contain live code, equations, visualizations, and explanatory text.
* [PixieDust](https://github.com/ibm-watson-data-lab/pixiedust): Provides a Python helper library for IPython Notebook.
* [Watson Discovery](https://www.ibm.com/watson/developercloud/discovery.html): A cognitive search and content analytics engine for applications to identify patterns, trends, and actionable insights.

## Featured technologies

* [Cloud](https://www.ibm.com/developerworks/learn/cloud/): Accessing computer and information technology resources through the Internet.
* [Data Science](https://medium.com/ibm-data-science-experience/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
* [Python](https://www.python.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.
* [pandas](http://pandas.pydata.org/): A Python library providing high-performance, easy-to-use data structures.

## Watch the Video

See the template or example code patterns for how to include a link to your video with a preview. Say the YouTube video is at `https://www.youtube.com/watch?v=Jxi7U7VOMYg`, that means the video ID is `Jxi7U7VOMYg`. To generate a video preview, add the following snippet, replacing `Jxi7U7VOMYg` with the new video ID:  ``[![](http://img.youtube.com/vi/Jxi7U7VOMYg/0.jpg)](https://youtu.be/Jxi7U7VOMYg)``

## Steps

This pattern runs through the steps below. Check out the notebook for the code!



    1. Sign up for the Data Science Experience
    2. Create the notebook
    3. Upload your data as a data asset into DSX.
    4. Load the data (in the right hand corner select the '1001' button and select import as a pandas dataframe.)
    5. Explore the data in python and visualize the correlations.
    6. Remove irrelevant variables as well as 0 and NaN values and create a new, smaller dataframe.
    7. Create a heatmap of the correlations in the new dataframe.
    8. Explore and visualize these connections using Pixie Dust, matplotlib and seaborn.
    9. Run the notebook
    10. Save and Share
    11. Download our new dataframe from DSX.
    12. Upload our new dataframe csv into Watson Analytics.
    13. Check out the discoveries that Watson Analytics offers.
    14. Suggest different relationships to visualize in the display section of Watson Analytics.

1. Sign up for the Data Science Experience

Sign up for IBM's Data Science Experience. By signing up for the Data Science Experience, two services: DSX-Spark and DSX-ObjectStore will be created in your Bluemix account. If these services do not exist, or if you are already using them for some other application, you will need to create new instances.

To create these services:

    Login to your Bluemix account.
    Create your Spark service by selecting the service type Apache Spark. If not already used, name your service DSX-Spark.
    Create your Object Storage service by selecting the service type Cloud Object Storage. If not already used, name your service DSX-ObjectStorage.

    Note: When creating your Object Storage service, select the Swift storage type in order to avoid having to pay an upgrade fee.

Take note of your service names as you will need to select them in the following steps.

2. Create the notebook

First you must create a new Project:

    From the IBM Data Science Experience page either click the Get Started tab at the top or scroll down to Recently updated projects.
    Click on New project under Recently updated projects.
    Enter a Name and optional Description.
    For Spark Service, select your Apache Spark service name.
    For Storage Type, select the Object Storage (Swift API) option.
    For Target Object Storage Instance, select your Object Storage service name.
    Click Create.

Create the Notebook:

    Click on your project to open up the project details panel.
    Click add notebooks.
    Click the tab for From URL and enter a Name and optional Description.
    For Notebook URL enter: https://github.com/IBM/spark-tpc-ds-performance-test/blob/master/notebooks/run-tpcds-on-spark.ipynb
    For Spark Service, select your Apache Spark service name.
    Click Create Notebook.
    
3. Upload your data as a data asset into DSX.

   To begin, I used the combined_data.csv as my data asset. You'll want to upload it as a data asset and once that is    complete, go into your notebook in the edit mode (click on the pencil icon next to your notebook on the dashboard). To load your data in your notebook, you'll click on the "1001" data icon in the top right. The combined_data.csv should show up. Click on it and select "Insert Pandas Data Frame". Once you do that, a whole bunch of code will show up in your first cell. Once you see that, run the cell and follow along with my tutorial!
   
4. Load the data (in the right hand corner select the '1001' button and select import as a pandas dataframe.)
5. Explore the data in python and visualize the correlations.
6. Remove irrelevant variables as well as 0 and NaN values and create a new, smaller dataframe.
7. Create a heatmap of the correlations in the new dataframe.
8. Explore and visualize these connections using Pixie Dust, matplotlib and seaborn.

   To activate Pixie Dust, we just import it and then write:

```display(your_dataframe_name)```

   After doing this your dataframe will show up in a column-row table format. To visualize your data, you can click the chart icon at the top left (looks like an arrow going up). From there you can choose from a variety of visuals. Once you select the type of chart you want, you can then select the variables you want to showcase. It's worth playing around with this to see how you can create the most effective visualizations for your audience. The notebook below showcases a couple options such as scatterplots, bar charts, line charts, and histograms.
   
9. Run the notebook

When a notebook is executed, what is actually happening is that each code cell in the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag format is In [x]:. Depending on the state of the notebook, the x can be:

    A blank, this indicates that the cell has never been executed.
    A number, this number represents the relative order this code step was executed.
    A *, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

    One cell at a time.
        Select the cell, and then press the Play button in the toolbar.
    Batch mode, in sequential order.
        From the Cell menu bar, there are several options available. For example, you can Run All cells in your notebook, or you can Run All Below, that will start executing from the first cell under the currently selected cell, and then continue executing all cells that follow.
    At a scheduled time.
        Press the Schedule button located in the top right section of your notebook panel. Here you can schedule your notebook to be executed once at some future time, or repeatedly at your specified interval.

10. Save and Share
How to save your work:

Under the File menu, there are several ways to save your notebook:

    Save will simply save the current state of your notebook, without any version information.
    Save Version will save your current state of your notebook with a version tag that contains a date and time stamp. Up to 10 versions of your notebook can be saved, each one retrievable by selecting the Revert To Version menu item.

How to share your work:

You can share your notebook by selecting the “Share” button located in the top right section of your notebook panel. The end result of this action will be a URL link that will display a “read-only” version of your notebook. You have several options to specify exactly what you want shared from your notebook:

    Only text and output: will remove all code cells from the notebook view.
    All content excluding sensitive code cells: will remove any code cells that contain a sensitive tag. For example, # @hidden_cell is used to protect your dashDB credentials from being shared.
    All content, including code: displays the notebook as is.
    A variety of download as options are also available in the menu.



11. Download our new dataframe from DSX.

    Unfortunately, in DSX we cannot download our dataframe as a csv in one line of code, but we can download it to DSX so that   it can be downloaded and used elsewhere as well as for other projects. I demonstrate how to do this in the notebook.

    Once you follow along, you can take the new .csv (found under "Data Services" --> "Object Storage" from the top button) and upload it to Watson Analytics. Again, if you do not have an account, you'll want to set one up. Once you are logged in and ready to go, you can upload the data (saved in this repo as df_focusedvalues.csv) to your Watson platform.
    
12. Upload our new dataframe csv into Watson Analytics.

13. Check out the discoveries that Watson Analytics offers.

    Once you've set up your account, you can see taht the Watson plaform has three sections: data, discover and display. You uploaded your data to the "data" section, but now you'll want to go to the "discover" section. Under "discover" you can select your dataframe dataset for use. Once you've selected it, the Watson platform will suggest different insights to visualize. You can move forward with its selections or your own, or both. You can take a look at mine here (you'll need an account to view): https://ibm.co/2xAlAkq or see the screen shots attached to this repo. You can also go into the "display" section and create a shareable layout like mine (again you'll need an account): https://ibm.co/2A38Kg6.

14. Suggest different relationships to visualize in the display section of Watson Analytics.

    You can see that with these visualizations the user can see the impact of food insecurity by state, geographically distributed and used aid such as reduced school lunches, a map of diabetes by state, a predictive model for food insecurity and diabetes (showcasing the factors that, in combination, suggest a likelihood of food insecurity), drivers of adult diabetes, drivers of food insecurity, the relationship with the frequency of farmers market locations, food insecurity and adult obesity, as well as the relationship between farmers markets, the percent of the population that is Asian, food insecurity and poverty rates.

    By reviewing our visualizations both in DSX and Watson, we learn that obesity and diabetes almost go hand in hand, along with food insecurity. We can also learn that this seems to be an inequality issue, both in income and race, with Black and Hispanic populations being more heavily impacted by food insecurity and diet-related diseases than those of the White and Asian populations. We can also see that school-aged children who qualify for reduced lunch are more likely obese than not whereas those that have a farm-to-school program are more unlikely to be obese.

    Like many data science investigations, this analysis could have a big impact on policy and people's approach to food insecurity in the U.S. What's best is that we can create many projects much like this in a quick time period and share them with others by using Pandas, Pixie Dust as well as Watson's predictive and recommended visualizations.

## Sample output

Use a final _Analyze the results_ or _Conclusion_ step above, plus sample output to wrap up the code pattern for a developer running it and also for people that are only going to read the README. Sample output format will vary depending on the code pattern technology used.

## Links

 - DSX:https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html.
 - IBM Watson Analytics: https://www.ibm.com/watson-analytics
 - Pandas:http://pandas.pydata.org/
 - Pixie Dust: https://ibm-watson-data-lab.github.io/pixiedust/displayapi.html#introduction
 - Data:https://www.bls.gov/cex/ ; https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/.


## Troubleshooting

Please follow the example format or contribute templates or suggestions. Break out troubleshooting into it's own .MD file when the section gets long.



Copyright 2017 IBM Corp. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

   ```http://www.apache.org/licenses/LICENSE-2.0```

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
