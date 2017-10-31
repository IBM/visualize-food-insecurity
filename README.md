
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


```python
#import data and libraries (do this by using the 1001 button above to the right)

from io import StringIO
import requests
import json
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

# @hidden_cell
# This function accesses a file in your Object Storage. The definition contains your credentials.

df_data_1 = pd.read_csv(get_object_storage_file_with_credentials_97ff18b62fe74e0496a8cb3fe9eaa15b('DietRelatedDisease', 'combined_data.csv'))
df_data_1.head()

```






<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>FIPS</th>
      <th>State</th>
      <th>County</th>
      <th>LACCESS_POP10</th>
      <th>PCT_LACCESS_POP10</th>
      <th>LACCESS_LOWI10</th>
      <th>PCT_LACCESS_LOWI10</th>
      <th>LACCESS_CHILD10</th>
      <th>PCT_LACCESS_CHILD10</th>
      <th>...</th>
      <th>CES_WATRPSCQ_std</th>
      <th>CES_WATRPSPQ_std</th>
      <th>CES_WELFAREB_std</th>
      <th>CES_WELFAREI_std</th>
      <th>CES_WINDOWAC_std</th>
      <th>CES_WOMGRLCQ_std</th>
      <th>CES_WOMGRLPQ_std</th>
      <th>CES_WOMSIXCQ_std</th>
      <th>CES_WOMSIXPQ_std</th>
      <th>CES_YRBUILT_std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1001</td>
      <td>AL</td>
      <td>Autauga</td>
      <td>18428.439685</td>
      <td>33.769657</td>
      <td>5344.427472</td>
      <td>9.793530</td>
      <td>4822.500269</td>
      <td>8.837112</td>
      <td>...</td>
      <td>81.423484</td>
      <td>104.052107</td>
      <td>NaN</td>
      <td>6.511603</td>
      <td>0.0</td>
      <td>129.670249</td>
      <td>198.594827</td>
      <td>121.172656</td>
      <td>180.653472</td>
      <td>23.432432</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1003</td>
      <td>AL</td>
      <td>Baldwin</td>
      <td>35210.814078</td>
      <td>19.318473</td>
      <td>9952.144027</td>
      <td>5.460261</td>
      <td>7916.131932</td>
      <td>4.343199</td>
      <td>...</td>
      <td>81.423484</td>
      <td>104.052107</td>
      <td>NaN</td>
      <td>6.511603</td>
      <td>0.0</td>
      <td>129.670249</td>
      <td>198.594827</td>
      <td>121.172656</td>
      <td>180.653472</td>
      <td>23.432432</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1005</td>
      <td>AL</td>
      <td>Barbour</td>
      <td>5722.305602</td>
      <td>20.840972</td>
      <td>3135.676086</td>
      <td>11.420316</td>
      <td>940.419327</td>
      <td>3.425062</td>
      <td>...</td>
      <td>81.423484</td>
      <td>104.052107</td>
      <td>NaN</td>
      <td>6.511603</td>
      <td>0.0</td>
      <td>129.670249</td>
      <td>198.594827</td>
      <td>121.172656</td>
      <td>180.653472</td>
      <td>23.432432</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1007</td>
      <td>AL</td>
      <td>Bibb</td>
      <td>1044.867327</td>
      <td>4.559753</td>
      <td>491.449066</td>
      <td>2.144661</td>
      <td>249.204753</td>
      <td>1.087518</td>
      <td>...</td>
      <td>81.423484</td>
      <td>104.052107</td>
      <td>NaN</td>
      <td>6.511603</td>
      <td>0.0</td>
      <td>129.670249</td>
      <td>198.594827</td>
      <td>121.172656</td>
      <td>180.653472</td>
      <td>23.432432</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1009</td>
      <td>AL</td>
      <td>Blount</td>
      <td>1548.175559</td>
      <td>2.700840</td>
      <td>609.027708</td>
      <td>1.062468</td>
      <td>384.911607</td>
      <td>0.671490</td>
      <td>...</td>
      <td>81.423484</td>
      <td>104.052107</td>
      <td>NaN</td>
      <td>6.511603</td>
      <td>0.0</td>
      <td>129.670249</td>
      <td>198.594827</td>
      <td>121.172656</td>
      <td>180.653472</td>
      <td>23.432432</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1244 columns</p>
</div>



### Cleaning data and Exploring

This notebook starts out as a typical data science pipeline: exploring what our data looks like and cleaning the data. Though this is often considered the boring part of the job, it is extremely important. Without clean data, our insights and visualizations could be inaccurate or unclear. 

To initially explore, I used matplotlib to see a correlation matrix of our original data. I also looked at some basic statistics to get a feel for what kind of data we are looking at. I also went ahead and plotted using pandas and seaborn to make bar plots, scatterplots and regression plots.


```python
#check out our data!
```


```python
df_data_1.columns
```




    Index(['Unnamed: 0', 'FIPS', 'State', 'County', 'LACCESS_POP10',
           'PCT_LACCESS_POP10', 'LACCESS_LOWI10', 'PCT_LACCESS_LOWI10',
           'LACCESS_CHILD10', 'PCT_LACCESS_CHILD10',
           ...
           'CES_WATRPSCQ_std', 'CES_WATRPSPQ_std', 'CES_WELFAREB_std',
           'CES_WELFAREI_std', 'CES_WINDOWAC_std', 'CES_WOMGRLCQ_std',
           'CES_WOMGRLPQ_std', 'CES_WOMSIXCQ_std', 'CES_WOMSIXPQ_std',
           'CES_YRBUILT_std'],
          dtype='object', length=1244)




```python
df_data_1.describe()
```

    /usr/local/src/conda3_runtime.v19/4.1.1/lib/python3.5/site-packages/numpy/lib/function_base.py:3834: RuntimeWarning: Invalid value encountered in percentile
      RuntimeWarning)





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>FIPS</th>
      <th>LACCESS_POP10</th>
      <th>PCT_LACCESS_POP10</th>
      <th>LACCESS_LOWI10</th>
      <th>PCT_LACCESS_LOWI10</th>
      <th>LACCESS_CHILD10</th>
      <th>PCT_LACCESS_CHILD10</th>
      <th>LACCESS_SENIORS10</th>
      <th>PCT_LACCESS_SENIORS10</th>
      <th>...</th>
      <th>CES_WATRPSCQ_std</th>
      <th>CES_WATRPSPQ_std</th>
      <th>CES_WELFAREB_std</th>
      <th>CES_WELFAREI_std</th>
      <th>CES_WINDOWAC_std</th>
      <th>CES_WOMGRLCQ_std</th>
      <th>CES_WOMGRLPQ_std</th>
      <th>CES_WOMSIXCQ_std</th>
      <th>CES_WOMSIXPQ_std</th>
      <th>CES_YRBUILT_std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3262.000000</td>
      <td>3262.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>3150.000000</td>
      <td>...</td>
      <td>2763.000000</td>
      <td>2763.000000</td>
      <td>1947.000000</td>
      <td>2763.000000</td>
      <td>2763.0</td>
      <td>2763.000000</td>
      <td>2763.000000</td>
      <td>2763.000000</td>
      <td>2763.000000</td>
      <td>2681.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1630.500000</td>
      <td>31457.332925</td>
      <td>20130.485391</td>
      <td>23.540300</td>
      <td>5541.030150</td>
      <td>8.359188</td>
      <td>4953.676750</td>
      <td>5.502879</td>
      <td>2677.821025</td>
      <td>3.909622</td>
      <td>...</td>
      <td>69.546993</td>
      <td>101.138452</td>
      <td>1.353149</td>
      <td>10.882370</td>
      <td>0.0</td>
      <td>121.350532</td>
      <td>212.334189</td>
      <td>106.390781</td>
      <td>186.085924</td>
      <td>26.350572</td>
    </tr>
    <tr>
      <th>std</th>
      <td>941.802616</td>
      <td>16375.524971</td>
      <td>51254.806435</td>
      <td>20.231676</td>
      <td>13849.378974</td>
      <td>8.212651</td>
      <td>13155.181390</td>
      <td>4.875273</td>
      <td>6519.993517</td>
      <td>4.212330</td>
      <td>...</td>
      <td>10.964508</td>
      <td>17.466676</td>
      <td>0.890585</td>
      <td>4.850197</td>
      <td>0.0</td>
      <td>26.669356</td>
      <td>50.179029</td>
      <td>28.662127</td>
      <td>48.519222</td>
      <td>6.232962</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1001.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>45.659691</td>
      <td>54.544706</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
      <td>48.651029</td>
      <td>112.399808</td>
      <td>45.121029</td>
      <td>80.583433</td>
      <td>14.443555</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>815.250000</td>
      <td>19025.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1630.500000</td>
      <td>30038.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2445.750000</td>
      <td>47006.500000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3261.000000</td>
      <td>72153.000000</td>
      <td>886068.668386</td>
      <td>100.000001</td>
      <td>292541.789025</td>
      <td>72.274456</td>
      <td>260308.794094</td>
      <td>34.015595</td>
      <td>78922.918719</td>
      <td>29.208633</td>
      <td>...</td>
      <td>106.552781</td>
      <td>153.465998</td>
      <td>2.638870</td>
      <td>22.275774</td>
      <td>0.0</td>
      <td>200.919016</td>
      <td>376.288907</td>
      <td>189.674002</td>
      <td>363.251444</td>
      <td>54.306640</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 1235 columns</p>
</div>




```python
#to see columns distinctly and evaluate their state
df_data_1['PCT_LACCESS_POP10'].unique()
```




    array([ 33.7696573 ,  19.3184726 ,  20.84097171, ...,  20.22041443,
            10.91540662,  17.20994869])




```python
df_data_1['PCT_REDUCED_LUNCH10'].unique()
```




    array([  6.88610662,   5.54233978,   4.58214007, ...,  12.14804723,
            16.24668435,   7.57430489])




```python
df_data_1['PCT_DIABETES_ADULTS10'].unique()
```




    array([ 11.8,  14.2,  11.1,  14. ,  17.5,  16. ,  13.9,  15. ,  13.6,
            12.5,  17.3,  15.7,  14.7,  13.3,  12.6,  13.7,  17.7,  15.6,
            14.8,  13. ,  15.1,  12.9,  12.3,  13.2,  14.1,  15.8,  19.4,
            16.3,  14.3,  13.4,  11.7,  13.8,   9.8,  10.7,  19.3,  16.6,
            17.4,  12.1,  11.9,  18.2,  14.6,  13.5,   8.9,  10.5,  18.1,
             nan,   7.7,   6.7,   7. ,   8.3,   6.8,   8.2,   6.1,   6.3,
             7.2,   8.4,   7.8,   7.6,   7.5,   8.6,  14.9,   9. ,   7.3,
            11.2,  10.8,   9.3,   8. ,  12. ,  11.6,   7.9,  10.4,   6.4,
             9.7,  14.4,   8.7,  11.4,  15.2,  11.5,  12.7,  10.2,  12.2,
            11. ,  12.4,  10.9,  14.5,  13.1,  15.9,  12.8,   8.1,   9.6,
             9.1,   8.8,   7.4,  10. ,   5.5,   9.4,   6.6,   6.9,   9.5,
             9.2,   8.5,   6. ,   7.1,   6.5,   4.7,   5.9,   4.1,   3.6,
             5.1,   5. ,   4.3,   5.6,   4.9,   5.2,   4.8,   3.3,   3.9,
            10.3,  10.1,   9.9,  11.3,  15.3,  10.6,  16.2,  16.7,  16.4,
             5.8,   5.4,  17.2,  15.4,  16.1,  16.8,   6.2,   5.7,  16.5,
            17. ,  18.3,   4.5,  15.5,  17.8,  17.1,  17.9,   4. ])




```python
df_data_1['FOODINSEC_10_12'].unique()
```




    array([ 17.9,   nan,  12.1,  14.9,  19.7,  15.6,  14.1,  13.4,  11.6,
            12. ,  14.8,  16.9,  14. ,  14.3,  13. ,  13.5,  12.6,  14.4,
            15.7,  11.4,  10.6,  20.9,  16.7,  16.6,   9.9,  15.2,  13.2,
            17. ,   8.7,  16.1,  15.3,  13.6,  12.3,  15.4,  12.9,  16.2,
            18.4,  12.7,   9.2,  14.6,  14.2,  11.2,  13.8])




```python
#looking at correlation in a table format
df_data_1.corr()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>FIPS</th>
      <th>LACCESS_POP10</th>
      <th>PCT_LACCESS_POP10</th>
      <th>LACCESS_LOWI10</th>
      <th>PCT_LACCESS_LOWI10</th>
      <th>LACCESS_CHILD10</th>
      <th>PCT_LACCESS_CHILD10</th>
      <th>LACCESS_SENIORS10</th>
      <th>PCT_LACCESS_SENIORS10</th>
      <th>...</th>
      <th>CES_WATRPSCQ_std</th>
      <th>CES_WATRPSPQ_std</th>
      <th>CES_WELFAREB_std</th>
      <th>CES_WELFAREI_std</th>
      <th>CES_WINDOWAC_std</th>
      <th>CES_WOMGRLCQ_std</th>
      <th>CES_WOMGRLPQ_std</th>
      <th>CES_WOMSIXCQ_std</th>
      <th>CES_WOMSIXPQ_std</th>
      <th>CES_YRBUILT_std</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Unnamed: 0</th>
      <td>1.000000</td>
      <td>0.988077</td>
      <td>-0.044663</td>
      <td>0.046794</td>
      <td>-0.038625</td>
      <td>0.022513</td>
      <td>-0.034837</td>
      <td>0.042818</td>
      <td>-0.066165</td>
      <td>0.067212</td>
      <td>...</td>
      <td>-0.090276</td>
      <td>-0.029702</td>
      <td>0.494726</td>
      <td>-0.130957</td>
      <td>NaN</td>
      <td>-0.041758</td>
      <td>0.060190</td>
      <td>0.040847</td>
      <td>0.054743</td>
      <td>0.124638</td>
    </tr>
    <tr>
      <th>FIPS</th>
      <td>0.988077</td>
      <td>1.000000</td>
      <td>-0.041895</td>
      <td>0.043318</td>
      <td>-0.035782</td>
      <td>0.017944</td>
      <td>-0.032581</td>
      <td>0.039247</td>
      <td>-0.062296</td>
      <td>0.065611</td>
      <td>...</td>
      <td>-0.108750</td>
      <td>-0.032423</td>
      <td>0.493055</td>
      <td>-0.132797</td>
      <td>NaN</td>
      <td>-0.057590</td>
      <td>0.050669</td>
      <td>0.016636</td>
      <td>0.042205</td>
      <td>0.129827</td>
    </tr>
    <tr>
      <th>LACCESS_POP10</th>
      <td>-0.044663</td>
      <td>-0.041895</td>
      <td>1.000000</td>
      <td>0.053159</td>
      <td>0.920129</td>
      <td>-0.051210</td>
      <td>0.992586</td>
      <td>0.078428</td>
      <td>0.951078</td>
      <td>-0.036140</td>
      <td>...</td>
      <td>0.211475</td>
      <td>0.189190</td>
      <td>0.110039</td>
      <td>0.112132</td>
      <td>NaN</td>
      <td>0.141888</td>
      <td>0.163084</td>
      <td>0.141098</td>
      <td>0.158161</td>
      <td>0.030863</td>
    </tr>
    <tr>
      <th>PCT_LACCESS_POP10</th>
      <td>0.046794</td>
      <td>0.043318</td>
      <td>0.053159</td>
      <td>1.000000</td>
      <td>0.058148</td>
      <td>0.901876</td>
      <td>0.051701</td>
      <td>0.960261</td>
      <td>0.059648</td>
      <td>0.919669</td>
      <td>...</td>
      <td>0.078081</td>
      <td>0.073386</td>
      <td>-0.028346</td>
      <td>0.029399</td>
      <td>NaN</td>
      <td>0.008136</td>
      <td>0.135848</td>
      <td>0.034067</td>
      <td>0.145976</td>
      <td>-0.130053</td>
    </tr>
    <tr>
      <th>LACCESS_LOWI10</th>
      <td>-0.038625</td>
      <td>-0.035782</td>
      <td>0.920129</td>
      <td>0.058148</td>
      <td>1.000000</td>
      <td>0.014073</td>
      <td>0.934088</td>
      <td>0.087289</td>
      <td>0.839824</td>
      <td>-0.033301</td>
      <td>...</td>
      <td>0.152635</td>
      <td>0.158271</td>
      <td>0.097096</td>
      <td>0.078706</td>
      <td>NaN</td>
      <td>0.080647</td>
      <td>0.110861</td>
      <td>0.078411</td>
      <td>0.109655</td>
      <td>-0.043724</td>
    </tr>
    <tr>
      <th>PCT_LACCESS_LOWI10</th>
      <td>0.022513</td>
      <td>0.017944</td>
      <td>-0.051210</td>
      <td>0.901876</td>
      <td>0.014073</td>
      <td>1.000000</td>
      <td>-0.046528</td>
      <td>0.890063</td>
      <td>-0.049752</td>
      <td>0.826947</td>
      <td>...</td>
      <td>-0.015167</td>
      <td>0.013740</td>
      <td>-0.044804</td>
      <td>-0.032070</td>
      <td>NaN</td>
      <td>-0.043802</td>
      <td>0.052483</td>
      <td>-0.036339</td>
      <td>0.054818</td>
      <td>-0.179561</td>
    </tr>
    <tr>
      <th>LACCESS_CHILD10</th>
      <td>-0.034837</td>
      <td>-0.032581</td>
      <td>0.992586</td>
      <td>0.051701</td>
      <td>0.934088</td>
      <td>-0.046528</td>
      <td>1.000000</td>
      <td>0.086029</td>
      <td>0.914988</td>
      <td>-0.043977</td>
      <td>...</td>
      <td>0.194859</td>
      <td>0.173740</td>
      <td>0.111565</td>
      <td>0.104240</td>
      <td>NaN</td>
      <td>0.136906</td>
      <td>0.162738</td>
      <td>0.135360</td>
      <td>0.156621</td>
      <td>0.017344</td>
    </tr>
    <tr>
      <th>PCT_LACCESS_CHILD10</th>
      <td>0.042818</td>
      <td>0.039247</td>
      <td>0.078428</td>
      <td>0.960261</td>
      <td>0.087289</td>
      <td>0.890063</td>
      <td>0.086029</td>
      <td>1.000000</td>
      <td>0.069079</td>
      <td>0.823482</td>
      <td>...</td>
      <td>0.053151</td>
      <td>0.050514</td>
      <td>-0.042987</td>
      <td>0.020719</td>
      <td>NaN</td>
      <td>0.006240</td>
      <td>0.133672</td>
      <td>0.025514</td>
      <td>0.138429</td>
      <td>-0.152206</td>
    </tr>
    <tr>
      <th>LACCESS_SENIORS10</th>
      <td>-0.066165</td>
      <td>-0.062296</td>
      <td>0.951078</td>
      <td>0.059648</td>
      <td>0.839824</td>
      <td>-0.049752</td>
      <td>0.914988</td>
      <td>0.069079</td>
      <td>1.000000</td>
      <td>0.004796</td>
      <td>...</td>
      <td>0.243962</td>
      <td>0.222335</td>
      <td>0.093470</td>
      <td>0.127424</td>
      <td>NaN</td>
      <td>0.137348</td>
      <td>0.147420</td>
      <td>0.138918</td>
      <td>0.145248</td>
      <td>0.049154</td>
    </tr>
    <tr>
      <th>PCT_LACCESS_SENIORS10</th>
      <td>0.067212</td>
      <td>0.065611</td>
      <td>-0.036140</td>
      <td>0.919669</td>
      <td>-0.033301</td>
      <td>0.826947</td>
      <td>-0.043977</td>
      <td>0.823482</td>
      <td>0.004796</td>
      <td>1.000000</td>
      <td>...</td>
      <td>0.087898</td>
      <td>0.079362</td>
      <td>-0.033220</td>
      <td>0.037170</td>
      <td>NaN</td>
      <td>-0.042446</td>
      <td>0.091596</td>
      <td>-0.005059</td>
      <td>0.112250</td>
      <td>-0.088942</td>
    </tr>
    <tr>
      <th>LACCESS_HHNV10</th>
      <td>-0.026096</td>
      <td>-0.022842</td>
      <td>0.881175</td>
      <td>-0.015056</td>
      <td>0.877247</td>
      <td>-0.071680</td>
      <td>0.862960</td>
      <td>0.007832</td>
      <td>0.865782</td>
      <td>-0.094863</td>
      <td>...</td>
      <td>0.161296</td>
      <td>0.141365</td>
      <td>0.110913</td>
      <td>0.105661</td>
      <td>NaN</td>
      <td>0.107646</td>
      <td>0.088286</td>
      <td>0.101520</td>
      <td>0.077641</td>
      <td>0.084197</td>
    </tr>
    <tr>
      <th>PCT_LACCESS_HHNV10</th>
      <td>-0.075215</td>
      <td>-0.085881</td>
      <td>-0.143286</td>
      <td>0.121065</td>
      <td>-0.103820</td>
      <td>0.279894</td>
      <td>-0.139366</td>
      <td>0.163006</td>
      <td>-0.140336</td>
      <td>0.044663</td>
      <td>...</td>
      <td>-0.133084</td>
      <td>-0.110760</td>
      <td>-0.063137</td>
      <td>-0.126576</td>
      <td>NaN</td>
      <td>0.095210</td>
      <td>-0.070004</td>
      <td>0.061236</td>
      <td>-0.083239</td>
      <td>-0.064216</td>
    </tr>
    <tr>
      <th>REDEMP_SNAPS08</th>
      <td>-0.020698</td>
      <td>-0.019780</td>
      <td>0.178027</td>
      <td>-0.123591</td>
      <td>0.232706</td>
      <td>-0.060781</td>
      <td>0.182011</td>
      <td>-0.063277</td>
      <td>0.158180</td>
      <td>-0.242959</td>
      <td>...</td>
      <td>-0.089795</td>
      <td>-0.062693</td>
      <td>0.005128</td>
      <td>-0.027524</td>
      <td>NaN</td>
      <td>-0.111341</td>
      <td>-0.073952</td>
      <td>-0.129054</td>
      <td>-0.097433</td>
      <td>-0.117248</td>
    </tr>
    <tr>
      <th>REDEMP_SNAPS12</th>
      <td>-0.064050</td>
      <td>-0.065383</td>
      <td>0.252891</td>
      <td>-0.094995</td>
      <td>0.294116</td>
      <td>-0.049594</td>
      <td>0.253167</td>
      <td>-0.032711</td>
      <td>0.241764</td>
      <td>-0.224668</td>
      <td>...</td>
      <td>0.023405</td>
      <td>0.055361</td>
      <td>-0.037699</td>
      <td>0.022950</td>
      <td>NaN</td>
      <td>-0.045447</td>
      <td>-0.057415</td>
      <td>-0.068540</td>
      <td>-0.065334</td>
      <td>-0.073315</td>
    </tr>
    <tr>
      <th>PCH_REDEMP_SNAPS_08_12</th>
      <td>-0.050894</td>
      <td>-0.054862</td>
      <td>0.056659</td>
      <td>0.121958</td>
      <td>0.026905</td>
      <td>0.079466</td>
      <td>0.051467</td>
      <td>0.114052</td>
      <td>0.072679</td>
      <td>0.129978</td>
      <td>...</td>
      <td>0.182524</td>
      <td>0.183645</td>
      <td>-0.082666</td>
      <td>0.070648</td>
      <td>NaN</td>
      <td>0.137609</td>
      <td>0.041138</td>
      <td>0.136638</td>
      <td>0.073879</td>
      <td>0.048338</td>
    </tr>
    <tr>
      <th>PCT_SNAP09</th>
      <td>0.011844</td>
      <td>0.017125</td>
      <td>-0.064858</td>
      <td>-0.244282</td>
      <td>-0.004672</td>
      <td>-0.111960</td>
      <td>-0.060723</td>
      <td>-0.237965</td>
      <td>-0.075745</td>
      <td>-0.238120</td>
      <td>...</td>
      <td>-0.403927</td>
      <td>-0.304419</td>
      <td>0.147030</td>
      <td>-0.367963</td>
      <td>NaN</td>
      <td>-0.313253</td>
      <td>-0.366668</td>
      <td>-0.335817</td>
      <td>-0.427855</td>
      <td>0.079764</td>
    </tr>
    <tr>
      <th>PCT_SNAP14</th>
      <td>-0.093331</td>
      <td>-0.080684</td>
      <td>-0.003672</td>
      <td>-0.263890</td>
      <td>0.042999</td>
      <td>-0.136362</td>
      <td>-0.008115</td>
      <td>-0.259932</td>
      <td>0.003635</td>
      <td>-0.264234</td>
      <td>...</td>
      <td>-0.363802</td>
      <td>-0.186216</td>
      <td>0.042931</td>
      <td>-0.335208</td>
      <td>NaN</td>
      <td>-0.245785</td>
      <td>-0.388051</td>
      <td>-0.333137</td>
      <td>-0.445169</td>
      <td>0.054839</td>
    </tr>
    <tr>
      <th>PCH_SNAP_09_14</th>
      <td>-0.269302</td>
      <td>-0.248736</td>
      <td>0.140477</td>
      <td>-0.119052</td>
      <td>0.122258</td>
      <td>-0.094520</td>
      <td>0.119398</td>
      <td>-0.123404</td>
      <td>0.184596</td>
      <td>-0.134196</td>
      <td>...</td>
      <td>0.036678</td>
      <td>0.262685</td>
      <td>-0.190088</td>
      <td>0.023267</td>
      <td>NaN</td>
      <td>0.125574</td>
      <td>-0.121168</td>
      <td>-0.051485</td>
      <td>-0.120981</td>
      <td>-0.054543</td>
    </tr>
    <tr>
      <th>PC_SNAPBEN08</th>
      <td>-0.066898</td>
      <td>-0.067325</td>
      <td>-0.094902</td>
      <td>-0.191937</td>
      <td>-0.006581</td>
      <td>0.047172</td>
      <td>-0.086587</td>
      <td>-0.136502</td>
      <td>-0.101752</td>
      <td>-0.223542</td>
      <td>...</td>
      <td>-0.227864</td>
      <td>-0.168357</td>
      <td>0.013857</td>
      <td>-0.181775</td>
      <td>NaN</td>
      <td>-0.083309</td>
      <td>-0.187648</td>
      <td>-0.130060</td>
      <td>-0.225616</td>
      <td>-0.081343</td>
    </tr>
    <tr>
      <th>PC_SNAPBEN10</th>
      <td>-0.076502</td>
      <td>-0.079619</td>
      <td>-0.087884</td>
      <td>-0.163075</td>
      <td>0.005622</td>
      <td>0.098296</td>
      <td>-0.079930</td>
      <td>-0.099405</td>
      <td>-0.090307</td>
      <td>-0.202200</td>
      <td>...</td>
      <td>-0.301894</td>
      <td>-0.249105</td>
      <td>-0.020869</td>
      <td>-0.158668</td>
      <td>NaN</td>
      <td>-0.130791</td>
      <td>-0.231943</td>
      <td>-0.188383</td>
      <td>-0.260923</td>
      <td>-0.141032</td>
    </tr>
    <tr>
      <th>PCH_PC_SNAPBEN_08_10</th>
      <td>-0.006155</td>
      <td>-0.002022</td>
      <td>0.114578</td>
      <td>0.080039</td>
      <td>0.056286</td>
      <td>-0.047009</td>
      <td>0.104438</td>
      <td>0.070528</td>
      <td>0.145490</td>
      <td>0.087127</td>
      <td>...</td>
      <td>0.180917</td>
      <td>0.173498</td>
      <td>-0.045508</td>
      <td>0.170202</td>
      <td>NaN</td>
      <td>0.050537</td>
      <td>-0.056311</td>
      <td>0.061137</td>
      <td>-0.012865</td>
      <td>0.035004</td>
    </tr>
    <tr>
      <th>SNAP_PART_RATE08</th>
      <td>0.077127</td>
      <td>0.080683</td>
      <td>-0.100596</td>
      <td>-0.181826</td>
      <td>-0.103559</td>
      <td>-0.142488</td>
      <td>-0.102569</td>
      <td>-0.193320</td>
      <td>-0.096309</td>
      <td>-0.132408</td>
      <td>...</td>
      <td>-0.122852</td>
      <td>-0.246877</td>
      <td>-0.056387</td>
      <td>-0.201921</td>
      <td>NaN</td>
      <td>-0.204451</td>
      <td>-0.311783</td>
      <td>-0.120186</td>
      <td>-0.295036</td>
      <td>0.449616</td>
    </tr>
    <tr>
      <th>SNAP_PART_RATE10</th>
      <td>0.085016</td>
      <td>0.101552</td>
      <td>-0.077972</td>
      <td>-0.161039</td>
      <td>-0.085095</td>
      <td>-0.129554</td>
      <td>-0.083517</td>
      <td>-0.167006</td>
      <td>-0.063753</td>
      <td>-0.112845</td>
      <td>...</td>
      <td>-0.128447</td>
      <td>-0.200009</td>
      <td>-0.194032</td>
      <td>-0.041216</td>
      <td>NaN</td>
      <td>-0.175976</td>
      <td>-0.408063</td>
      <td>-0.174242</td>
      <td>-0.378347</td>
      <td>0.321516</td>
    </tr>
    <tr>
      <th>SNAP_OAPP00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>SNAP_OAPP05</th>
      <td>0.212774</td>
      <td>0.210379</td>
      <td>0.068099</td>
      <td>-0.019983</td>
      <td>0.031910</td>
      <td>-0.061822</td>
      <td>0.050550</td>
      <td>-0.038503</td>
      <td>0.112495</td>
      <td>0.002548</td>
      <td>...</td>
      <td>0.339327</td>
      <td>0.486929</td>
      <td>-0.125598</td>
      <td>-0.004218</td>
      <td>NaN</td>
      <td>0.120991</td>
      <td>0.004272</td>
      <td>0.189050</td>
      <td>0.025778</td>
      <td>0.203574</td>
    </tr>
    <tr>
      <th>SNAP_OAPP10</th>
      <td>0.217132</td>
      <td>0.233162</td>
      <td>0.115514</td>
      <td>-0.072607</td>
      <td>0.105232</td>
      <td>-0.107058</td>
      <td>0.111439</td>
      <td>-0.075502</td>
      <td>0.122938</td>
      <td>-0.058096</td>
      <td>...</td>
      <td>0.173905</td>
      <td>0.271489</td>
      <td>0.507822</td>
      <td>0.176107</td>
      <td>NaN</td>
      <td>0.064757</td>
      <td>0.295202</td>
      <td>0.126236</td>
      <td>0.285163</td>
      <td>0.119140</td>
    </tr>
    <tr>
      <th>SNAP_FACEWAIVER05</th>
      <td>0.180566</td>
      <td>0.192662</td>
      <td>0.015057</td>
      <td>0.063979</td>
      <td>0.042766</td>
      <td>0.080201</td>
      <td>0.028704</td>
      <td>0.084953</td>
      <td>-0.011328</td>
      <td>0.045745</td>
      <td>...</td>
      <td>0.049473</td>
      <td>0.098129</td>
      <td>0.341567</td>
      <td>0.089326</td>
      <td>NaN</td>
      <td>0.071815</td>
      <td>0.269614</td>
      <td>-0.006544</td>
      <td>0.229346</td>
      <td>-0.246334</td>
    </tr>
    <tr>
      <th>SNAP_FACEWAIVER10</th>
      <td>0.004849</td>
      <td>0.017843</td>
      <td>0.031878</td>
      <td>-0.070758</td>
      <td>0.027163</td>
      <td>-0.091963</td>
      <td>0.030302</td>
      <td>-0.056144</td>
      <td>0.033877</td>
      <td>-0.083212</td>
      <td>...</td>
      <td>0.103302</td>
      <td>0.098952</td>
      <td>-0.138741</td>
      <td>0.113518</td>
      <td>NaN</td>
      <td>0.066456</td>
      <td>0.068912</td>
      <td>0.090033</td>
      <td>0.126004</td>
      <td>-0.057639</td>
    </tr>
    <tr>
      <th>SNAP_VEHEXCL00</th>
      <td>0.007528</td>
      <td>0.005794</td>
      <td>-0.010563</td>
      <td>0.107142</td>
      <td>-0.021966</td>
      <td>0.080123</td>
      <td>-0.014828</td>
      <td>0.068442</td>
      <td>-0.002300</td>
      <td>0.147351</td>
      <td>...</td>
      <td>0.067537</td>
      <td>0.008277</td>
      <td>0.009204</td>
      <td>-0.069254</td>
      <td>NaN</td>
      <td>-0.038216</td>
      <td>-0.005551</td>
      <td>-0.019987</td>
      <td>-0.022081</td>
      <td>0.173248</td>
    </tr>
    <tr>
      <th>SNAP_VEHEXCL05</th>
      <td>-0.182582</td>
      <td>-0.184599</td>
      <td>-0.064598</td>
      <td>-0.170097</td>
      <td>-0.073809</td>
      <td>-0.130845</td>
      <td>-0.067652</td>
      <td>-0.182092</td>
      <td>-0.073161</td>
      <td>-0.165000</td>
      <td>...</td>
      <td>-0.094604</td>
      <td>-0.130608</td>
      <td>-0.242658</td>
      <td>0.043423</td>
      <td>NaN</td>
      <td>-0.096798</td>
      <td>-0.262114</td>
      <td>-0.102766</td>
      <td>-0.252702</td>
      <td>0.239247</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>CES_VEHINSPQ_std</th>
      <td>0.066236</td>
      <td>0.052620</td>
      <td>0.090187</td>
      <td>-0.072886</td>
      <td>0.058104</td>
      <td>-0.092209</td>
      <td>0.078184</td>
      <td>-0.096483</td>
      <td>0.106541</td>
      <td>-0.082128</td>
      <td>...</td>
      <td>0.315734</td>
      <td>0.190899</td>
      <td>0.409001</td>
      <td>-0.088098</td>
      <td>NaN</td>
      <td>0.280858</td>
      <td>0.219424</td>
      <td>0.414361</td>
      <td>0.222149</td>
      <td>0.320937</td>
    </tr>
    <tr>
      <th>CES_VEHQ_std</th>
      <td>0.052244</td>
      <td>0.032290</td>
      <td>-0.081834</td>
      <td>0.061518</td>
      <td>-0.094509</td>
      <td>0.038157</td>
      <td>-0.074210</td>
      <td>0.080443</td>
      <td>-0.096067</td>
      <td>0.041159</td>
      <td>...</td>
      <td>0.142604</td>
      <td>-0.086024</td>
      <td>-0.214661</td>
      <td>-0.070969</td>
      <td>NaN</td>
      <td>0.227820</td>
      <td>0.031598</td>
      <td>0.282631</td>
      <td>0.118418</td>
      <td>0.049442</td>
    </tr>
    <tr>
      <th>CES_VEHQL_std</th>
      <td>-0.090815</td>
      <td>-0.080388</td>
      <td>0.102652</td>
      <td>-0.006063</td>
      <td>0.064928</td>
      <td>-0.037507</td>
      <td>0.087702</td>
      <td>-0.023161</td>
      <td>0.133159</td>
      <td>0.019540</td>
      <td>...</td>
      <td>0.061398</td>
      <td>0.126952</td>
      <td>0.099513</td>
      <td>0.161632</td>
      <td>NaN</td>
      <td>-0.029619</td>
      <td>0.025919</td>
      <td>-0.073908</td>
      <td>-0.052464</td>
      <td>0.143554</td>
    </tr>
    <tr>
      <th>CES_VELECTRC_std</th>
      <td>-0.110812</td>
      <td>-0.102233</td>
      <td>0.069176</td>
      <td>-0.102695</td>
      <td>0.033232</td>
      <td>-0.143409</td>
      <td>0.057499</td>
      <td>-0.121032</td>
      <td>0.083259</td>
      <td>-0.086253</td>
      <td>...</td>
      <td>0.211125</td>
      <td>0.088049</td>
      <td>0.269587</td>
      <td>0.218070</td>
      <td>NaN</td>
      <td>0.287024</td>
      <td>0.254610</td>
      <td>0.347549</td>
      <td>0.256358</td>
      <td>0.014511</td>
    </tr>
    <tr>
      <th>CES_VELECTRP_std</th>
      <td>-0.069142</td>
      <td>-0.044017</td>
      <td>0.146649</td>
      <td>-0.044964</td>
      <td>0.120078</td>
      <td>-0.093364</td>
      <td>0.126152</td>
      <td>-0.072895</td>
      <td>0.184692</td>
      <td>-0.027688</td>
      <td>...</td>
      <td>0.289130</td>
      <td>0.412657</td>
      <td>0.384890</td>
      <td>0.320889</td>
      <td>NaN</td>
      <td>0.328804</td>
      <td>0.382313</td>
      <td>0.354396</td>
      <td>0.388110</td>
      <td>-0.013125</td>
    </tr>
    <tr>
      <th>CES_VFUELOIC_std</th>
      <td>0.000303</td>
      <td>0.006508</td>
      <td>0.043829</td>
      <td>-0.058747</td>
      <td>0.022195</td>
      <td>-0.078208</td>
      <td>0.037697</td>
      <td>-0.073545</td>
      <td>0.049269</td>
      <td>-0.042695</td>
      <td>...</td>
      <td>0.036017</td>
      <td>-0.017249</td>
      <td>0.230965</td>
      <td>0.130722</td>
      <td>NaN</td>
      <td>0.081713</td>
      <td>0.164219</td>
      <td>0.127738</td>
      <td>0.131031</td>
      <td>0.168540</td>
    </tr>
    <tr>
      <th>CES_VFUELOIP_std</th>
      <td>0.049114</td>
      <td>0.038254</td>
      <td>0.119847</td>
      <td>-0.014773</td>
      <td>0.053122</td>
      <td>-0.084283</td>
      <td>0.103514</td>
      <td>-0.023049</td>
      <td>0.144876</td>
      <td>-0.059093</td>
      <td>...</td>
      <td>0.225113</td>
      <td>0.278246</td>
      <td>0.225566</td>
      <td>0.081610</td>
      <td>NaN</td>
      <td>0.467098</td>
      <td>0.370810</td>
      <td>0.511457</td>
      <td>0.386273</td>
      <td>0.212174</td>
    </tr>
    <tr>
      <th>CES_VMISCHEC_std</th>
      <td>-0.028581</td>
      <td>-0.026595</td>
      <td>0.145830</td>
      <td>0.021266</td>
      <td>0.062980</td>
      <td>-0.029533</td>
      <td>0.129957</td>
      <td>0.012172</td>
      <td>0.170636</td>
      <td>0.014190</td>
      <td>...</td>
      <td>0.218323</td>
      <td>0.050952</td>
      <td>0.064738</td>
      <td>0.108017</td>
      <td>NaN</td>
      <td>0.141670</td>
      <td>0.029051</td>
      <td>0.150980</td>
      <td>0.021977</td>
      <td>0.185688</td>
    </tr>
    <tr>
      <th>CES_VMISCHEP_std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>CES_VNATLGAC_std</th>
      <td>0.041579</td>
      <td>0.046201</td>
      <td>0.035769</td>
      <td>-0.022837</td>
      <td>0.019034</td>
      <td>-0.060364</td>
      <td>0.030727</td>
      <td>-0.043293</td>
      <td>0.041992</td>
      <td>0.003031</td>
      <td>...</td>
      <td>0.254017</td>
      <td>0.042991</td>
      <td>0.329164</td>
      <td>-0.000949</td>
      <td>NaN</td>
      <td>0.251575</td>
      <td>0.232278</td>
      <td>0.351812</td>
      <td>0.252665</td>
      <td>0.177470</td>
    </tr>
    <tr>
      <th>CES_VNATLGAP_std</th>
      <td>0.155491</td>
      <td>0.166431</td>
      <td>0.096940</td>
      <td>-0.080542</td>
      <td>0.046609</td>
      <td>-0.143795</td>
      <td>0.083992</td>
      <td>-0.111415</td>
      <td>0.115606</td>
      <td>-0.047000</td>
      <td>...</td>
      <td>0.306591</td>
      <td>0.155636</td>
      <td>0.406763</td>
      <td>0.105880</td>
      <td>NaN</td>
      <td>0.365013</td>
      <td>0.230959</td>
      <td>0.462469</td>
      <td>0.257756</td>
      <td>0.405688</td>
    </tr>
    <tr>
      <th>CES_VOTHRFLC_std</th>
      <td>0.053035</td>
      <td>0.046556</td>
      <td>0.050838</td>
      <td>0.035027</td>
      <td>0.022480</td>
      <td>-0.018349</td>
      <td>0.043731</td>
      <td>0.026910</td>
      <td>0.060692</td>
      <td>0.021827</td>
      <td>...</td>
      <td>0.288979</td>
      <td>0.178599</td>
      <td>0.246818</td>
      <td>0.156000</td>
      <td>NaN</td>
      <td>0.392100</td>
      <td>0.244699</td>
      <td>0.414853</td>
      <td>0.278586</td>
      <td>0.104355</td>
    </tr>
    <tr>
      <th>CES_VOTHRFLP_std</th>
      <td>0.001369</td>
      <td>0.022003</td>
      <td>0.024301</td>
      <td>-0.020685</td>
      <td>0.008009</td>
      <td>-0.060685</td>
      <td>0.016073</td>
      <td>-0.027973</td>
      <td>0.038448</td>
      <td>-0.008947</td>
      <td>...</td>
      <td>0.109627</td>
      <td>0.102539</td>
      <td>0.024853</td>
      <td>0.190826</td>
      <td>NaN</td>
      <td>0.147998</td>
      <td>0.094170</td>
      <td>0.177689</td>
      <td>0.142370</td>
      <td>0.102429</td>
    </tr>
    <tr>
      <th>CES_VOTHRLOC_std</th>
      <td>-0.002398</td>
      <td>-0.012361</td>
      <td>0.135999</td>
      <td>0.064723</td>
      <td>0.062084</td>
      <td>-0.029051</td>
      <td>0.117716</td>
      <td>0.039778</td>
      <td>0.166956</td>
      <td>0.079065</td>
      <td>...</td>
      <td>0.289144</td>
      <td>0.184245</td>
      <td>0.300886</td>
      <td>0.310295</td>
      <td>NaN</td>
      <td>0.412197</td>
      <td>0.448292</td>
      <td>0.497684</td>
      <td>0.472253</td>
      <td>0.133535</td>
    </tr>
    <tr>
      <th>CES_VOTHRLOP_std</th>
      <td>-0.003977</td>
      <td>-0.005293</td>
      <td>0.091796</td>
      <td>-0.037790</td>
      <td>0.067797</td>
      <td>-0.078856</td>
      <td>0.081389</td>
      <td>-0.052534</td>
      <td>0.103099</td>
      <td>-0.040147</td>
      <td>...</td>
      <td>0.340539</td>
      <td>0.311835</td>
      <td>0.015931</td>
      <td>0.342816</td>
      <td>NaN</td>
      <td>0.421032</td>
      <td>0.223553</td>
      <td>0.335443</td>
      <td>0.261099</td>
      <td>-0.222583</td>
    </tr>
    <tr>
      <th>CES_VRNTLOCQ_std</th>
      <td>-0.265304</td>
      <td>-0.249898</td>
      <td>0.182076</td>
      <td>0.130533</td>
      <td>0.141293</td>
      <td>0.068903</td>
      <td>0.163641</td>
      <td>0.115232</td>
      <td>0.224952</td>
      <td>0.147208</td>
      <td>...</td>
      <td>0.204502</td>
      <td>0.301551</td>
      <td>-0.055710</td>
      <td>0.304664</td>
      <td>NaN</td>
      <td>0.186930</td>
      <td>0.294351</td>
      <td>0.102837</td>
      <td>0.243598</td>
      <td>-0.119112</td>
    </tr>
    <tr>
      <th>CES_VRNTLOPQ_std</th>
      <td>0.036183</td>
      <td>0.042281</td>
      <td>0.137868</td>
      <td>0.046356</td>
      <td>0.111173</td>
      <td>-0.010121</td>
      <td>0.126039</td>
      <td>0.021939</td>
      <td>0.159647</td>
      <td>0.068610</td>
      <td>...</td>
      <td>0.356983</td>
      <td>0.427312</td>
      <td>0.428110</td>
      <td>0.299515</td>
      <td>NaN</td>
      <td>0.199406</td>
      <td>0.441262</td>
      <td>0.287515</td>
      <td>0.409940</td>
      <td>-0.022298</td>
    </tr>
    <tr>
      <th>CES_VWATERPC_std</th>
      <td>0.251427</td>
      <td>0.257229</td>
      <td>0.108004</td>
      <td>0.045227</td>
      <td>0.072332</td>
      <td>-0.019740</td>
      <td>0.100349</td>
      <td>0.026363</td>
      <td>0.118041</td>
      <td>0.044158</td>
      <td>...</td>
      <td>0.506729</td>
      <td>0.447461</td>
      <td>0.501402</td>
      <td>-0.171248</td>
      <td>NaN</td>
      <td>0.383364</td>
      <td>0.395144</td>
      <td>0.515861</td>
      <td>0.411214</td>
      <td>0.205028</td>
    </tr>
    <tr>
      <th>CES_VWATERPP_std</th>
      <td>0.054538</td>
      <td>0.069033</td>
      <td>0.117500</td>
      <td>-0.048733</td>
      <td>0.076417</td>
      <td>-0.092260</td>
      <td>0.098764</td>
      <td>-0.071506</td>
      <td>0.152741</td>
      <td>-0.042988</td>
      <td>...</td>
      <td>0.403925</td>
      <td>0.487231</td>
      <td>0.265483</td>
      <td>-0.006159</td>
      <td>NaN</td>
      <td>0.455761</td>
      <td>0.251591</td>
      <td>0.521368</td>
      <td>0.297574</td>
      <td>0.337655</td>
    </tr>
    <tr>
      <th>CES_WATERHT_std</th>
      <td>0.209749</td>
      <td>0.202673</td>
      <td>0.084877</td>
      <td>-0.091996</td>
      <td>0.017783</td>
      <td>-0.122355</td>
      <td>0.072517</td>
      <td>-0.105574</td>
      <td>0.092337</td>
      <td>-0.102330</td>
      <td>...</td>
      <td>0.076191</td>
      <td>-0.031172</td>
      <td>0.393546</td>
      <td>0.048018</td>
      <td>NaN</td>
      <td>0.255014</td>
      <td>0.241000</td>
      <td>0.315309</td>
      <td>0.212909</td>
      <td>0.498062</td>
    </tr>
    <tr>
      <th>CES_WATRPSCQ_std</th>
      <td>-0.090276</td>
      <td>-0.108750</td>
      <td>0.211475</td>
      <td>0.078081</td>
      <td>0.152635</td>
      <td>-0.015167</td>
      <td>0.194859</td>
      <td>0.053151</td>
      <td>0.243962</td>
      <td>0.087898</td>
      <td>...</td>
      <td>1.000000</td>
      <td>0.810559</td>
      <td>0.300181</td>
      <td>0.247167</td>
      <td>NaN</td>
      <td>0.506441</td>
      <td>0.467559</td>
      <td>0.616968</td>
      <td>0.539846</td>
      <td>0.069979</td>
    </tr>
    <tr>
      <th>CES_WATRPSPQ_std</th>
      <td>-0.029702</td>
      <td>-0.032423</td>
      <td>0.189190</td>
      <td>0.073386</td>
      <td>0.158271</td>
      <td>0.013740</td>
      <td>0.173740</td>
      <td>0.050514</td>
      <td>0.222335</td>
      <td>0.079362</td>
      <td>...</td>
      <td>0.810559</td>
      <td>1.000000</td>
      <td>0.240940</td>
      <td>0.263607</td>
      <td>NaN</td>
      <td>0.484258</td>
      <td>0.443853</td>
      <td>0.502657</td>
      <td>0.485019</td>
      <td>-0.151236</td>
    </tr>
    <tr>
      <th>CES_WELFAREB_std</th>
      <td>0.494726</td>
      <td>0.493055</td>
      <td>0.110039</td>
      <td>-0.028346</td>
      <td>0.097096</td>
      <td>-0.044804</td>
      <td>0.111565</td>
      <td>-0.042987</td>
      <td>0.093470</td>
      <td>-0.033220</td>
      <td>...</td>
      <td>0.300181</td>
      <td>0.240940</td>
      <td>1.000000</td>
      <td>-0.006104</td>
      <td>NaN</td>
      <td>0.341348</td>
      <td>0.634923</td>
      <td>0.497427</td>
      <td>0.615452</td>
      <td>0.106999</td>
    </tr>
    <tr>
      <th>CES_WELFAREI_std</th>
      <td>-0.130957</td>
      <td>-0.132797</td>
      <td>0.112132</td>
      <td>0.029399</td>
      <td>0.078706</td>
      <td>-0.032070</td>
      <td>0.104240</td>
      <td>0.020719</td>
      <td>0.127424</td>
      <td>0.037170</td>
      <td>...</td>
      <td>0.247167</td>
      <td>0.263607</td>
      <td>-0.006104</td>
      <td>1.000000</td>
      <td>NaN</td>
      <td>0.233294</td>
      <td>0.145097</td>
      <td>0.151349</td>
      <td>0.145197</td>
      <td>-0.076629</td>
    </tr>
    <tr>
      <th>CES_WINDOWAC_std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>CES_WOMGRLCQ_std</th>
      <td>-0.041758</td>
      <td>-0.057590</td>
      <td>0.141888</td>
      <td>0.008136</td>
      <td>0.080647</td>
      <td>-0.043802</td>
      <td>0.136906</td>
      <td>0.006240</td>
      <td>0.137348</td>
      <td>-0.042446</td>
      <td>...</td>
      <td>0.506441</td>
      <td>0.484258</td>
      <td>0.341348</td>
      <td>0.233294</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>0.551383</td>
      <td>0.933138</td>
      <td>0.558541</td>
      <td>-0.045663</td>
    </tr>
    <tr>
      <th>CES_WOMGRLPQ_std</th>
      <td>0.060190</td>
      <td>0.050669</td>
      <td>0.163084</td>
      <td>0.135848</td>
      <td>0.110861</td>
      <td>0.052483</td>
      <td>0.162738</td>
      <td>0.133672</td>
      <td>0.147420</td>
      <td>0.091596</td>
      <td>...</td>
      <td>0.467559</td>
      <td>0.443853</td>
      <td>0.634923</td>
      <td>0.145097</td>
      <td>NaN</td>
      <td>0.551383</td>
      <td>1.000000</td>
      <td>0.641364</td>
      <td>0.975629</td>
      <td>-0.154610</td>
    </tr>
    <tr>
      <th>CES_WOMSIXCQ_std</th>
      <td>0.040847</td>
      <td>0.016636</td>
      <td>0.141098</td>
      <td>0.034067</td>
      <td>0.078411</td>
      <td>-0.036339</td>
      <td>0.135360</td>
      <td>0.025514</td>
      <td>0.138918</td>
      <td>-0.005059</td>
      <td>...</td>
      <td>0.616968</td>
      <td>0.502657</td>
      <td>0.497427</td>
      <td>0.151349</td>
      <td>NaN</td>
      <td>0.933138</td>
      <td>0.641364</td>
      <td>1.000000</td>
      <td>0.672675</td>
      <td>0.076700</td>
    </tr>
    <tr>
      <th>CES_WOMSIXPQ_std</th>
      <td>0.054743</td>
      <td>0.042205</td>
      <td>0.158161</td>
      <td>0.145976</td>
      <td>0.109655</td>
      <td>0.054818</td>
      <td>0.156621</td>
      <td>0.138429</td>
      <td>0.145248</td>
      <td>0.112250</td>
      <td>...</td>
      <td>0.539846</td>
      <td>0.485019</td>
      <td>0.615452</td>
      <td>0.145197</td>
      <td>NaN</td>
      <td>0.558541</td>
      <td>0.975629</td>
      <td>0.672675</td>
      <td>1.000000</td>
      <td>-0.143426</td>
    </tr>
    <tr>
      <th>CES_YRBUILT_std</th>
      <td>0.124638</td>
      <td>0.129827</td>
      <td>0.030863</td>
      <td>-0.130053</td>
      <td>-0.043724</td>
      <td>-0.179561</td>
      <td>0.017344</td>
      <td>-0.152206</td>
      <td>0.049154</td>
      <td>-0.088942</td>
      <td>...</td>
      <td>0.069979</td>
      <td>-0.151236</td>
      <td>0.106999</td>
      <td>-0.076629</td>
      <td>NaN</td>
      <td>-0.045663</td>
      <td>-0.154610</td>
      <td>0.076700</td>
      <td>-0.143426</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
<p>1235 rows × 1235 columns</p>
</div>




```python
#checking out a correlation matrix with matplotlib
plt.matshow(df_data_1.corr())
#we notice that there is a great deal of variables which makes it hard to read!
```




    <matplotlib.image.AxesImage at 0x7f891344a128>




![png](output_13_1.png)



```python
#other stats
df_data_1.max()
df_data_1.min()
df_data_1.std()
```




    Unnamed: 0                   941.802616
    FIPS                       16375.524971
    LACCESS_POP10              51254.806435
    PCT_LACCESS_POP10             20.231676
    LACCESS_LOWI10             13849.378974
    PCT_LACCESS_LOWI10             8.212651
    LACCESS_CHILD10            13155.181390
    PCT_LACCESS_CHILD10            4.875273
    LACCESS_SENIORS10           6519.993517
    PCT_LACCESS_SENIORS10          4.212330
    LACCESS_HHNV10              1120.253645
    PCT_LACCESS_HHNV10             3.206407
    REDEMP_SNAPS08             97745.876587
    REDEMP_SNAPS12            125533.099779
    PCH_REDEMP_SNAPS_08_12        35.284667
    PCT_SNAP09                     3.139895
    PCT_SNAP14                     3.519111
    PCH_SNAP_09_14                 1.357692
    PC_SNAPBEN08                   7.075854
    PC_SNAPBEN10                   9.855525
    PCH_PC_SNAPBEN_08_10          34.769849
    SNAP_PART_RATE08              10.406635
    SNAP_PART_RATE10               8.965901
    SNAP_OAPP00                    0.000000
    SNAP_OAPP05                    0.365205
    SNAP_OAPP10                    0.477862
    SNAP_FACEWAIVER05              0.403137
    SNAP_FACEWAIVER10              0.307124
    SNAP_VEHEXCL00                 0.272805
    SNAP_VEHEXCL05                 0.399108
                                  ...      
    CES_VEHINSPQ_std              48.925556
    CES_VEHQ_std                   0.200146
    CES_VEHQL_std                  0.085709
    CES_VELECTRC_std               9.143066
    CES_VELECTRP_std              16.039146
    CES_VFUELOIC_std              20.820641
    CES_VFUELOIP_std               9.380419
    CES_VMISCHEC_std               0.012837
    CES_VMISCHEP_std               0.000000
    CES_VNATLGAC_std               7.782216
    CES_VNATLGAP_std               9.175211
    CES_VOTHRFLC_std               7.249821
    CES_VOTHRFLP_std              10.395105
    CES_VOTHRLOC_std             102.162451
    CES_VOTHRLOP_std             198.393619
    CES_VRNTLOCQ_std              61.686233
    CES_VRNTLOPQ_std             101.492763
    CES_VWATERPC_std               5.702207
    CES_VWATERPP_std               9.687167
    CES_WATERHT_std                0.115692
    CES_WATRPSCQ_std              10.964508
    CES_WATRPSPQ_std              17.466676
    CES_WELFAREB_std               0.890585
    CES_WELFAREI_std               4.850197
    CES_WINDOWAC_std               0.000000
    CES_WOMGRLCQ_std              26.669356
    CES_WOMGRLPQ_std              50.179029
    CES_WOMSIXCQ_std              28.662127
    CES_WOMSIXPQ_std              48.519222
    CES_YRBUILT_std                6.232962
    dtype: float64




```python
# Plot counts of a specified column using Pandas
df_data_1.FOODINSEC_10_12.value_counts().plot(kind='barh')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f8497945eb8>




![png](output_15_1.png)



```python
# Bar plot example
sns.factorplot("PCT_SNAP09", "PCT_OBESE_ADULTS10", data=df_data_1,size=3,aspect=2)
```




    <seaborn.axisgrid.FacetGrid at 0x7f37285631d0>




![png](output_16_1.png)



```python
# Regression plot
sns.regplot("FOODINSEC_10_12", "PCT_OBESE_ADULTS10", data=df_data_1, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_17_0.png)


After looking at the data I realize that I'm only interested in seeing the connection between certain values and because the dataset is so large it's bringing in irrelevant information and creating noise. To change this, I created a smaller data frame, making sure to remove NaN and 0 values (0s in this dataset generally mean that a number was not recorded).


```python
#create a dataframe of values that are most interesting to food insecurity
df_focusedvalues = df_data_1[["State", "County","PCT_REDUCED_LUNCH10", "PCT_DIABETES_ADULTS10", "PCT_OBESE_ADULTS10", "FOODINSEC_10_12", "PCT_OBESE_CHILD11", "PCT_LACCESS_POP10", "PCT_LACCESS_CHILD10", "PCT_LACCESS_SENIORS10", "SNAP_PART_RATE10", "PCT_LOCLFARM07", "FMRKT13", "PCT_FMRKT_SNAP13", "PCT_FMRKT_WIC13", "FMRKT_FRVEG13", "PCT_FRMKT_FRVEG13", "PCT_FRMKT_ANMLPROD13", "FOODHUB12", "FARM_TO_SCHOOL", "SODATAX_STORES11", "State_y", "GROC12", "SNAPS12", "WICS12", "PCT_NHWHITE10", "PCT_NHBLACK10", "PCT_HISP10", "PCT_NHASIAN10", "PCT_65OLDER10", "PCT_18YOUNGER10", "POVRATE10", "CHILDPOVRATE10"]]
```


```python
#remove NaNs and 0s
df_focusedvalues = df_focusedvalues[(df_focusedvalues != 0).all(1)]
df_focusedvalues = df_focusedvalues.dropna(how='any')
```

Before visualizing, a quick heatmap is created so that we can see what correlations we may want to visualize. I visualized a few of these relationships using seaborn, but I ultimately want to try out other visualizations. The quickest way to explore these is through Pixie Dust.


```python
#look at heatmap of correlations with the dataframe to see what we should visualize
corr = df_focusedvalues.corr()
sns.heatmap(corr, 
            xticklabels=corr.columns.values,
            yticklabels=corr.columns.values)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f7fe4176208>




![png](output_22_1.png)


We can immediately see that a fair amount of strong correlations and relationships exist. Some of these include 18 and younger and Hispanic, an inverse relationship between Asian and obese, a correlation between sodatax and Hispanic, African American and obesity as well as food insecurity, sodatax and obese minors, farmers markets and aid such as WIC and SNAP, obese minors and reduced lunches and a few more.

Let's try and plot some of these relationships with seaborn.


```python
#Percent of the population that is white vs SNAP aid participation (positive relationship)
sns.regplot("PCT_NHWHITE10", "SNAP_PART_RATE10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_24_0.png)



```python
#Percent of the population that is Hispanic vs SNAP aid participation (negative relationship)
sns.regplot("SNAP_PART_RATE10", "PCT_HISP10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_25_0.png)



```python
#Eligibility and use of reduced lunches in schools vs percent of the population that is Hispanic (positive relationship)
sns.regplot("PCT_REDUCED_LUNCH10", "PCT_HISP10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_26_0.png)



```python
#Percent of the population that is black vs percent of the population with diabetes (positive relationship)
sns.regplot("PCT_NHBLACK10", "PCT_DIABETES_ADULTS10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_27_0.png)



```python
#Percent of population with diabetes vs percent of population with obesity (positive relationship)
sns.regplot("PCT_DIABETES_ADULTS10", "PCT_OBESE_ADULTS10", data=df_focusedvalues, robust=True, ci=95, color="seagreen")
sns.despine();
```


![png](output_28_0.png)


### Now, let's visualize with Pixie Dust.

Now that we've gained some initial insights, let's try out a different tool: Pixie Dust!

As you can see in the notebook below, to activate Pixie Dust, we just import it and then write:

 ```display(your_dataframe_name)```
 
After doing this your dataframe will show up in a column-row table format. To visualize your data, you can click the chart icon at the top left (looks like an arrow going up). From there you can choose from a variety of visuals. Once you select the type of chart you want, you can then select the variables you want to showcase. It's worth playing around with this to see how you can create the most effective visualizations for your audience. The notebook below showcases a couple options such as scatterplots, bar charts, line charts, and histograms.


```python
import pixiedust
```

    Pixiedust database opened successfully



```python
#looking at the dataframe table. Pixie Dust does this automatically, but to find it again you can click the table icon.
display(df_focusedvalues)
```

```python
#using seaborn in Pixie Dust to look at Food Insecurity and the Percent of the population that is black in a scatter plot
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Food Insecurity vs Percent of the population that is black
        </div>
    


```python
#using matplotlib in Pixie Dust to view Food Insecurity by state in a bar chart
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Food Insecurity by State
        </div>
    
        <div id="chartFigure5ece6881" class="pd_save" style="overflow-x:auto">
            
                    
                            <center><img style="max-width:initial !important" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAy8AAAJzCAYAAAAGFiwIAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAOwwAADsMBx2+oZAAAIABJREFUeJzt3X2clXWd+P/3OWecFSGPjGsKPpQyTUY05FZA1NJly31kakZrGGiZd4mpmfUoyq9abmZZWuiabuaSeFN5g93thnanoCI3at6blpmlsAw73E0MnnP9/uDHrJMYgsx1zYfzfD4ePMa5rnM473lLNK855zqWsizLAgAAoJcrFz0AAADA6yFeAACAJIgXAAAgCeIFAABIQlPRA+Qly7Joa1sV9br3J8hDuVyKlpa+dp4jO8+fnefPzvNn5/mz8/zZef7K5VLsuGO/Tb9fD8zSK5VKpSiVSkWP0TDW79vO82Pn+bPz/Nl5/uw8f3aePzvP3+buumHiBQAASJt4AQAAkiBeAACAJIgXAAAgCeIFAABIgngBAACSIF4AAIAkiBcAACAJ4gUAAEiCeAEAAJIgXgAAgCSIFwAAIAniBQAASEJT0QPkZcGCBdHe3hH1er3oURpCuVyOarXP6955a+uQaG5uzmEyAABS1TDxMnLkRRGxU9FjsEFLYvbss2Lo0GFFDwIAQC/WMPGyLlwGFj0EAACwmVzzAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACSh8Hi5/vrrY/z48d2O3XfffTF48OC48sorux0/44wz4txzz43p06fHpEmT8hwTAAAoWOHxMm7cuFi6dGk89dRTXcfmzp0bb3/72+Pee+/tOlav1+P++++PAw88MCIiSqVS7rMCAADFKTxe9thjjxgwYEDMnTu369icOXPijDPOiEceeSQ6OjoiIuLhhx+OFStWxLhx44oaFQAAKFDh8RKx7tmXOXPmRETEsmXL4tlnn41DDjkkWltbY968eRGx7tmYPffcM9785jcXOSoAAFCQXhEvBx54YMyfPz/Wrl0b9913XwwdOjSam5u7Rc3cuXO7XjIGAAA0nqaiB4iIGDt2bHR2dsbChQtj7ty5XS8NGzNmTJx//vmxevXqePDBB+OUU04peFJ6SrlcjkqlV7R0siqV0is+2mUe7Dx/dp4/O8+fnefPzvO3fuebqlfEyw477BD77LNPzJkzJ+bMmROXX355RETsv//+8ec//znuuOOOKJfLMWrUqIInpadUq32ipaVv0WNsFarV7YoeoeHYef7sPH92nj87z5+d9369Il4i1l33cuutt0ZnZ2fst99+ERHR1NQUI0eOjCuuuCKGDRsW2267bcFT0lPa2zuirW1V0WMkrVIpRbW6XbS3r45aLSt6nIZg5/mz8/zZef7sPH92nr/1O99UvSZexo8fH1dffXVMmDCh2/Fx48bF3XffHZMnTy5oMvJQr9ejVqsXPUbi1j3NXatldpkbO8+fnefPzvNn5/mz8/xt3svzSlmWNURelkqnRMTAosdgg/4cs2cfF0OHDit6kKRVKuVoaekbbW2r/MWbEzvPn53nz87zZ+f5s/P8rd/5pnJFEgAAkATxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEloKnqA/CwpegBek383AABsXMPEy/z506K9vSPq9XrRozSEcrkc1Wqf173z1tYhOUwFAEDKGiZeRowYEW1tq6JWEy95qFTK0dLS184BANhiXPMCAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEkQLwAAQBLECwAAkATxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEkQLwAAQBLECwAAkATxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEloKnqAvCxYsCDa2zuiXq8XPUpDKJfLUa32SWbnra1Dorm5uegxAAD4OxomXkaOvCgidip6DHqlJTF79lkxdOiwogcBAODvaJh4WRcuA4seAgAA2EyueQEAAJIgXgAAgCSIFwAAIAniBQAASIJ4AQAAkiBeAACAJIgXAAAgCeIFAABIgngBAACSkGu8XH/99TF+/Phux+67774YPHhwXHnlld2OT506Nc4999yuz59//vkYPHhwHHvssbnMCgAA9C65xsu4ceNi6dKl8dRTT3Udmzt3brz97W+Pe++9t+tYvV6PefPmdQudG2+8Mfr37x8PPfRQPPHEE3mODQAA9AK5xssee+wRAwYMiLlz53YdmzNnTpxxxhnxyCOPREdHR0REPPzww7FixYoYO3ZsRER0dnbGrbfeGieffHLs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vJfn9YoX9ZVKG37N20033RTjxo2LUaNGxY477tj1a9CgQTFx4sS48cYbc54UAAAoSq945mXGjBkbPD59+vTXvM/nPve5nhoHAADohXrFMy8AAAAbI14AAIAkiBcAACAJ4gUAAEiCeAEAAJIgXgAAgCSIFwAAIAniBQAASIJ4AQAAkiBeAACAJIgXAAAgCU1FD5CfJUUPAHTxv0cAYNM1TLzMnz8t2ts7ol6vFz1KQyiXy1Gt9rHzHKW289bWIUWPAAAkpmHiZcSIEdHWtipqtd7/Td3WoFIpR0tLXzvPkZ0DAFs717wAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEkQLwAAQBLECwAAkATxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEkQLwAAQBLECwAAkATxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJEC8AAEASxAsAAJAE8QIAACRBvAAAAEkQLwAAQBKaNuXGL7/8cixevDh22mmn2GabbXpqph6xYMGCaG/viHq9XvQoDaFcLke12sfOc2Tn+eupnbe2Donm5uYt9vsBwNbidcXL7Nmz48orr4ynnnoqsiyLH/zgBzFkyJCYNm1ajBw5Mo4++uienvMNGznyoojYqegxADZiScyefVYMHTqs6EEAoNfZaLzcfPPNceGFF8axxx4bp556apx55pld5972trfFLbfckkS8rAuXgUUPAQAAbKaNxst//Md/xGmnnRZTp06NWq3W7dzb3va2uPrqq3tsOAAAgPU2esH+iy++GCNGjNjguW222SY6Ojq2+FAAAAB/a6Pxsttuu8WiRYs2eG7RokWxxx57bPGhAAAA/tZG4+XDH/5wXHXVVXHdddfFiy++GBERK1asiB//+Mfxne98J6ZMmdLjQwIAAGz0mpdJkybFihUr4vLLL4+vfOUrERHxkY98JLbddts47bTTErlYHwAASN3reqvkU045JY477rh48MEHY9myZVGtVmP//feP7bffvqfnAwAAiIjX8bKx6dOnx0svvRT9+vWL8ePHxxFHHBEHH3xwbL/99rF48eKYPn16HnMCAAANbqPxcsUVV8RLL720wXOLFy+OK664YosPBQAA8Lc2Gi9Zlr3mud/97nexww47bNGBAAAA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b7/94vnnn49Vq1YVONnW5e67746BAwfGm970poiw957y61//OkaNGhXveMc74pJLLokTTjgh+vfvHxF23lOmTZsWp512Wuyyyy7djtt3z3nsscdi3Lhxceihh8Y555wTf/rTnyLCznvKX//611i0aFGUy+WYOHFiHHDAAXHMMcfEz3/+84iw95527733xnPPPRfHHntsRNh3TzrppJNi9uzZsWTJkli7dm1cf/31MWjQoBg8eHA8/vjjm7T3pjwHz9vKlSsjIrq+qVtv++237zpHz1m5cmVsv/323Y6t/3zlypXRt2/fIsbaqsydOzeuvPLKbq8Ntfeeccghh8QDDzwQy5cvj9tuu63bN9R2vuXNnDkzIiImTpz4qnP23TPe8573xDHHHBMDBgyIxYsXxyWXXBIf/ehHY9asWXbeQ9rb26Ner8esWbPi29/+drS2tsZdd90VZ599dlx//fX23sNuvPHGOOigg2LgwIER4e+WnjR8+PCYNWtWHHTQQdHU1BTVajWmT58ezc3Nm7z3rfqZl379+kVExIoVK7odX758edc5ek6/fv1i+fLl3Y6t/9z+37hf/vKXceaZZ8bXvva1OPDAA7uO23vP2n777WPKlCkxbdq0ePLJJyPCzre0559/Pv793/89vvSlL23wvH33jD333DMGDBgQERFvfvOb49/+7d/ixRdfjEWLFtl5D1n/Tdn73//+GDJkSJTL5ZgwYUIccMABcdddd9l7D1q8eHH84he/iEmTJnUds++ekWVZTJkyJXbaaad44IEH4uGHH44LL7wwTjrppHjiiSc2ee9bfbzsuuuu8dvf/rbrWK1Wi8ceeyxaW1sLnKwxrH953ivfgezhhx+O3XbbzU8v3qA77rgjPv3pT8fll18ehx12WLdz9t7zarVavPzyy/Hcc89FhJ1vafPnz4/29vZ4//vfH2PGjIkxY8ZERMQnPvGJOO+886K1tTUee+wx+85JlmX+jPeQfv36xe677/6a5+2959x8880xYMCAOPjgg7uO2XfPaG9vjz/96U8xZcqUeNOb3hTlcjkOO+yw2H333eOee+7Z5L1v1fESETFp0qS49tpr4+mnn46//vWvcdlll0Vzc3NMmDCh6NG2CvV6PTo7O6OzszMiItasWROdnZ2RZVlMmDAhyuVyfPOb34w1a9bEk08+Gd/97nfjuOOOK3jqtF1//fXxpS99Ka666qoYN27cq87b+5Y3Y8aMWLp0aUREtLW1xQUXXBDNzc0xfPjwiLDzLe1f/uVf4s4774zbb789Zs2aFbNmzYqIiAsvvDA+9alPxYQJE6JSqdj3Fvazn/0sli1bFhER//M//xOf//znY6eddophw4b5M96DPvzhD8ett94aTzzxRGRZFnfddVfMnz8//vmf/9nee0itVosf/OAHXde6rGffPWOHHXaIPffcM2bOnBkrV66MLMvil7/8Zfzud7+Lfffdd5P3XsqyLMv5a8jdt771rbj55ptj1apVse+++8Z5550Xe+21V9FjbRVuu+22+OxnPxulUiki1v2ErlQqxYwZM2LUqFHx1FNPxQUXXBCPPvpo9OvXLz70oQ/F6aefXvDUaRs8eHA0NTVFc3NzRPzfzq+55poYMWJERIS9b2GnnnpqPPLII7Fq1aro169f7LfffnH66afHkCFDum5j5z2rtbU1rr322hg7dmxE2HdPOO200+Khhx6Kjo6O2H777WPkyJFx1llnxW677RYRdt6Trr766rjhhhtixYoVMWjQoDjjjDPiXe96V0TYe0/4+c9/Hueee278+te/jh122KHbOfvuGX/84x/jK1/5SixatCg6OztjwIABMWXKlK7rGjdl7w0RLwAAQPq2+peNAQAAWwfxAgAAJEG8AAAASRAvAABAEsQLAACQBPECAAAkQbwAAABJ+P8A1epTdiZmqQIAAAAASUVORK5CYII=" class="pd_save"></center>
                        
                    
                
        </div>
    



```python
#using bokeh in Pixie Dust to view the percent of the population that is black vs the percent of the population that is obese in a line chart
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Percent of Population that is Black vs Percent of Population that is Obese
        </div>
    
        <div id="chartFigure903d94f8" class="pd_save" style="overflow-x:auto">
            
                    <script class="pd_save">
                    if ( !window.Bokeh && !window.autoload){
                        window.autoload=true;
                        


```python
#using seaborn in Pixie Dust to view obesity vs diabetes in a scatterplot
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Obesity vs Diabetes
        </div>
    
        <div id="chartFigure4bb3b1e0" class="pd_save" style="overflow-x:auto">
            
                    
                            <center><img style="max-width:initial !important" src="data:image/png;base64,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9TfqjjtGlbr8jBnT9K9/rfG8c8Xtdquw8DfdfPNfNG7cBEnS7t07NX/+XP3ww/cKCgrSJZe01733TlD9+vX98meqrPfee1tvvrlMx4/nqGnTZho37n5dcsmlpS5fUlKiuXNnad26tSopKVH79h00YcKDOu+8ep5ltm7dojlzZunnn/cqLi5egwYN0Y033lTubXz77TeaPfs5HTx4QC6XU+eee54GDLjFaxtPP/2EPv54jUJDwzzvd+vf/2aNHj22Go4SzhZnUABUm5KSkhqxz4KCAk2YcK8uvri9Vq/+TDNnzlV6+gq9/fYbpa4zceIUrV37b338cYY+/jhDCxYslsViUe/efSSdLCyTJt2nCy5oqVWrPtY776xUUFCwnnjioSrPX5U+++wTLVr0ov72t79rzZp16tOnryZO/Kt++eVoqevMnTtL//3vdi1e/LqWL1+jqKhoPfjg/Z7PDx48qIkT/6q+fW/URx+t10MPPaYFC9K0YcP6cm+jcePGevLJZ7V69af66KP1evzxp/Xyywu1adNGryxXXnm1Pv44w/PdUE6Mi4IC1BL33nuXZs9+Tg8/PElXX91Df/lLf3300Ydey3z77Te699671KfPlbr55r566aUFcrl+f4Hbc889pZtv7qtevbrrlltu0Msv/8PnPh577CFde21PPf/8DOXl5emRRx5UaupVuuaaHho0aIAyMj7zrJOZuVF33jlEvXtfrkGDBuiNN5Z5vbW8W7eOeu+9t3T33XeqV6/uGjp0oHbs2Ob5fPHihbrnnhF66aUFuvHGa3XHHYOr+tApI+Mzud1ujRgxWiEhIWrR4nwNHDhE77//drm38f77b6tNmwvVsmVrSVJ+fr5OnDiu667rq+DgYNWpU0e9e/fRDz9871nnyJHD6t37Cv33v9tL3e6pYz5lykR16NBBt9xy42nfa1X64IN31adPX11ySXsFBwerf/+b1aRJE61evcrn8sXFxVqzZpVGjrxb551XTxEREbr33vH66af/eb7H999/X02bJujGG29ScHCw2rfvoD59+uq9994u9zZiYs5R/foNvPZtsViUlbW32o4FqheXeIBa5MMPV2rq1Gc0deqz2rx5kx56aKIaN26idu0u1s8/79V9992jKVMe1RVXXKmjR4/owQfvV1hYmIYMGS5Jatu2nUaNGqNzzjlHO3d+o0mT/qpzzz1Pffv28+xj9ep0Pfnks3riiaflcJTopZde0m+//aZ3301XeHi4jhw5rMLCQknSrl3f6uGHJ+mxx55U9+5X6IcfvtcDD4xXcHCwbr75L55tpqev0FNPTVf9+g2UljZbf//7I3r33d9/Ie7c+Y06d07WO++s9CpUf7Rs2StatuxVWSwWTwH64+WX+vUb6JVX/ulz3T17vtcFF7SS1fr7v+natGmrgwcPqKCgQBEREWc87gUFdq1d+y9NmPCAZywqKkr9+9+sVauW6+6771VxcYlWr16lHj16epapV6++Pvpo3Rm3LZ38Xp966jm9+OI8rVnziR544H7P9+rL0KEDdfToEc+f/dSxOHXZ47bbhmrw4KE+1/3hh+91ww39vcZat26rH374zufyP/+cpeLiYrVp09YzdqpM/PDDd7r00g7avXu32rZtd9o2TxWtrKy9Z9zGxRe394zfdNP1ys7OVklJsZo3b6Frrunjtd3MzI1KTb1KkZFRSkrqpBEj7tY555zjMzsCi4IC1CJdunRVcvJlkqTk5K7q3v1yffjhSrVrd7Hef/9dXXZZd/XseZWkk78cBw68XYsX/8NTUFJTb/Bsq23bdrr66uv0n/984VVQLrusuzp3TpYkhYWFKSQkRCdOnNDevf9Tq1ZtVK/e7/Mr0tNXKCXlMl1++ZWSpFatWmvQoCFaseI9r4IycOAQNWzYSJJ0/fX99O67byknJ0exsbGSpLi4eA0deucZ/+y33TZMt902rFLH7dSb1/8oKipa0snyUVZBWbMmXaGhoerZs5fX+OWXX6kZM6bp6qt7SJLOP7+lZsyYW+F8Xbp0VUrKZbJarad9r768+mrpl6bKUlBgV2RklNdYVFS0Dh066HN5u90uST7XOfVZfn6+6tdvXOrnBQUFZW7jlHffXSWHw6Ft27Zq+/avvb6bm266VXfffa9iY+N04MB+zZgxTQ8+eL8WLODFtEZEQQFqkYYNG3r93KBBI33//cl/+e7f/7O+/vorXXvt7/+Cd7tdXpdbXn31ZX3yyb907NgxSVJxcZHatLnwT9v03sfIkSNVUFCkZ555UkePHlFSUifdddcYNWrUWEePHlHz5i28lm/UqImOHDnsNRYfX9fz33Xq1JF08hflqYLyx9JTHWw2m3755Revsby8XElSRIStzPU/+OA99enTVyEhIZ6x/fv3afz4MfrrXycqNfUGOZ0OLVv2qkaPHq6lS99UWFh4ufOd6XutahERNuXn53mN5eXlymbzfRxOjefn5ykuLt7nOpGRkWfcZnm28UfBwcFKSuqkjIx1WrToRY0de58keS6vSVKjRo31wAOP6Oabr9f+/fvUuHGT8h0A+A0FBahFDh069KefD+q8886TdPIsRK9evfXgg4/4XHft2o/0zjtvavbsNJ1/fktZLBbNmTNT33+/22u5P14GkU6eRRkxYrSGDx+lvLw8zZz5jJ566nHNn/+Szjuvng4c2O+1/IED+ypcOP68T19ee22Jli5d4rms80dut1sNGjTQ0qVv+Vz3/PNbau3af8nlcnn2tXPnt2rYsFGZZ0+2bt2in3/O0nPPPe81/uOPPygsLFw33jhA0slfqgMH3qZXXnlJ//vfj6cVvzM50/fqy5Aht+jIkSOnjZ+6xDNkyHANGTLM57oXXNBSu3bt1JVXXu0Z2717p9elqT9q2jRBoaGh2rVrp7p27SZJOn78uA4dOugpDK1bt9batZ94rbdr17e64IJWZW7j1DK+OBwOHT16uNTPJXld8oOxMEkWqEW++OJzbdr0uVwul774IlMbNmSoT5++kqT+/W/W+vWfat26T+RwOORyubR//z5t3rxJ0slT9cHBwYqJOXm9fuvWLfr449Vl7vOzzz7TTz/9T06nU6GhoQoLC1NQUJAkqU+fvtq06XNlZKyTy+XS99/v1htvLFPfvv3PuM3K/EIZMmS41101f/y/tWv/XWo5kaQePXrKarXq5Zf/oaKiIv344x69+eYy9e9/S5n7Xb78XXXq1OW0M0utWrWVw1GiVas+kNPpVFFRkd5883VFRESoSZMESdLhw4fUrVtHbdu29Yz7+OP3umnT517fqy+vvfZ2qcfh448zSi0nktSv30368MOV2r59mxwOh957723t379f1113vc/lQ0NDdd111+vllxfoyJHDstvz9cILs9Sixfm66KJLJEn9+/dXVtZeffDBe57LM6tXp2vAgFvK3Map+ScZGeu0Z88Pcjgccjgcysj4TB9/vNpzSbO4uFjr138quz1f0skSN33602rVqo2aNGl6xuOLwOAMClCL9OnTV+npH+ixxx5SXFycJk2a4vkl0bp1W82ePU+LFr2omTOfldPpVIMGDTzPkbjuuuv13/9u1+23/0VBQVYlJXXW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FoulunIAAAB4lHoXzwsvvKDY2FhJ8ry9eM6cOYqJifEsk5OTU83xAABAbeSzoHTs2FGFhYU6dOiQ11hBQYEKCgq8lk1KSqrehAAAoNbxWVBee+01f+cAAADwYJIsAAAwnFLnoPz888/6/PPP5XA41L17dyUkJGjHjh1auHCh9u7dq/r16+vWW29Vr169/JkXAADUAj7PoHzxxRe6/vrr9fTTT2v27Nnq27evVq9erdtvv10HDhzQBRdcoF9++UXjxo3T6tWr/Z0ZAADUcD4Lyty5c5WcnKyvvvpKW7du1dChQ/Xggw/qhhtu0PLlyzV79mytWLFC/fr108svv+zvzAAAoIbzWVC+//57DR48WKGhoZKk4cOHq7i4WNdee63Xcn369NHevXurPSQAAKhdfBaU/Px8r+edREdHS5KioqK8louMjDzttmMAAICzxZNkAQCA4ZR6F8+jjz4qm83mNfbwww8rIiLC87Pdbq++ZAAAoNbyWVD69et32ljTpk19bqBNmzZVmwgAANR6PgvKtGnT/J0DAADAo9RLPH/kcrl0/PhxSdI555wjq5UH0AIAgOpzxoLy8ccfa9myZdq+fbuKi4slSaGhobrkkks0ZMgQniILAACqhc+C4na79dBDD2n58uVq0aKFBg0apAYNGkiSDh06pH//+98aN26c+vXrp6efftqvgQEAQM3ns6C8+eabSk9P13PPPae+ffue9vkDDzygVatW6eGHH9bFF1+sv/zlL9UeFIC3XHuR0jOzlJ1XqLiocKWmJCjaFhboWABQJXwWlLfeekt33HGHz3JyyvXXX689e/borbfeoqAAfpZrL9L0N7bpwLHfb/XflZWjSQPbU1IA1Ag+Z7v+9NNP6tq1a5krp6Sk6H//+1+VhwJwZumZWV7lRJIOHLMrfVNWgBIBQNXyWVDCw8OVm5tb5sp5eXkKDw+v8E5nzpyp66+/XomJierWrZsmTJigw4cPey1TXFysWbNmqWfPnrr00kvVs2dPrVixosL7Amqi7LxC3+O5vscBwGx8FpTExET985//POOKbrdbb7zxhpKSkiq+U6tVzz77rDZv3qw1a9ZIkkaPHu21zLhx4/Ttt9/q1Vdf1ddff613331Xl1xySYX3BdREcVG+/2EQF13xfzAAgBH5LChjxozRli1bNGLECO3YseO0z//73//qrrvu0pYt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" class="pd_save"></center>
                        
                    
                
        </div>
    



```python
#using matplotlib in Pixie Dust to view the percent of the population that is white vs SNAP participation rates in a histogram
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Histogram: Percent of the Population that is White vs SNAP Participation Rates
        </div>
    
        <div id="chartFigure6b8dfb0c" class="pd_save" style="overflow-x:auto">
            
                    
                            <center><img style="max-width:initial !important" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAy4AAALFCAYAAAAlR7deAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAOwwAADsMBx2+oZAAAIABJREFUeJzs3XuYlHXd+PHP7AAqIIsockjQjBSQgyykIuABQstMASM1D2nIwTxLZCiaiYc07fExNDzwWKaiUQL2Sy/xmJLIQbySVEwNErWIABcRcGF3fn/wsA8rqwuyO/PFfb2uq6tr75m5789+Gdb7vXPPkMnlcrkAAABIWFGhBwAAAKiJcAEAAJInXAAAgOQJFwAAIHnCBQAASJ5wAQAAkidcAACA5AkXAAAgeQ0KPQBQt8rKymLBggV5O17Xrl2jUaNGeTsen3+ewwBECBf43FuwYEH06nVNRLTMw9GWxbx5l0XPnj3zcCzqiwULFsQ1vXrl6Rkccdm8eZ7DAAkSLlAvtIyItoUeAj4zz2AAvMcFKJjTTjstunTpEiUlJdGrV6/45je/Gb/73e8qb3/99dfj4osvjr59+0ZJSUkMGDAgRo8eHa+++mr8+Mc/jh49ekRJSUn06NEjOnbsGD169KjcduWVV37qsadOnRodO3aM888/v8r2WbNmRceOHavc7/DDD9/i8XPmzImOHTtGRUVFfPDBB3HAAQfEs88+W+U+RxxxRAwcOLDKtieffDK6du0a69ati3fffTc6duwYS5Ys2WL/HTt2jFmzZkVEVLnf7bffvsX33b179ygpKYmSkpIYMWJE5eMPPPDAyu2bHrNs2bLK7/OMM86Igw8++BNn+Oc//xmjRo2KkpKS6N27d4wfPz42bNjwqetaH73zzjsxevTo6NevX5SUlMRhhx0WI0eOjP/85z+Vz5OhQ4dWeczbb78dHTt2jPfee6/K9lwuFwMGDIgDDzwwPvjggyq3bXoebPqzPPTQQ2PEiBHx5ptv1jjj1KlTo1OnTpXPh8MPPzwuv/zyLY7x4YcfRo8ePeKII46IXC5X5bZN38umffTt2zfOP//8yu9h2rRplbOVlJREp06dolu3bpXPv0GDBn3qjJvWZPPvb/jw4fHGG29Ue//Ro0dX+XsSEbF48eIqMxxwwAGVP2M2bV+xYkVcdNFFlds3v+3ee++tXIfzzz8/Bg4cGJ06dYqJEyducfxcLhc///nPo2/fvtGjR48444wz4u9//3uNfxbAZydcgIIaPnx4zJ8/P+bOnRvDhw+PcePGxdy5c2P27Nnx7W9/O1q2bBm//e1vY/78+TF9+vTo27dvPPbYY/GTn/wkXnrppcrtmUwmHnnkkcptNYVLRETjxo3j2WefjTlz5lTZnslkPvXrj2/fddddo0uXLlVOoBYtWhTr16+PNWvWxDvvvFO5fdasWVFSUhI777zzp+77k441cuTIyu9x3rx5ERExadKkmD9/fsyfPz/uuOOOysdMnDixcvumx7Rs2bLyex80aFDccMMN1c6Qy+VixIgRsdtuu8XMmTPjoYceirlz58YNN9ywVfPWJyNGjIimTZvGI488EvPnz49p06bF17/+9Srr+u6778ZDDz1U5XHVrfuf/vSn+Ne//hU77bRT/P73v9/i9kwmEw8//HDMnz8/Hnvssdh5551j1KhRWzVnq1atKp8P9957b8yZMyeuvfbaKveZNm1aZLPZWL58eTz55JPVHv/FF1+M+fPnx8MPPxwrV66MsWPHRkTEoEGDKp9n8+fPj9133z2uvfbayufftGnTapxx09/j+fPnx4wZM2KXXXaJc889d4v7rVixImbMmBG77bZbTJ48uXL7PvvsU2WGAQMGxLe+9a0qfwdatGgRmUwmBg8evMXfj1NPPbVyjl69esV1110XnTp1qnbW2267LR555JH4zW9+Ey+88EJ07NgxzjrrrCgrK6vx+wQ+G+ECJCGTycRxxx0XzZs3j1deeSV+/OMfx9e//vUYO3ZstG278SKhpk2bxuDBg+Oiiy6qdh8f/w1xTZo1axZnnnlmXHPNNdv82I/r06dP/PnPf678+vnnn4/evXvHIYccEs8//3zl9j//+c/Rp0+f7TrWx33S7J/2PXXv3j0GDRoUHTp0qPb2uXPnxqJFi+JHP/pRNG7cONq0aRMXXHBBTJkyxYnZZt5///34+9//HieddFLsuuuuERHRokWLGDRoUOy+++4RsfG5fcEFF8TPf/7zWLt27afub/LkydGvX78YMmRIPPDAA59631133TWGDBkS7777bpSWlm7T3O3atYv+/fvHK6+8UmX7Aw88EIMGDYojjzwy7r///mofu+l51aJFi/ja1772qR+c8Fn+Xm16TNOmTeO4446Lt99+O1avXl3lPlOmTIlmzZrFFVdcEU899VT8+9//3ubjfJrGjRvH6aefHr169YqGDRtWe58HHnggRo4cGV/84hdjp512iosvvjhWr14dTz/9dK3OAvwf4QIkoby8PKZNmxarVq2KLl26xOLFi+P444+v8+OOHDkyVq5cGb/97W+36XEfPyHr06dPvPHGG/Gf//wnIjYGyqGHHhqHHHJIzJw5MyI2Xnq1aNGiOPTQQ2tn+Dq0cOHCaNeuXRQXF1du69q1a6xduzYWL15cuMES07x589h///3jiiuuiIceeihef/31ak/WTzjhhNhzzz3jtttu+8R9LVmyJJ577rk48cQTY+jQobF48eIq0ftxK1eujN/97nfRvn37Kn9OW2PRokXx1FNPRffu3Su3zZkzJ958880YOnRoDB06NGbNmhX/+Mc/tnjspu9v6dKl8cgjj3xi/G6vlStXxtSpU6NVq1bRtGnTKsd/8MEHY/DgwXHUUUdF8+bN48EHH6yTGT7J8uXLY9myZdG1a9fKbY0aNYr99tsvXn311bzOAvWJN+cDBTVp0qS47777IpvNRtu2beO6666LoqKiyGQy0apVqzo//s477xyjR4+O66+/Pr7xjW9Ue5+lS5fGQQcdVGXb+vXrq3x94IEHRpMmTeL555+Pb3zjGzFnzpy44oorory8PG688caI2BgzzZs3jy5dulQ+LpfLxeDBg6OoqKjKtq29hOzTnHPOOdGgQYPKfe6xxx7x6KOPbtVjV69eHc2aNauybdPJ8cd/+13f3XPPPXHPPffEAw88EK+//nrsvPPOMWTIkLj44osr71NUVBSXXnppfO9734tvf/vb1e7nwQcfjJYtW8aRRx4ZERFf+cpXYvLkyVVCd9PzJZvNxi677BJdunSJX/7yl1s156bncSaTieLi4ujXr1+VGSdPnhzdu3eP/fffP/bbb79o27ZtTJ48OX70ox9VOX7v3r1j/fr1sXbt2ujcuXPcfPPN27RenyaXy8Vxxx0XFRUVsWbNmth77723iL1nnnkm/vnPf8a3v/3tyGazccIJJ8SUKVPinHPOqfL3qCbTpk2Lxx57rPK4mUwmJk6cGCUlJTU+dvXq1ZHJZCpfZdukWbNm8eGHH271DMC2ES5AQQ0bNiwuuOCCKtv+8Y9/RC6Xi6VLl8a+++5b5zMcf/zxMXny5PjFL34RRxxxxBa3t2rVKp555pkq2+bMmRPf/e53K7/OZrNx0EEHxfPPPx977bVX7LHHHtG6deuI2HjCv2DBgpg1a1b07t27yn4ymUxMnTo12rVrV2X75h8Q8Fnddtttccghh3ymxzZt2jRWrVpVZdumy5E2/+03G/98zzvvvDjvvPNi/fr18eyzz8Yll1wSTZo0qRK8vXr1iq9+9atxww03xJgxY6q8MlNWVhYPPfRQnHjiiZXbTjjhhBg3blwsXbq0MuI/6fmyNap7Hm+yfPnyePzxxyvfG5bJZOKEE06IX//613HxxRdX/rs2mUwmZs+eHZlMJl566aU4//zzY8mSJdG+ffttnqc6mUwm/vCHP0SbNm3izTffjFGjRsXf/va36Ny5c+V9Jk+eHF/5ylcqj/mtb30r7rjjjnj88cfj6KOP3upjDRo0KMaPH/+Z5mzatGnkcrktPtxg1apV8eUvf/kz7ROomXCBemHZDnWcvffeO/bZZ5+YPn36Fif6deXSSy+NU045ZbtOwPr06RMTJ06ML3zhC1V+S967d+947rnnYtasWZ/4/py6sD3v2+nUqVO88847UVpaWvlKy8svvxy77LJL7LPPPrU04dbbUZ7BDRs2jAEDBsShhx4ar7766hav1I0ZMyaOOeaY6NevX5VX1R599NFYsWJF3H///TFlypSIiKioqIjy8vJ48MEHt/j0u9r24IMPRnl5edx0002Vr6CsX78+Vq1aFX/4wx/ihBNOqLzvplcnevToEeeff35ceumlMWPGjNhpp51qZZZNz9sOHTrEFVdcERdddFH069cvdt9991iyZEnMnDkzdt555+jbt2/lY4qKiuL+++/fpnDZHrvvvnu0bNkyFixYUPlLhrKysnj99dfjtNNOy8sMUB8JF/ic69q1a8ybd1lej1cbrrrqqspPtTrttNOibdu2sXr16njiiSdi8eLFceGFF1a5//a+ub5bt27xjW98IyZMmPCZ99G3b98YP358TJkyJa644orK7Yccckhce+21sXLlyionWxFbP/f2fn/V7W/9+vXx0UcfRS6Xi7KysigrK4sGDRpEUVFR9OrVK/bdd9/46U9/GuPGjYv3338/brnllhg6dGje/1X5rl27xmX/+wlq+Tre1lq1alXceeedceyxx8YXv/jFaNCgQcyePTtmz54dZ599dkRU/bNr06ZNfO9734v/+q//qrKfyZMnxxFHHBHXXHNNle133XVX5WVQH99XbamoqIgpU6bESSedtMUneI0fPz4mT55cJVw2N2TI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lSujuLg4OnToUCcnIKWlpVFRURHTp0+P22+/PTp16hRPPvlkXHTRRXHfffdF9+7dt2o/xcWNa302Pp01z78dcc2Li3cp9AgAUO8VF+8SLVo0KfQY22SrwiUiIpPJRIcOHepyloiIaNJk4wIOGTIkDjjggIiIGDhwYBx88MHxxBNPbHW4lJauifLyXM13ZLtls5koLm5szfNoR17z0tK1hR4BAOq90tK1sWLFh3k51qbzlu1VY7hcfvnlNe5k/Pjx2z3IJk2bNo327dtv937Ky3NRXl5RCxNRs42vvFnzfNpx17yiYseaFwA+jyoqKvJ4DlE7V2nVGC5/+9vftthWWloa77zzTjRv3jz22muvWhlkc6eeemrccccdccwxx8T+++8fTz31VMybNy8uvPDCWj8WAACQvhrD5cEHH6x2+5IlS+LCCy+MkSNH1vpQp59+eqxbty5GjRoVH3zwQey9995x8803R9euXWv9WAAAQPq2+j0uH9euXbsYOXJkXH/99XHkkUfW5kwRsfFjmEeMGFHr+wUAAHY823XBWUVFRSxbtqy2ZgEAAKhWja+4zJ07d4tt69evj0WLFsUdd9wRPXv2rJPBAAAANqkxXE477bTIZDKRy1X9yNWioqI44ogj4sorr6yr2QAAACJiK8JlxowZW2xr1KhRtGzZMrLZbJ0MBQAAsLkaw6U2/k0VAACA7VFjuMyfP3+bdlhSUvKZhwEAAKhOjeHyne98JzKZTOXXuVxui683yWQy8dprr9XyiAAAQH1XY7hMmjQpxo0bF1/96lejf//+sdtuu8XKlSvjySefjCeffDKuvvrqaNOmTT5mBQAA6qmtCpeTTjopRo4cWWV77969Y4899og777wzfvWrX9XVfAAAADX/A5QvvvhidO7cudrbOnfuHC+99FKtDwUAALC5GsOlTZs28cADD0RFRUWV7RUVFTF58mSXiQEAAHWuxkvFxo4dG+eee270798/DjvssMr3uDz77LOxfPnymDBhQj7mBAAA6rEaw+Xwww+PRx99NH71q1/FK6+8EsuXL4/dd989Bg4cGKeffnq0a9cuH3MCAAD1WI3hEhGx1157xbhx4+p6FgAAgGptVbhERPzpT3+Kv/71r7F06dIYNWpUtG3bNubOnRvt27ePVq1a1eWMAABAPVdjuCxdujTOPvvs+Nvf/hb77LNPvPXW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" class="pd_save"></center>
                        
                    
                
        </div>
    



```python
#using bokeh in Pixie Dust to view the trends in obesity, diabetes, food insecurity and the percent of the population that is black in a line graph
display(df_focusedvalues)
```


```python
#using matplotlib in Pixie Dust to view childhood obesity vs reduced school lunches in a scatterplot
display(df_focusedvalues)
```


<style type="text/css">.pd_warning{display:none;}</style><div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em></div>
        <div class="pd_save is-viewer-good" style="padding-right:10px;text-align: center;line-height:initial !important;font-size: xx-large;font-weight: 500;color: coral;">
            Childhood Obesity vs Reduced Lunches
        </div>
    
        <div id="chartFigureccbca9ab" class="pd_save" style="overflow-x:auto">
            
                    
                            <center><img style="max-width:initial !important" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAysAAALcCAYAAADnty9JAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAOwwAADsMBx2+oZAAAIABJREFUeJzs3Xl8XXWdN/BPmpSlpXThQmkbqgiIQURRpCwKMj4MQ+VxRIqCbFagjMMoiQoqOAqKZQQZg09BFrEt0AHUDhQQcZBNC4iDIMMMAQWnSKy0abELW5fkPn/QVkpv6EKSe9r7fr9e/JFf7jn3k3xR+uk5v3PryuVyOQAAAAXTr9oBAAAAKlFWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQmqodgCAWtHZ2Zk//OEP2WabbTJs2LBefa/jjjsuDz/8cDbbbLP069cvI0aMyAknnJBx48YlSZ544olcdtll+fWvf50XX3wxQ4cOzbve9a6ceOKJuf7663PTTTelrq4u5XI5L730UrbccsskSV1dXT784Q/n7LPPft33f+GFFzJp0qTcfvvtmTdvXrbaaqvss88+Oe2007LDDjuset3f/M3fZN68eWloeOU/R8OGDcuHP/zhfPazn634syRJuVxOXV1dvvnNb+bQQw9NuVzOZZddlhtvvDEdHR1paGjI6NGj86lPfSqHHnpoxfdZeY7Jkyfnne9851p/n939vk466aQ0NTXlhhtuSGtra+65557Vjvv1r3+d448/Po899lj69eu3xusmTZqU++67L//2b/+2xnv+6U9/ygc/+MFsueWW6dev36o5jhkzJuPHj8/IkSNXvfbqq6/OzTffnN/97ncZMmRI7r777tXONWfOnJxzzjl5/PHHM3v27Jx77rmr/l0AKDJlBaAPTJ9+a775zR/liSfqM3To8hx00Ha58spzV/0BvDecfPLJOe2001Iul3PzzTfnjDPOyJve9KZ0dXVlwoQJOeqoo/LDH/4wI0eOzPPPP5/bb789P/vZz3LOOefknHPOSZL88Y9/zCGHHJJbb701I0aMWKf3femll3LMMcdks802y6RJk7Lrrrtmzpw5ueiiizJu3Lhcd9112XHHHVe9/mtf+1qOOOKIJMnvf//7nHDCCdl+++3zsY99bI2fpZIrrrgiN9xwQyZNmpRddtklS5YsyaOPPpqlS5eu9rpXv8/6eOCBB7r9fd12221pampK8kqRq+S162v7+rXfu+mmm1YVvN///ve5/PLL83//7//N1Vdfnd122y1JMnz48Jx88sl56qmnct11161xnn79+uV973tfTj755Hzuc59b9x8eoMqUFYBeNm/evHzuc9fmj3/cOUny4ovJNde8mFLpgnznO2f1+vuvvBoyceLE/M///E+uu+66HHroofnyl7+86jVbbbVVDj/88G7PUS6X1/n9pk6dmtmzZ+f222/P4MGDkyTbb799zjvvvHziE5/IxIkTc8UVV1Q89y677JJ3v/vdeeKJJ9b5/R566KEccMAB2WWXXZIkm2++efbaa6839DO82te+9rX1/n31ll122SUXXHBBjjvuuJx33nm5+uqrkyR/+7d/myR5/vnnKx637bbb5hOf+ESS1y9HAEVjzwpAL7vkkmn54x9Hv2Z1QGbOnNUn79/Z2Zkbb7wxixYtyu67755Zs2bl7//+73vt/e6+++584AMfWFVUXu3www/P/fffv8ZVj5Xa2try0EMPVSwb3RkzZkxuvPHGXHLJJfn1r3/d7R/YN8TTTz/9hn5fG1qQ1uawww7LQw891O3vEWBT4coKQC/r7OxMsubfZvfWH2RXuvLKKzNt2rTU19dn5MiROe+889KvX7/U1dVl+PDhvfa+f/nLX/Le97634veGDx+ezs7OLFy4MNtuu22S5Jvf/GbOP//8LF26NEuWLMkhhxyy6krBa3+W5K/7TX784x9n9OjRGT9+fIYPH55bbrkl11xzTRYuXJi99947X/nKV7LTTjutOsfK93n1Oe69997079+/259l/vz56/z7mjNnTvbee+/V1pYtW7bW4zbEiBEj0tXVtdrvEWBTpKwA9LJTTz02P/jBF9LevtOrVl/OPvvs0O0xPeHEE09cY5/H008/nXK5nDlz5uQtb3lLr7zv0KFDM2fOnIrfe/bZZ1NfX7/aVZevfOUrq/aSzJkzJ5///Odzxhln5MILL1z1mko/y6uNHTs2Y8eOTZI89dRT+frXv55TTjklP//5zyu+z7raZptt1vn3NXz48DU2tv/617/OCSecsF7vuS5mz56dfv36Vbx6BbApcRsYQC/bbrvt8i//8tHsvvv/pqGhPdttNysf+9iSXHjhl/o8y5ve9Ka8+c1vzowZM3rtPT7wgQ/knnvuyaJFi9b43owZM7Lvvvuu9mCBV19hGj58eA499NA1/tC/Pnbaaad88pOfzJ/+9KfVMmzIlay++H1tiFtuuSXvfve7e/UBDQBFoKwA9IFjjvlIHnroivzmN5/JI4+cl+uv/9dsvvnmVcny9a9/Pbfddlu+9a1vZfbs2Ule2Zh94403prW1dY3Xr+8f8o8//vhsv/32mTBhQh5//PF0dXXlz3/+c84888z8/ve/z5e+1H1Jmzt3bm677bZVT7laF5MnT84999yThQsXJnnlqsO//du/ZZdddsnWW2+9XtkrWd/f1/oql8tZunTpav+8cuvg6r/7crm86vfX1ta22ob/zs7OLF26dNVtZyvP82orb7N79euXL1/+hvMD9Ca3gQH0kf79+2ePPfbok/d6vSc+7b333vnhD3+YSy+9NEceeWReeumlDB06NHvuuWdOOumk9TpXJQMGDMi0adNy8cUX59RTT838+fMzYMCA7Lvvvqv2mbzaN77xjUycODHlcjkDBw7MmDFjcsYZZ6z2mu9///uZOnVqkr/uNzn11FNz4oknZquttspll12WP/zhD1m6dGm23nrrjBkzZtXjl1/7Pq8+x8rPank96/v7Wl8PP/zwqs96WZnrH/7hHzJu3LhVT3Jb+Tkr22+/fcaMGZObbropo0aNWnWO733ve5k0adKqWe2xxx6pq6vLHXfcserzWFauJcnZZ5+ds88+Ox/5yEdy3nnnveGfAaC31JV7e4cnAADABnAbGAAAUEhuAwNgvVx22WW59NJLV7s9bOXtS6effnqOPvroKqZbf4cddtiqvSgrrfx5fvGLX2SrrbaqUjIA3AYGAAAUktvAAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQlJWAACAQqpKWbn11ltzzDHH5D3veU+amprS1dW12vcff/zxHHvssdlzzz1zwAEHZNKkSdWICQAAVFFVysrgwYNzzDHH5Mwzz1zjey+88EJOOumkvOc978kDDzyQ73//+/nRj36UqVOnViEpAABQLVUpK/vvv3/Gjh2bHXbYYY3v/cd//EfK5XJOO+20bLbZZnnrW9+aE088MdOmTatCUgAAoFoKt2fl8ccfT1NTU/r1+2u0d7zjHXnmmWfywgsvVDEZAADQlwpXVp5//vlsvfXWq62t/Pr555+vRiQAAKAKCldWttpqqyxatGi1tZVfb7XVVut8nnK53KO5AACAvtVQ7QCv1dTUlFtuuSVdXV2rbgX7r//6r+ywww4ZOHDgOp+nrq4uCxe+mM5OpWVTV19fl8GDB5h3jTDv2mLetcW8a4t515aV815fVSkrXV1dWb58eZYuXZokWbJkSerr69O/f/8cfPDBufDCC/Pd7343n/70pzNr1qxMnjw548ePX+/36ewsp7Oza+0vZCP3Sqk171ph3rXFvGuLedcW864tG3ZDV1VuA5sxY0b22GOPnHzyyUmSPffcM+985zvz4IMPZuDAgbnyyivzn//5nxkzZkxOPPHEHHnkkTnhhBOqERUAAKiSuvImvLnjuede0NRrQH19vwwbNtC8a4R51xbzri3mXVvMu7asnPf6KtwGewAAgERZAQAACkpZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACklZAQAACqmh2gEAADYVHR0daW2dkvb2xWlsHJSWlvEplUrVjgUbLWUFAKAHdHR0ZNy4s9LWtnOSIUmWZ+bMMzN9+kSFBTaQ28AAAHpAa+uUFUVl5d8FN6Stbae0tk6uZizYqCkrAAA9oL19cda8aaX/inVgQygrAAA9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/ffPyJEj+zQsAABQO153w8nIkSNz5JFH9lUWAACAVXyCPQAAUEhvqKz87Gc/S1NTU09lAQAAWMWVFQAAoJAq7lk544wz1ung2bNn92gYAACAlSqWlZtvvjkjR45c69O+FixY0CuhAAAAKpaVnXbaKbvuumsuvPDC1z34tttuS0tLS68EAwAAalvFPSvvete78vDDD6/14Lq6upTL5R4PBQAAUPHKyoQJE3LQQQet9eADDzwwd9xxR4+HAgAAqFhWRo8endGjR6/14C222CKjRo3q8VAAAAAeXQwAABRSxSsrxx9//Hqd5KqrruqRMAAAACtVLCvDhw9f7etyuZxbbrklBxxwQAYPHtwnwQAAgNpWsaxccMEFq329fPny3HLLLTnttNPy9re/vU+CAQAAtW2d9qzU1dX1dg4AAIDV2GAPAAAUkrICAAAUUsU9K11dXat93dnZuWr9td9Lkn79dB4AAKBnVSwru+22W8V9Kh/72McqnqStra1nUwEAADWvYlmZOHGiTfUAAEBVVSwrH/3oR/s6BwAAwGoqbjZ58cUXc/HFF+fBBx/s9sAHH3wwF198cV5++eVeCwcAANSuimVlypQpmT59enbfffduD9x9993z7//+77nqqqt6LRwAQG/r6OjIWWddkBNO+GrOOuuCzJs3r9qRgBUqlpWf/vSnOf7447PFFlt0e+AWW2yR4447Lj/5yU96LRwAQG/q6OjIuHFn5YoryvnpT4fkiivKOeKIMxUWKIiKZeXpp5/ObrvtttaDd9ttt8yaNaunMwEA9InW1ilpa9s5f93G25C2tp3S2jq5mrGAFSqWlf79+6/TXpQlS5akoaHiHn0AgMJrb1+cNZ831H/FOlBtFcvK2972tvziF79Y68H33HNPdt111x4PBQDQFxobByVZ/prVZSvWgWqrWFY+/vGP57rrrsutt97a7YG33nprrr/++nziE5/otXAAAL2ppWV8mpqeTLJsxcqyNDU9lebm8dWMBaxQ8R6uD3/4w3nooYfyuc99LlOnTs3++++fESNGJEmeffbZ3HvvvXnkkUdy1FFH5bDDDuvTwAAAPaVUKmX69IlpbZ2c9vYFaWwclObmiSmVStWOBiSpK5fL5e6+efvtt+eqq67KI488kqVLlyZ5ZT/Lu971rpxwwgn5P//n//RZ0A3x3HMvpLOzq9ox6GX19f0ybNhA864R5l1bzLu2mHdtMe/asnLe6+t1d8cffPDBOfjgg7N8+fIsWLAgSTJkyBCb6gEAgF63Tq2joaHB5VAAAKBPVSwrF1100Xqd5LTTTuuRMCsddthhmT179qqvu7q68vLLL2fSpEmFv/UMAADoGRXLyowZM9Z6YLlczrPPPpuk58vKLbfcstrXV199dS655JIccMABPfo+AABAcVUsK3feeWe3ByxdujT//u//nh/84Afp169fxo4d22vhVrr22mtz5JFHZrPNNuv19wIAAIphnXfKP//887n22mtz1VVX5fnnn8/hhx+eT33qU2lsbOzNfLn//vvz9NNP5+Mf/3ivvg8AAFAsay0r8+fPz9SpU3Pttdemrq4uRx99dE444YQMGzasL/Ll2muvzfvf//6MGjVqvY+tr69LN597ySbklTmbd60w79pi3rXFvGuLedeWlfNeX91+zsozzzyTK6+8MjfccEO23nrrfPKTn8xRRx2VgQPX//nIG2ru3Ln5m7/5G/tVAACgBlW8svL5z38+P/vZz9LY2JivfOUr+chHPpL+/fv3dbZcf/31GTFixAYXlYULX0xnZ7efeckmor6+LoMHDzDvGmHetcW8a4t51xbzri0r572+KpaVn/zkJ0mSF154IRdffHEuvvji1z3J3Xffvd5vvDadnZ350Y9+lBNOOOENnKPsE1FrwiuXjs27Vph3bTHv2mLetcW8a8uG3epXsaz80z/90xuK0hPuuOOOLFy4MEcccUS1owAAAFXQ7Z6VTcFzz72gqdeA+vp+GTZsoHnXCPOuLeZdW8y7tph3bVk57/Xl0QsAAEAhVbwN7MADD0xdXeXHi9XX12fYsGHZa6+9ctxxx2XkyJG9GhAAAKhNFcvKuHHjui0rnZ2d6ejoyG233ZYf//jHmTZtWt761rf2akgAAKD2bPCelaVLl2b8+PEZNGhQLr300p7O1SPcA1kb3PNaW8y7tph3bTHv2tHR0ZGLLpqajo4Xs+22A9Lc/MmUSqVqx6IX9fmelc022yzHHntsfvOb32zoKQAAqDEdHR0ZN+6sXH55V264YatcfnlXjjjizMybN6/a0SigN7TBftCgQVmyZElPZQEAYBPX2jolbW0756+7ERrS1rZTWlsnVzMWBfWGysp9992XN7/5zT0UBQCATV17++KsuW26/4p1WF3FDfbPPPNMtwd0dnZm3rx5ueeeezJ16tScffbZvZUNAIBNTGPjoCTLs/ofQ5etWIfVVSwrBx98cLdPA0uScrmcbbbZJl/60pdy5JFH9lo4AAA2LS0t4zNz5plpa9spSf8ky9LU9FSamydWOxoFVLGsXHXVVd0eUF9fn6FDh2bHHXd83UIDAACvVSqVMn36xFx00dTMnft8tttuYE47baKngVHRBj+6OElmzZqVu+66K+N9tagmAAAgAElEQVTHj+/JTD3Gow9rg0dd1hbzri3mXVvMu7aYd23p80cXJ8kTTzyR888//42cAgAAoKI3VFYAAAB6i7ICAAAUkrICAAAUUsWngd1///3rdPDvfve7Hg0DAACwUsWyMn78+NTV1WVdHhTm8cUAAEBvqFhW7rjjjr7OAQAAsJqKZWXUqFEbdLJyuZwzzzwzn/nMZzJy5Mg3FAwAAKhtPbrBvqurKzfeeGP+8pe/9ORpAQCAGtTjTwNbl30uAAAAa+PRxQAAQCEpKwAAQCEpKwAAQCH1eFnxuSsAAEBPsMEeAAAopIqfs/Jaf/rTnzJ//vwkSalU6vYzVOrr6/P444/3XDoAAKBmdVtWli1blu9973v54Q9/uKqorFQqlXLUUUfllFNOSUPDOvUdAACA9VKxaXR1deWkk07Kgw8+mL/7u7/Lfvvtl+HDh6dcLmfOnDm59957c8kll+Shhx7K97//fftUAACAHlexrNx444156KGHcuWVV2afffZZ4/vjxo3L/fffnwkTJmTGjBn5yEc+0utBAQCA2lJxg/1PfvKTHHHEERWLykr77rtvjjjiiNx88829Fg4AAKhdFcvKE088kfe9731rPfh973tfnnjiiR4PBQAAULGsLFiwINtss81aD95mm22yYMGCHg8FAABQsawsX7489fX1az+4X790dnb2eCgAAIBunzv8//7f/8vQoUNf9+C//OUvPR4IAAAg6aasvPe9783LL7+cP//5z2s9wV577dXjoQAAACqWlauvvrqvcwAAAKym4p4VAACAaqtYVg4//PD8/ve/X/V1uVzO2WefnWeffXa11/33f/939t57795NCAAA1KSKZaWtrS0vvfTSqq+7urpy/fXXZ/78+au9rrOzM4sXL+7dhAAAQE1a59vAyuVyb+YAAABYjT0rAABAIXX7OSvAKzo6OtLaOiXt7YvT2DgoLS3jUyqV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M3r06L7MAgAAsIoN9gAAQCEpKwAAQCFVLCttbW1ZunRpnn766W4PnDVrVh588MFeCwYAANS2imVlxowZmTBhQvr16/7CS319fU455ZTccsstvRYOAACoXRXbyI9//OMcffTR2WGHHbo9cIcddsgnPvGJXH/99b0WDgAAqF0Vy8pjjz2Wfffdd60H77PPPnnsscd6PBQAAEDFstLZ2ZnNNttsrQc3NDRk+fLlPR4KAACgYlkZPXp0fvvb36714EceecRnsQAAAL2iYlkZO3ZsfvCDH+SPf/xjtwc+/fTTmTJlSj70oQ/1WjgAAKB2VfwE+0996lO54447cvjhh+eYY47JfvvtlxEjRiRJnn322dx3332ZNm1a3vKWt2T8+PF9GhgAAKgNFcvK5ptvnquuuir/+q//mquvvjqXX3556urqkiTlcjlbbrllxo0bl5aWlmy++eZ9GhgAAKgNFctKkgwYMCCnnnpqDjnkkNTV1WXOnDmpq6vLdtttl9133z1bbLFFX+YEAABqTMWy8vzzz+fMM8/M7bffvmrt7W9/e7797W/nzW9+c19lAwAAaljFDfatra2ZOXNmmpubc9lll+WrX/1q5s+fny9/+ct9nQ8AAKhRFa+s3H333Wlpaclxxx23au2tb31rjj322CxcuDCDBw/us4AAAEBtqnhlZfbs2dl9991XW3vHO96RcrmcP//5z30SDAAAqG0Vy0pXV1fq6+tXW1v5dVdXV++nAgAAal63TwP76le/moEDB66xftZZZ2XAgAGrrU2bNq3nkwEAADWtYlk5/PDDK7549OjRvRoGAABgpYpl5bzzzuvrHAAAAKupuGcFAACg2pQVAACgkJQVAACgkJQVAACgkLp9dDEA9KSOjo60tk5Je/viNDYOSkvL+JRKpWrHAqDAlBUAel1HR0fGjTsrbW07JxmSZHlmzjwz06dPVFgA6JbbwADoda2tU1YUlZV/R9aQtrad0to6uZqxACg4ZQWAXtfevjhrXszvv2IdACpTVgDodY2Ng5Isf83qshXrAFCZsgJAr2tpGZ+mpieTLFuxsixNTU+luXl8NWMBUHA22APQ60qlUqZPn5jW1slpb1+QxsZBaW62uR6A16esANAnSqVSzj339GrHAGAj4jYwAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkBqqHaCSSZMm5ZJLLskWW2yRcrmcurq6HHTQQbnwwgurHQ0AAOgjhSwrSbLnnntm2rRp1Y4BAABUidvAAACAQirslZXHHnss++23X7bYYovsueeeaWlpSWNjY7VjAQAAfaSuXC6Xqx3itZ588skMHDgwI0aMyNy5c3P++efnkUceyU033ZQtt9xync+zcOGL6ews3I9HD6uvr8vgwQPMu0aYd20x79pi3rXFvGvLynmvr0KWlddaunRp9tprr1x66aXZb7/9qh2HdTR37tx885uX55lnFmaHHQbnK185Jdtuu221YwEAsJEo7G1glaxvr9LUq6ejY24OP/ystLXtlGSrJEty++2n5cYbJ6ZU6tnC4m9maot51xbzri3mXVvMu7Zs6JWVQpaVn/70p9lnn30ydOjQzJs3L+eff3623Xbb7Lnnnut1ns7Ocjo7u3opJa/nwgsnrygqK/8Va0hb21ty4YU/yLnnnt7D7/bKcyLMu1aYd20x79pi3rXFvGvLhj3Xq5Bl5aabbso3vvGNvPTSS9l6662z1157ZcqUKRkwYP3bGNXR3r44yZDXrPZPe/uCasQBAGAjVMiy8r3vfa/aEXiDGhsHJVme1f8VW7ZiHQAA1s7nrNArWlrGp6npySTLVqwsS1PTU2luHl/NWAAAbEQKeWWFjV+pVMr06RPT2jo57e0L0tg4KM3NE1MqlaodDQCAjYSyQq8plUq9sJkeAIBa4TYwAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkJQVAACgkBqqHQAopo6OjrS2Tkl7++I0Ng5KS8v4lEqlascCAGqIsgKsoaOjI+PGnZW2tp2TDEmyPDNnnpnp0ycqLABAn3EbGLCG1tYpK4rK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" class="pd_save"></center>
                        
                    
                
        </div>
    


### Let's download our dataframe and work with it on Watson Analytics.

Unfortunately, in DSX we cannot download our dataframe as a csv in one line of code, but we can download it to DSX so that it can be downloaded and used elsewhere as well as for other projects. I demonstrate how to do this in the notebook.

Once you follow along, you can take the new .csv (found under "Data Services" --> "Object Storage" from the top button) and upload it to Watson Analytics. Again, if you do not have an account, you'll want to set one up. Once you are logged in and ready to go, you can upload the data (saved in this repo as df_focusedvalues.csv) to your Watson platform. 


```python
#download dataframe as csv file to use in IBM Watson Analytics
```


```python
#Below is a tutorial on how to do this, but I found it a bit confusing and uncompatable with python 3. I suggest doing what I did below!
#https://medium.com/ibm-data-science-experience/working-with-object-storage-in-data-science-experience-python-edition-c96bc6c6101
```


```python
#First get your credentials by going to the "1001" button again and under your csv file selecting "Insert Credentials". 
#The cell below will be hidden because it has my personal credentials so go ahead and insert your own.
```


```python

# @hidden_cell

```


```python
df_focusedvalues.to_csv('df_focusedvalues.csv',index=False)
```


```python
from io import StringIO  
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
                    print(resp2)
```


```python
put_file(credentials_98,'df_focusedvalues.csv')
```

    <Response [201]>


 Once this is complete, go get your csv file from Data Services, Object Storage! (Find this above! ^)

### Using Watson to visualize our insights.

Once you've set up your account, you can see taht the Watson plaform has three sections: data, discover and display. You uploaded your data to the "data" section, but now you'll want to go to the "discover" section. Under "discover" you can select your dataframe dataset for use. Once you've selected it, the Watson platform will suggest different insights to visualize. You can move forward with its selections or your own, or both. You can take a look at mine here (you'll need an account to view): https://ibm.co/2xAlAkq or see the screen shots attached to this repo. You can also go into the "display" section and create a shareable layout like mine (again you'll need an account): https://ibm.co/2A38Kg6.

You can see that with these visualizations the user can see the impact of food insecurity by state, geographically distributed and used aid such as reduced school lunches, a map of diabetes by state, a predictive model for food insecurity and diabetes (showcasing the factors that, in combination, suggest a likelihood of food insecurity), drivers of adult diabetes, drivers of food insecurity, the relationship with the frequency of farmers market locations, food insecurity and adult obesity, as well as the relationship between farmers markets, the percent of the population that is Asian, food insecurity and poverty rates.

By reviewing our visualizations both in DSX and Watson, we learn that obesity and diabetes almost go hand in hand, along with food insecurity. We can also learn that this seems to be an inequality issue, both in income and race, with Black and Hispanic populations being more heavily impacted by food insecurity and diet-related diseases than those of the White and Asian populations. We can also see that school-aged children who qualify for reduced lunch are more likely obese than not whereas those that have a farm-to-school program are more unlikely to be obese.

Like many data science investigations, this analysis could have a big impact on policy and people's approach to food insecurity in the U.S. What's best is that we can create many projects much like this in a quick time period and share them with others by using Pandas, Pixie Dust as well as Watson's predictive and recommended visualizations.
