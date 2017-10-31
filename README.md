## Visualizing-Food-Insecurity-with-Pixie-Dust-and-Watson-Analytics
_IBM Journey showing how to visualize US Food Insecurity with Pixie Dust and Watson Analytics._

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating our findings or communicating them ineffectively to non data science audiences. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that might not have had the same opportunity to work on a project of the same subject. By visualizing our findings and those insights that have the most power to do social good, we can bring awareness and maybe even change. This journey walks you through how to do just that, with IBM's Data Science Experience (DSX), Pandas, Pixie Dust and Watson Analytics.

For this particular journey, food insecurity throughout the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. For some context, this problem is a more and more relevant problem for the United States as obesity and diabetes rise and two out of three adult Americans are considered obese, one third of American minors are considered obsese, nearly ten percent of Americans have diabetes and nearly fifty percent of the African American population have heart disease. Even more, cardiovascular disease is the leading global cause of death, accounting for 17.3 million deaths per year, and rising. Native American populations more often than not do not have grocery stores on their reservation... and all of these trends are on the rise. The problem lies not only in low access to fresh produce, but food culture, low education on healthy eating as well as racial and income inequality.

The government data that I use in this journey has been conveniently combined into a dataset for our use, which you can find in this repo under combined_data.csv. You can find the original, government data from the US Bureau of Labor Statistics https://www.bls.gov/cex/ and The United States Department of Agriculture https://www.ers.usda.gov/data-products/food-environment-atlas/data-access-and-documentation-downloads/.

### What is DSX, Pixie Dust and Watson Analytics and why should I care enough about them to use them for my visualizations?

IBM's Data Science Experience, or DSX, is an online browser platform where you can use notebooks or R Studio for your data science projects. DSX is unique in that it automatically starts up a Spark instance for you, allowing you to work in the cloud without any extra work. DSX also has open data available to you, which you can connect to your notebook. There are also other projects available, in the form of notebooks, which you can follow along with and apply to your own use case. DSX also lets you save your work, share it and collaborate with others, much like I'm doing now!

Pixie Dust is a visualization library you can use on DSX. It is already installed into DSX and once it's imported, it only requires one line of code (two words) to use. With that same line of code, you can pick and choose different values to showcase and visualize in whichever way you want from matplotlib, seaborn and bokeh. If you have geographic data, you can also connect to google maps and Mapbox, depending on your preference. Check out a tutorial on Pixie Dust here: https://ibm-watson-data-lab.github.io/pixiedust/displayapi.html#introduction

IBM's Watson Analytics is another browser platform which allows you to input your data, conduct analysis on it and then visualize your findings. If you're new to data science, Watson recommends connections and visualizations with the data it has been given. These visualizations range from bar and scatter plots to predictive spirals, decision trees, heatmaps, trend lines and more. The Watson platform then allows you to share your findings and visualizations with others, completing your pipeline. Check out my visualizations with the link further down in the notebook, or in the images in this repo.

### Let's start with DSX.

Here's a tutorial on getting started with DSX: https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html.

To summarize the introduction, you must first make an account and log in. Then, you can create a project (I titled mine: "Diet-Related Disease"). From there, you'll be able to add data and start a notebook. To begin, I used the combined_data.csv as my data asset. You'll want to upload it as a data asset and once that is complete, go into your notebook in the edit mode (click on the pencil icon next to your notebook on the dashboard). To load your data in your notebook, you'll click on the "1001" data icon in the top right. The combined_data.csv should show up. Click on it and select "Insert Pandas Data Frame". Once you do that, a whole bunch of code will show up in your first cell. Once you see that, run the cell and follow along with my tutorial!

First import data and your libraries (do this by using the 1001 button above to the right). Libraries used in this notebook are: 

  ```from io import StringIO
  import requests
  import json
  import pandas as pd
  import seaborn as sns
  import numpy as np
  import matplotlib.pyplot as plt
  %matplotlib inline```
  
### Cleaning data and Exploring

This notebook starts out as a typical data science pipeline: exploring what our data looks like and cleaning the data. Though this is often considered the boring part of the job, it is extremely important. Without clean data, our insights and visualizations could be inaccurate or unclear. 

To initially explore, I used matplotlib to see a correlation matrix of our original data. I also looked at some basic statistics to get a feel for what kind of data we are looking at. I also went ahead and plotted using pandas and seaborn to make bar plots, scatterplots and regression plots.

  ```#check out our data!```
  ```df_data_1.columns```
  ```df_data_1.describe()```
  ```#to see columns distinctly and evaluate their state
  df_data_1['PCT_LACCESS_POP10'].unique()```
  ```df_data_1['PCT_REDUCED_LUNCH10'].unique()```
  ```df_data_1['PCT_DIABETES_ADULTS10'].unique()```
  ```df_data_1['FOODINSEC_10_12'].unique()```
  ```#looking at correlation in a table format
  df_data_1.corr()```
  ```#checking out a correlation matrix with matplotlib
  plt.matshow(df_data_1.corr())
  #we notice that there is a great deal of variables which makes it hard to read!```
  ```#other stats
  df_data_1.max()
  df_data_1.min()
  df_data_1.std()```
  ```# Plot counts of a specified column using Pandas
  df_data_1.FOODINSEC_10_12.value_counts().plot(kind='barh')```
  ```# Bar plot example
  sns.factorplot("PCT_SNAP09", "PCT_OBESE_ADULTS10", data=df_data_1,size=3,aspect=2)```
  ```# Regression plot
  sns.regplot("FOODINSEC_10_12", "PCT_OBESE_ADULTS10", data=df_data_1, robust=True, ci=95, color="seagreen")
  sns.despine();```

After looking at the data I realize that I'm only interested in seeing the connection between certain values and because the dataset is so large it's bringing in irrelevant information and creating noise. To change this, I created a smaller data frame, making sure to remove NaN and 0 values (0s in this dataset generally mean that a number was not recorded).

  ```#create a dataframe of values that are most interesting to food insecurity
df_focusedvalues = df_data_1[["State", "County","PCT_REDUCED_LUNCH10", "PCT_DIABETES_ADULTS10", "PCT_OBESE_ADULTS10", "FOODINSEC_10_12", "PCT_OBESE_CHILD11", "PCT_LACCESS_POP10", "PCT_LACCESS_CHILD10", "PCT_LACCESS_SENIORS10", "SNAP_PART_RATE10", "PCT_LOCLFARM07", "FMRKT13", "PCT_FMRKT_SNAP13", "PCT_FMRKT_WIC13", "FMRKT_FRVEG13", "PCT_FRMKT_FRVEG13", "PCT_FRMKT_ANMLPROD13", "FOODHUB12", "FARM_TO_SCHOOL", "SODATAX_STORES11", "State_y", "GROC12", "SNAPS12", "WICS12", "PCT_NHWHITE10", "PCT_NHBLACK10", "PCT_HISP10", "PCT_NHASIAN10", "PCT_65OLDER10", "PCT_18YOUNGER10", "POVRATE10", "CHILDPOVRATE10"]]```

  ```#remove NaNs and 0s
  df_focusedvalues = df_focusedvalues[(df_focusedvalues != 0).all(1)]
  df_focusedvalues = df_focusedvalues.dropna(how='any')```
  ```Before visualizing, a quick heatmap is created so that we can see what correlations we may want to visualize. I visualized a few of these relationships using seaborn, but I ultimately want to try out other visualizations. The quickest way to explore these is through Pixie Dust.```
    
  ```#look at heatmap of correlations with the dataframe to see what we should visualize
  corr = df_focusedvalues.corr()
  sns.heatmap(corr, 
              xticklabels=corr.columns.values,
              yticklabels=corr.columns.values)```  

We can immediately see that a fair amount of strong correlations and relationships exist. Some of these include 18 and younger and Hispanic, an inverse relationship between Asian and obese, a correlation between sodatax and Hispanic, African American and obesity as well as food insecurity, sodatax and obese minors, farmers markets and aid such as WIC and SNAP, obese minors and reduced lunches and a few more.

Let's try and plot some of these relationships with seaborn.

  ```#Percent of the population that is white vs SNAP aid participation (positive relationship)
  sns.regplot("PCT_NHWHITE10", "SNAP_PART_RATE10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
  ```#Percent of the population that is Hispanic vs SNAP aid participation (negative relationship)
  sns.regplot("SNAP_PART_RATE10", "PCT_HISP10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
   ```#Eligibility and use of reduced lunches in schools vs percent of the population that is Hispanic (positive relationship)
  sns.regplot("PCT_REDUCED_LUNCH10", "PCT_HISP10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
  ```#Percent of the population that is black vs percent of the population with diabetes (positive relationship)
  sns.regplot("PCT_NHBLACK10", "PCT_DIABETES_ADULTS10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
  ```#Percent of the population that is black vs percent of the population with diabetes (positive relationship)
  sns.regplot("PCT_NHBLACK10", "PCT_DIABETES_ADULTS10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
  ```#Percent of population with diabetes vs percent of population with obesity (positive relationship)
  sns.regplot("PCT_DIABETES_ADULTS10", "PCT_OBESE_ADULTS10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
  sns.despine();```
  
### Now, let's visualize with Pixie Dust.

Now that we've gained some initial insights, let's try out a different tool: Pixie Dust!

As you can see in the notebook below, to activate Pixie Dust, we just import it and then write:

 ```display(your_dataframe_name)```
 
After doing this your dataframe will show up in a column-row table format. To visualize your data, you can click the chart icon at the top left (looks like an arrow going up). From there you can choose from a variety of visuals. Once you select the type of chart you want, you can then select the variables you want to showcase. It's worth playing around with this to see how you can create the most effective visualizations for your audience. The notebook below showcases a couple options such as scatterplots, bar charts, line charts, and histograms.

  ```import pixiedust```
  
  ```#looking at the dataframe table. Pixie Dust does this automatically, but to find it again you can click the table icon.
  display(df_focusedvalues)```
  
  ```#using seaborn in Pixie Dust to look at Food Insecurity and the Percent of the population that is black in a scatter plot
  display(df_focusedvalues)```
  
  ```#using matplotlib in Pixie Dust to view Food Insecurity by state in a bar chart
  display(df_focusedvalues)```
  
  ```#using bokeh in Pixie Dust to view the percent of the population that is black vs the percent of the population that is obese in a line chart
  display(df_focusedvalues)```
  
  ```#using seaborn in Pixie Dust to view obesity vs diabetes in a scatterplot
  display(df_focusedvalues)```
  
  ```#using matplotlib in Pixie Dust to view the percent of the population that is white vs SNAP participation rates in a histogram
  display(df_focusedvalues)```
  
  ```#using bokeh in Pixie Dust to view the trends in obesity, diabetes, food insecurity and the percent of the population that is black in a line graph
  display(df_focusedvalues)```
  
  ```#using matplotlib in Pixie Dust to view childhood obesity vs reduced school lunches in a scatterplot
  display(df_focusedvalues)```
  
### Let's download our dataframe and work with it on Watson Analytics.

Unfortunately, in DSX we cannot download our dataframe as a csv in one line of code, but we can download it to DSX so that it can be downloaded and used elsewhere as well as for other projects. I demonstrate how to do this in the notebook.

Once you follow along, you can take the new .csv (found under "Data Services" --> "Object Storage" from the top button) and upload it to Watson Analytics. Again, if you do not have an account, you'll want to set one up. Once you are logged in and ready to go, you can upload the data (saved in this repo as df_focusedvalues.csv) to your Watson platform. 

  ```#download dataframe as csv file to use in IBM Watson Analytics```
  
  ```#Below is a tutorial on how to do this, but I found it a bit confusing and uncompatable with python 3. I suggest doing what I did below!
  #https://medium.com/ibm-data-science-experience/working-with-object-storage-in-data-science-experience-python-edition-c96bc6c6101```
  
  ```#First get your credentials by going to the "1001" button again and under your csv file selecting "Insert Credentials". 
  #The cell below will be hidden because it has my personal credentials so go ahead and insert your own.```
  
  ```df_focusedvalues.to_csv('df_focusedvalues.csv',index=False)```
  
  ```from io import StringIO  
import requests  
import json  
import pandas as pd


def put_file(credentials, local_file_name): 
    """This functions returns a StringIO object containing the file content from Bluemix Object Storage V3.""" 

    f = open(local_file_name,'r') 
    my_data = f.read() 
    url1 = ''.join(['https://identity.open.softlayer.com', '/v3/auth/tokens']) 
    data = {'auth': {'identity': {'methods': ['password'], 'password': {'user': {'name': credentials['username'],'domain': {'id': credentials['domain_id']}, 'password': credentials['password']}}}}} 

    headers1 = {'Content-Type': 'application/json'} 
    resp1 = requests.post(url=url1, data=json.dumps(data),    headers=headers1)
    resp1_body = resp1.json()
    for e1 in resp1_body['token']['catalog']: 
        if(e1['type']=='object-store'): 

            for e2 in e1['endpoints']: 
                if(e2['interface']=='public'and e2['region']== credentials['region']):    
                    url2 = ''.join([e2['url'],'/', credentials['container'], '/', local_file_name]) 

                    s_subject_token = resp1.headers['x-subject-token'] 
                    headers2 = {'X-Auth-Token': s_subject_token, 'accept': 'application/json'} 
                    resp2 = requests.put(url=url2, headers=headers2, data = my_data ) 
                    print(resp2)```
                    
  ```put_file(credentials_98,'df_focusedvalues.csv')```
  
  ``` Once this is complete, go get your csv file from Data Services, Object Storage! (Find this above! ^)```
  
### Using Watson to visualize our insights.

Once you've set up your account, you can see taht the Watson plaform has three sections: data, discover and display. You uploaded your data to the "data" section, but now you'll want to go to the "discover" section. Under "discover" you can select your dataframe dataset for use. Once you've selected it, the Watson platform will suggest different insights to visualize. You can move forward with its selections or your own, or both. You can take a look at mine here (you'll need an account to view): https://ibm.co/2xAlAkq or see the screen shots attached to this repo. You can also go into the "display" section and create a shareable layout like mine (again you'll need an account): https://ibm.co/2A38Kg6.

You can see that with these visualizations the user can see the impact of food insecurity by state, geographically distributed and used aid such as reduced school lunches, a map of diabetes by state, a predictive model for food insecurity and diabetes (showcasing the factors that, in combination, suggest a likelihood of food insecurity), drivers of adult diabetes, drivers of food insecurity, the relationship with the frequency of farmers market locations, food insecurity and adult obesity, as well as the relationship between farmers markets, the percent of the population that is Asian, food insecurity and poverty rates.

By reviewing our visualizations both in DSX and Watson, we learn that obesity and diabetes almost go hand in hand, along with food insecurity. We can also learn that this seems to be an inequality issue, both in income and race, with Black and Hispanic populations being more heavily impacted by food insecurity and diet-related diseases than those of the White and Asian populations. We can also see that school-aged children who qualify for reduced lunch are more likely obese than not whereas those that have a farm-to-school program are more unlikely to be obese.

Like many data science investigations, this analysis could have a big impact on policy and people's approach to food insecurity in the U.S. What's best is that we can create many projects much like this in a quick time period and share them with others by using Pandas, Pixie Dust as well as Watson's predictive and recommended visualizations.
  
  
