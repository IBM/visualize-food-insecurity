
## Visualizing-Food-Insecurity-with-Pixie-Dust-and-Watson-Analytics
_IBM Journey showing how to visualize US Food Insecurity with Pixie Dust and Watson Analytics._

## Badges

Most journeys will have badges, such as CI status and Bluemix Deployments. Badges should be at the very top, before the title. They should be side-by-side on one line.

## Title

Create Visualizations Using Pixie Dust, DSX and Watson Analytics

## Intro

This journey will guide you through downloading, cleaning and visualizing data using different tools. In particular this journey showcases food insecurity in the US, along with its associated factors.

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating our findings or communicating them ineffectively to non data science audiences. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that might not have had the same opportunity to work on a project of the same subject. By visualizing our findings and those insights that have the most power to do social good, we can bring awareness and maybe even change. This journey walks you through how to do just that, with IBM's Data Science Experience (DSX), Pandas, Pixie Dust and Watson Analytics.

For this particular journey, food insecurity throughout the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. For some context, this problem is a more and more relevant problem for the United States as obesity and diabetes rise and two out of three adult Americans are considered obese, one third of American minors are considered obsese, nearly ten percent of Americans have diabetes and nearly fifty percent of the African American population have heart disease. Even more, cardiovascular disease is the leading global cause of death, accounting for 17.3 million deaths per year, and rising. Native American populations more often than not do not have grocery stores on their reservation... and all of these trends are on the rise. The problem lies not only in low access to fresh produce, but food culture, low education on healthy eating as well as racial and income inequality.

The government data that I use in this journey has been conveniently combined into a dataset for our use, which you can find in this repo under combined_data.csv. You can find the original, government data from the US Bureau of Labor Statistics https://www.bls.gov/cex/ and The United States Department of Agriculture https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/.

## Architecture Image

![alt text](https://github.com/MadisonJMyers/Visualizing-Food-Insecurity-with-Pixie-Dust-and-Watson-Analytics/blob/master/images/FoodInsecurityArchDiagram.png "Architecture Image")

## Flow

1. Open DSX and create a notebook.
2. Download the data in DSX and explore it.
3. Load Pixie Dust and use for visualizations.
4. Download dataframe as a csv from DSX.
5. Upload the csv to Watson Analytics and visualize.

## With Watson

Copy the **With Watson** section and use it as-is (unless you don't use Watson at all).

## Included components

 - IBM Data Science Experience: Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.
 - Jupyter Notebook: An open source web application that allows you to create and share documents that contain live code, equations, visualizations, and explanatory text.
 -PixieDust: Provides a Python helper library for IPython Notebook.
 -Watson Discovery: A cognitive search and content analytics engine for applications to identify patterns, trends, and actionable insights.

## Featured technologies

 - Cloud: Accessing computer and information technology resources through the Internet.
 - Data Science: Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.
 - Python: Python is a programming language that lets you work more quickly and integrate your systems more effectively.
 - pandas: A Python library providing high-performance, easy-to-use data structures.
 - PixieDust: PixieDust is an open source helper library that works as an add-on to Jupyter notebooks to improve the user experience of working with data.
 - Jupyter Notebooks: An open-source web application that allows you to create and share documents that contain live code, equations, visualizations and explanatory text.

## Watch the Video

See the template or example journeys for how to include a link to your video with a preview. Say the YouTube video is at `https://www.youtube.com/watch?v=Jxi7U7VOMYg`, that means the video ID is `Jxi7U7VOMYg`. To generate a video preview, add the following snippet, replacing `Jxi7U7VOMYg` with the new video ID:  ``[![](http://img.youtube.com/vi/Jxi7U7VOMYg/0.jpg)](https://youtu.be/Jxi7U7VOMYg)``

## Steps

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


## Sample output

Use a final _Analyze the results_ or _Conclusion_ step above, plus sample output to wrap up the journey for a developer running it and also for people that are only going to read the README. Sample output format will vary depending on the journey technology used.

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
