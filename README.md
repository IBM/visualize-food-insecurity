
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


<em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>


```python
#using matplotlib in Pixie Dust to view Food Insecurity by state in a bar chart
display(df_focusedvalues)
```


<em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>



```python
#using bokeh in Pixie Dust to view the percent of the population that is black vs the percent of the population that is obese in a line chart
display(df_focusedvalues)
```


em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>
    

```python
#using seaborn in Pixie Dust to view obesity vs diabetes in a scatterplot
display(df_focusedvalues)
```


<em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>

```python
#using matplotlib in Pixie Dust to view the percent of the population that is white vs SNAP participation rates in a histogram
display(df_focusedvalues)
```


<div class="pd_warning"><em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>

```python
#using bokeh in Pixie Dust to view the trends in obesity, diabetes, food insecurity and the percent of the population that is black in a line graph
display(df_focusedvalues)
```


```python
#using matplotlib in Pixie Dust to view childhood obesity vs reduced school lunches in a scatterplot
display(df_focusedvalues)
```


<em>Hey, there's something awesome here! To see it, open this notebook outside GitHub, in a viewer like Jupyter</em>

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
