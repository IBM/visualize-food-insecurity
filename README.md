# Visualizing Food Insecurity with PixieDust and Watson Analytics

This Code Pattern will guide you through downloading, cleaning and visualizing data using different tools. In particular this Code Pattern showcases food insecurity in the US, along with its associated factors.

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating our findings or communicating them ineffectively to non data science audiences. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that might not have had the same opportunity to work on a project of the same subject. By visualizing our findings and those insights that have the most power to do social good, we can bring awareness and maybe even change. This Code Pattern walks you through how to do just that, with IBM's Watson Studio, Pandas, PixieDust and Watson Analytics.

For this particular Code Pattern, food insecurity throughout the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. For some context, this problem is a more and more relevant problem for the United States as obesity and diabetes rise and two out of three adult Americans are considered obese, one third of American minors are considered obese, nearly ten percent of Americans have diabetes and nearly fifty percent of the African American population have heart disease. Even more, cardiovascular disease is the leading global cause of death, accounting for 17.3 million deaths per year, and rising. Native American populations more often than not do not have grocery stores on their reservation... and all of these trends are on the rise. The problem lies not only in low access to fresh produce, but food culture, low education on healthy eating as well as racial and income inequality.

The government data that I use in this Code Pattern has been conveniently combined into a dataset for our use, which you can find in this repo under [`combined_data.csv.zip`](data/combined_data.csv.zip). You can find the original, government data from the US Bureau of Labor Statistics https://www.bls.gov/cex/ and The United States Department of Agriculture https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/. For the data we use in the second part of this Code Pattern with Watson Analytics, you can go to [`df_focusedvalues.csv`](data/df_focusedvalues.csv) in this repo. You will need a Watson Studio account and a Watson Analytics account to run the duration of this Code Pattern, but you can follow along with the [steps](https://github.com/IBM/visualize-food-insecurity#steps) below!

![](doc/source/images/architecture.png)

#### Notebooks

* [Diet-Related-Disease-Exploratory.ipynb](notebooks/Diet-Related-Disease-Exploratory.ipynb): The notebook we'll be using with no output.
* [Diet-Related-Disease-Exploratory.ipynb](examples/R4ML_Data_Preprocessing_and_Dimension_Reduction.ipynb): The notebook with completed output.

## Flow

1. Open Watson Studio and create a notebook.
2. Download the data in Watson Studio and explore it.
3. Load Pixie Dust and use for visualizations.
4. Download dataframe as a csv from Watson Studio.
5. Upload the csv to Watson Analytics and visualize.

## Included components

* [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.
* [IBM Watson Analytics](https://www.ibm.com/watson-analytics): Provides smart data discovery, automated predictive analytics and cognitive capabilities that enables users to interact with data conversationally.
* [Jupyter Notebook](https://jupyter.org/): An open source web application that allows you to create and share documents that contain live code, equations, visualizations, and explanatory text.
* [PixieDust](https://github.com/pixiedust/pixiedust): Provides a Python helper library for IPython Notebook.

## Featured technologies

* [Python](https://www.python.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.
* [Pandas](https://pandas.pydata.org/): A Python library providing high-performance, easy-to-use data structures.

## Watch the Video

[![](https://img.youtube.com/vi/TRvABjKkcqE/0.jpg)](https://youtu.be/TRvABjKkcqE)

## Steps

This Code Pattern consists of two activities:

* [Run a Jupyter notebook in the IBM Watson Studio](#run-using-a-jupyter-notebook-in-the-ibm-watson-studio).
* [Anaylze the data in Watson Analytics](#analyze-the-data-in-watson-analytics).

## Run using a Jupyter notebook in the IBM Watson Studio

1. [Create a new Watson Studio project](#1-create-a-new-watson-studio-project)
2. [Create the notebook](#2-create-the-notebook)
3. [Upload data](#3-upload-data)
4. [Run the notebook](#4-run-the-notebook)
5. [Save and Share](#5-save-and-share)

### 1. Create a new Watson Studio project

* Log into IBM's [Watson Studio](https://dataplatform.cloud.ibm.com). Once in, you'll land on the dashboard.

* Create a new project by clicking `+ New project` and choosing `Data Science`:

  ![studio project](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/new-project-data-science.png)

* Enter a name for the project name and click `Create`.

* **NOTE**: By creating a project in Watson Studio a free tier `Object Storage` service and `Watson Machine Learning` service will be created in your IBM Cloud account. Select the `Free` storage type to avoid fees.

  ![studio-new-project](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/new-project-data-science-name.png)

* Upon a successful project creation, you are taken to a dashboard view of your project. Take note of the `Assets` and `Settings` tabs, we'll be using them to associate our project with any external assets (datasets and notebooks) and any IBM cloud services.

  ![studio-project-dashboard](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/overview-empty.png)

### 2. Create the Notebook

* From the new project `Overview` panel, click `+ Add to project` on the top right and choose the `Notebook` asset type.

![studio-project-dashboard](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/add-assets-notebook.png)

* Fill in the following information:

  * Select the `From URL` tab. [1]
  * Enter a `Name` for the notebook and optionally a description. [2]
  * Under `Notebook URL` provide the following url: [https://github.com/IBM/visualize-food-insecurity/blob/master/notebooks/Diet-Related-Disease-Exploratory.ipynb](https://github.com/IBM/visualize-food-insecurity/blob/master/notebooks/Diet-Related-Disease-Exploratory.ipynb) [3]
  * For `Runtime` select the `Python 3.6` option. [4]

  ![add notebook](https://github.com/IBM/pattern-utils/raw/master/watson-studio/notebook-create-url-py35.png)

* Click the `Create` button.

* **TIP:** Once successfully imported, the notebook should appear in the `Notebooks` section of the `Assets` tab.

### 3. Upload data

* This project uses the dataset in [combined_data.csv.zip](combined_data.csv.zip). We need to load this asset to our project.

* Extract the zip file with your favorite unzip tool.

* From the new project `Overview` panel, click `+ Add to project` on the top right and choose the `Data` asset type.

   ![add asset](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/add-assets-data.png)

* A panel on the right of the screen will appear to assit you in uploading data. Follow the numbered steps in the image below.

  * Ensure you're on the `Load` tab. [1]
  * Click on the `browse` option. From your machine, browse to the location of the `combined_data.csv` file in this repository, and upload it. [not numbered]
  * Once uploaded, go to the `Files` tab. [2]
  * Ensure the `combined_data.csv` appears. [3]

   ![add data](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-studio/data-add-data-asset.png)

### 4. Run the notebook

* Click the `(►) Run` button to start stepping through the notebook.

* Stop at the second cell `Insert your data as a Pandas DataFrame`.

* Click on the `1001` data icon in the top right. The data files should show up.

* Click on each and select `Insert Pandas Data Frame`. Once you do that, a whole bunch of code will show up in the highlighted cell.

* Make sure your `combined_data.csv` is saved as `df_data_1`, so that it is consistent with my notebook and so you do not have to change the code.

![](doc/source/images/project-assets.png)

### 5. Save and Share

#### How to save your work:

Under the `File` menu, there are several ways to save your notebook:

* `Save` will simply save the current state of your notebook, without any version
  information.
* `Save Version` will save your current state of your notebook with a version tag
  that contains a date and time stamp. Up to 10 versions of your notebook can be
  saved, each one retrievable by selecting the `Revert To Version` menu item.

#### How to share your work:

You can share your notebook by selecting the `Share` button located in the top
right section of your notebook panel. The end result of this action will be a URL
link that will display a “read-only” version of your notebook. You have several
options to specify exactly what you want shared from your notebook:

* `Only text and output`: will remove all code cells from the notebook view.
* `All content excluding sensitive code cells`: will remove any code cells
  that contain a *sensitive* tag. For example, `# @hidden_cell` is used to protect
  your dashDB credentials from being shared.
* `All content, including code`: displays the notebook as is.
* A variety of `download as` options are also available in the menu.

## Analyze the data in Watson Analytics

1. [Download our new dataframe from Watson Studio](#1-download-our-new-dataframe-from-watson-studio)
2. [Upload our new dataframe csv into Watson Analytics](#2-upload-our-new-dataframe-csv-into-watson-analytics)
3. [Check out the discoveries that Watson Analytics offers](#3-check-out-the-discoveries-that-watson-analytics-offers)
4. [Suggest different relationships to visualize in the display section of Watson Analytics](#4-suggest-different-relationships-to-visualize-in-the-display-section-of-watson-analytics)

### 1. Download our new dataframe from Watson Studio

The last section of the notebook involves steps to download a dataframe from Watson Studio so that it can be used in Watson Analytics. For convenience, that data frame file (`df_focusdvalues.csv`) is also available in this repo and can be found [here](data/df_focusedvalues.csv). Download either version of the csv file for use in the following steps. The description of the data values can be found [here](data/Variable%20list.xlsx).

### 2. Upload our new dataframe csv into Watson Analytics

Once you create an account and login to [IBM Watson Analytics](https://www.ibm.com/watson-analytics) you can upload the csv you just downloaded and use it in your next steps. Do this in the "data" section and push "New data". This should only take a few moments to load.

> Note: If you are using a free trial version of Watson Analytics, the data download may fail due to storage limits. In this case, delete any pre-loaded sample data sets to make room for the `df_focusdvalues.csv` data file.

![](doc/source/images/Screen%20Shot%202017-10-30%20at%204.06.20%20PM.png)

### 3. Check out the discoveries that Watson Analytics offers

Once you've set up your account, you can see that the Watson platform has three sections: data, discover and display. You uploaded your data to the "data" section, but now you'll want to go to the "discover" section. Under "discover" you can select your dataframe dataset for use. Once you've selected it, the Watson platform will suggest different insights to visualize. You can move forward with its selections or create your own, or both. You can take a look at mine here (you'll need an account to view): https://ibm.co/2xAlAkq or see the screen shots attached to this repo. You can also go into the "display" section and create a shareable layout like mine (again you'll need an account): https://ibm.co/2A38Kg6.

![](doc/source/images/Screen%20Shot%202017-10-30%20at%204.05.53%20PM.png)

### 4. Suggest different relationships to visualize in the display section of Watson Analytics

You can see that with these visualizations the user can see the impact of food insecurity by state, geographically distributed and used aid such as reduced school lunches, a map of diabetes by state, a predictive model for food insecurity and diabetes (showcasing the factors that, in combination, suggest a likelihood of food insecurity), drivers of adult diabetes, drivers of food insecurity, the relationship with the frequency of farmers market locations, food insecurity and adult obesity, as well as the relationship between farmers markets, the percent of the population that is Asian, food insecurity and poverty rates.

## Analyzing output

By reviewing our visualizations both in Watson Studio and Watson, we learn that obesity and diabetes almost go hand in hand, along with food insecurity. We can also learn that this seems to be an inequality issue, both in income and race, with Black and Hispanic populations being more heavily impacted by food insecurity and diet-related diseases than those of the White and Asian populations. We can also see that school-aged children who qualify for reduced lunch are more likely obese than not whereas those that have a farm-to-school program are more unlikely to be obese.

Like many data science investigations, this analysis could have a big impact on policy and people's approach to food insecurity in the U.S. What's best is that we can create many projects much like this in a quick time period and share them with others by using Pandas, PixieDust as well as Watson's predictive and recommended visualizations.

![](doc/source/images/Screen%20Shot%202017-10-30%20at%204.29.41%20PM.png)

## Links

* [Create Watson Studio Notebooks](https://dataplatform.cloud.ibm.com/docs/content/analyze-data/creating-notebooks.html)
* [Bureau of Labor Statistics](https://www.bls.gov/cex/)
* [Food Environment Atlas](https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/)

## Learn more

* **Data Analytics Code Patterns**: Enjoyed this Code Pattern? Check out our other [Data Analytics Code Patterns](https://developer.ibm.com/technologies/data-science/)
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **Watson Studio**: Master the art of data science with IBM's [Watson Studio](https://www.ibm.com/cloud/watson-studio)

## License

This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
