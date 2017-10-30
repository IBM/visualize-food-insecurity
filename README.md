## Visualizing-Food-Insecurity-with-Pixie-Dust-and-Watson-Analytics
IBM Journey showing how to visualize US Food Insecurity with Pixie Dust and Watson Analytics

Often in data science we do a great deal of work to glean insights that have an impact on society or a subset of it and yet, often, we end up not communicating it or communicating it ineffectively. That's where visualizations become the most powerful. By visualizing our insights and predictions, we, as data scientists and data lovers, can make a real impact and educate those around us that didn't have the opportunity to work on the same project. This journey walks you through how to do just that, with IBM's Data Science Experience (DSX), Pixie Dust and Watson Analytics.

For this particular journey, food insecurity in the US is focused on. Low access, diet-related diseases, race, poverty, geography and other factors are considered by using open government data. This has been conveniently combined into a dataset for our use, which you can find in this repo under combined_data.csv.  

### What is DSX, Pixie Dust and Watson Analytics and why should I care enough about them to use them for my visualizations?

IBM's Data Science Experience, or DSX, is an online browser platform where you can use notebooks or R Studio for your data science projects. DSX is unique in that it automatically starts up a Spark instance for you, allowing you to work in the cloud. DSX also has open data available to you, which you can connect to your notebook. There are also other projects available, in the form of notebooks, which you can follow along with and apply to your own use case. DSX also lets you save your work, share it and collaborate with others.

Pixie Dust is a visualization library you can use on DSX. It is already installed into DSX and it only requires one line of code (two words) to use. With that same line you can pick and choose different values to showcase and visualize in whichever way you want from matplotlib, seaborn and bokeh. If you have geographic data, you can also connect to google maps and Mapbox, depending on your preference. Check out a tutorial on Pixie Dust here: https://ibm-watson-data-lab.github.io/pixiedust/displayapi.html#introduction

IBM's Watson Analytics is another browser platform which allows you to input your data, conduct analysis on it and then visualize your findings. If you're new to data science, Watson recommends connections and visualizations with the data it's been given. These visualizations range from bar and scatter plots to predictive spirals, decision trees, heatmaps, trend lines and more. The Watson platform then allows you to share your findings and visualizations with others, completing your pipeline.


### Let's start with DSX.

Here's a tutorial on getting started with DSX: https://datascience.ibm.com/docs/content/analyze-data/creating-notebooks.html.

To summarize, you must first make an account and log in. Then, create a project (I titled mine: "Diet-Related Disease"). From there you'll be able to add data and start a notebook. To begin, I used the combined_data.csv as my data asset. You'll want to upload it as a data asset and once that is complete, go into your notebook in the edit mode (click on the pencil icon next to your notebook on the dashboard). To load your data in your notebook, you'll click on the "1001" data icon in the top right. The combined_data.csv should show up. Click on it an select "Insert Pandas Data Frame". Once you do that, a whole bunch of code will show up in your first cell. Once you see that, run the cell and follow along with my tutorial!

### Cleaning data and Exploring

The notebook starts out as a typical data science pipeline: exploring what our data looks like and cleaning the data. Though this is often considered the boring part of the job, it is extremely important. Without clean data, our insights and visualizations could be inaccurate or unclear. 

To initially explore I used matplotlib to see a correlation matrix of our original data. I also looked at some basic statistics to get a feel for what kind of data we are looking at. I also went ahead and plotted using pandas and seaborn to make bar plots, scatterplots and a regression plot.

After looking at the data I realize that I'm only interested in seeing the connection between certain values and because the dataset is so large it's bringing in irrelevant information and creating noise. To change this, I created a smaller data frame, making sure to remove NaN and 0 values (0s in this dataset generally mean that a number was not recorded).

Before visualizing, a quick heatmap is created so that we can see what correlations we may want to visualize. I visualized a few of these relationships using seaborn, but I ultimately want to try out other visualizations. The quickest way to explore these is through Pixie Dust.

### Now, let's visualize with Pixie Dust.

As you can see in the notebook, to activate Pixie Dust, we just import it and then write:

 ```display(your_dataframe_name)```
 
 After doing this your dataframe will show up in a column-row table format. To visualize your data, you can click the chart icon at the top left (looks like an arrow going up). From there you can choose from a variety of visuals. Once you select the type of chart you want, you can then select the variables you want to showcase. It's worth playing around with this to see how you can create the most effective visualization for your audience. The notebook showcases a couple options such as scatterplots, bar charts, line charts, and histograms.

### Let's download our dataframe and work with it on Watson Analytics.

Unfortunately, in DSX we cannot download our dataframe as a csv in one line of code, but we can download it to DSX so that it can be downloaded and used elsewhere as well as for other projects. I demonstrate how to do this in the notebook.

Once you follow along, you can take the new .csv (found under "Data Services" --> "Object Storage" from the top button) and upload it to Watson Analytics. Again, if you do not have an account, you'll want to set one up. Once you are logged in and ready to go, you can upload the data (saved in this repo as df_focusedvalues.csv) to your Watson platform. 

### Using Watson to visualize our insights.

The Watson plaform has three sections: data, discover and display. You uploaded your data to the "data" section, but now you'll want to go to the "discover" section.
