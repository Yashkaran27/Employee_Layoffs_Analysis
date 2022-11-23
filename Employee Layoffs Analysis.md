```python
# Importing Libraries

Set the limit

import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')


# Importing the dataset

df = pd.read_csv(r'C:\Users\yashp\Desktop\Projects\Employee_Layoffs_dataset\layoffs.csv')
```


```python
# Loading dataset

df.head()
```


```python
df.shape
```




    (1651, 9)




```python
df.isna().sum()
```




    company                  0
    location                 0
    industry                 3
    total_laid_off         476
    percentage_laid_off    546
    date                     0
    stage                    4
    country                  0
    funds_raised           115
    dtype: int64




```python
# Lets fix some issues with the data here

#  droping some columns and fix some NaN values in dataset

df = df.drop(['funds_raised','percentage_laid_off', 'stage'], axis=1)
df.total_laid_off = df.total_laid_off.fillna(1) # lets assume there must be atleast 1 reported layoff in any company in this dataset
df.industry = df.industry.fillna('Unknown')
df.total_laid_off = df.total_laid_off.astype(int) # layoffs should be whole numbers not floats
```


```python
# Now let see the dataset again

df.shape
```




    (1651, 6)




```python
df.isna().sum()
```




    company           0
    location          0
    industry          0
    total_laid_off    0
    date              0
    country           0
    dtype: int64




```python
# Alright! It's time to analyse the data

# Top 5 countries affected by layoffs

df.groupby('country')['total_laid_off'].sum().sort_values(ascending=False).head().plot(ylabel="", figsize=(6,6), kind='pie', stacked=True, colormap='tab10')
```




    <AxesSubplot:>




    
![png](output_7_1.png)
    



```python
# Top 5 industry affected by layoffs

df.groupby('industry')['total_laid_off'].sum().sort_values(ascending=False).head().plot(ylabel="",figsize=(6,6), kind='pie', stacked=True, colormap='Set3')
```




    <AxesSubplot:>




    
![png](output_8_1.png)
    



```python
# Top 5 location affected world wide

df.groupby('location')['total_laid_off'].sum().sort_values(ascending=False).head().plot(ylabel="",figsize=(6,6), kind='pie', stacked=True, colormap='Set2')
```




    <AxesSubplot:>




    
![png](output_9_1.png)
    



```python
# Total layoffs by industry

plt.figure(figsize=(10, 6))
plt.title("Total Layoffs by industry world wide")
plt.ylabel("Number of layoffs reported")
df_industries = df.groupby('industry').sum()['total_laid_off'].sort_values(ascending=False).plot(figsize=(16,8), kind='bar', stacked=True, colormap='tab10')
```


    
![png](output_10_0.png)
    



```python
df = df.set_index('date')
df_2022 = df.loc[:'2022']
df_2021 = df.loc[(df.index > '2021-01-01')&(df.index < '2022-01-01')]
df_2020 = df.loc[(df.index > '2020-01-01')&(df.index < '2021-01-01')]
```


```python
# Top 5 companies that laid off their employees in 2022

df_2022_most_layoffs = df_2022.sort_values(by='total_laid_off', ascending=False)
df_2022_most_layoffs.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>location</th>
      <th>industry</th>
      <th>total_laid_off</th>
      <th>country</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2022-11-09</th>
      <td>Meta</td>
      <td>SF Bay Area</td>
      <td>Consumer</td>
      <td>11000</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2022-11-16</th>
      <td>Amazon</td>
      <td>Seattle</td>
      <td>Retail</td>
      <td>10000</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2022-11-16</th>
      <td>Cisco</td>
      <td>SF Bay Area</td>
      <td>Infrastructure</td>
      <td>4100</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2022-11-04</th>
      <td>Twitter</td>
      <td>SF Bay Area</td>
      <td>Consumer</td>
      <td>3700</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2022-03-08</th>
      <td>Better.com</td>
      <td>New York City</td>
      <td>Real Estate</td>
      <td>3000</td>
      <td>United States</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top 5 companies that laid off their employees in 2021

df_2021_most_layoffs = df_2021.sort_values(by='total_laid_off', ascending=False)
df_2021_most_layoffs.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>location</th>
      <th>industry</th>
      <th>total_laid_off</th>
      <th>country</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-06-01</th>
      <td>Katerra</td>
      <td>SF Bay Area</td>
      <td>Construction</td>
      <td>2434</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2021-11-02</th>
      <td>Zillow</td>
      <td>Seattle</td>
      <td>Real Estate</td>
      <td>2000</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2021-01-21</th>
      <td>Instacart</td>
      <td>SF Bay Area</td>
      <td>Food</td>
      <td>1877</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>WhiteHat Jr</td>
      <td>Mumbai</td>
      <td>Education</td>
      <td>1800</td>
      <td>India</td>
    </tr>
    <tr>
      <th>2021-08-05</th>
      <td>Bytedance</td>
      <td>Shanghai</td>
      <td>Consumer</td>
      <td>1800</td>
      <td>China</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top 5 companies that laid off their employees in 2020

df_2020_most_layoffs = df_2020.sort_values(by='total_laid_off', ascending=False)
df_2020_most_layoffs.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>location</th>
      <th>industry</th>
      <th>total_laid_off</th>
      <th>country</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-07-30</th>
      <td>Booking.com</td>
      <td>Amsterdam</td>
      <td>Travel</td>
      <td>4375</td>
      <td>Netherlands</td>
    </tr>
    <tr>
      <th>2020-05-06</th>
      <td>Uber</td>
      <td>SF Bay Area</td>
      <td>Transportation</td>
      <td>3700</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2020-05-18</th>
      <td>Uber</td>
      <td>SF Bay Area</td>
      <td>Transportation</td>
      <td>3000</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2020-04-13</th>
      <td>Groupon</td>
      <td>Chicago</td>
      <td>Retail</td>
      <td>2800</td>
      <td>United States</td>
    </tr>
    <tr>
      <th>2020-05-05</th>
      <td>Airbnb</td>
      <td>SF Bay Area</td>
      <td>Travel</td>
      <td>1900</td>
      <td>United States</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Layoffs in 2022 by industry

df_2022.groupby('industry')['total_laid_off'].sum().sort_values(ascending=False).head().plot(title ="Layoffs in 2022 by Industry" ,ylabel="No of layoffs", figsize=(8,6), kind='bar', stacked=True, colormap='Set2')
```




    <AxesSubplot:title={'center':'Layoffs in 2022 by Industry'}, xlabel='industry', ylabel='No of layoffs'>




    
![png](output_15_1.png)
    



```python
# Layoffs in 2021 by industry

df_2021.groupby('industry')['total_laid_off'].sum().sort_values(ascending=False).head().plot(title ="Layoffs in 2021 by Industry" ,ylabel="No of layoffs", figsize=(8,6), kind='bar', stacked=True, colormap='Pastel1')
```




    <AxesSubplot:title={'center':'Layoffs in 2021 by Industry'}, xlabel='industry', ylabel='No of layoffs'>




    
![png](output_16_1.png)
    



```python
# Layoffs in 2020 by industry

df_2020.groupby('industry')['total_laid_off'].sum().sort_values(ascending=False).head().plot(title ="Layoffs in 2020 by Industry" ,ylabel="No of layoffs", figsize=(8,6), kind='bar', stacked=True, colormap='Paired')
```




    <AxesSubplot:title={'center':'Layoffs in 2020 by Industry'}, xlabel='industry', ylabel='No of layoffs'>




    
![png](output_17_1.png)
    



```python
df = df.reset_index()
df['date'] = pd.to_datetime(df['date'])
df_industry = df.groupby([ df.industry, df.date.dt.year]).sum()
# df_industry.sort_values(by=['total_laid_off','date'], ascending=False)
```


```python
df_industry = df_industry.reset_index()
```


```python
# Yearly layoffs by Industry

plt.figure(figsize=(10, 8))
plt.xticks(rotation=90)
plt.title("Yearly layoffs by Industry")
sns.set(style="white", palette="tab20", color_codes=True)

sns.barplot(data=df_industry.sort_values(by=['total_laid_off','date'], ascending=False), x="industry", y="total_laid_off", hue="date")
```




    <AxesSubplot:title={'center':'Yearly layoffs by Industry'}, xlabel='industry', ylabel='total_laid_off'>




    
![png](output_20_1.png)
    


# Yearly layoff trend for the whole world shows that

- Layoffs were generally higher in 2020, then they dropped in 2021 and skyrocketed in 2022

- Initially Transportation and Travel industries got affected more as shown by the layoffs in 2020 but they somewhat recovered later

- Retail, Consumer and Food industries face sharp increase in layoffs


```python
# Let's analyze the data specifically for the USA as employee layoffs are highest in the country

df_usa = df[df['country']=="United States"]
df_minus_usa = df[df['country']!="United States"]
```


```python
# isolate data to be plotted
d1 = df_usa.groupby("industry")['total_laid_off'].sum().sort_values(ascending=False).head()
d2 = df_minus_usa.groupby("industry")['total_laid_off'].sum().sort_values(ascending=False).head()

# create subplots
fig, (ax1, ax2) = plt.subplots(1, 2)
# fig.tight_layout(rect=(0,0,1.3,1.2))
fig.tight_layout(rect=(0,0,1.7,0.9))

# set titles for each subplot
ax1.set_title("Layoffs by Industry - USA", fontweight='bold')
ax2.set_title("Layoffs by Industry - Outside of USA", fontweight='bold')

# plot a pie chart for each data series
ax1.pie(x=d1,labels=d1.index, autopct=lambda x: '{:.0f}'.format(x*d1.values.sum()/100))
ax2.pie(x=d2,labels=d2.index, autopct=lambda x: '{:.0f}'.format(x*d2.values.sum()/100))

# insert line between subplots
ax1.plot([1.3, 1.3], [0, 1], color='black', lw=2, transform=ax1.transAxes, clip_on=False)
```




    [<matplotlib.lines.Line2D at 0x272a11de4c0>]




    
![png](output_23_1.png)
    



```python
# isolate data to be plotted
d1 = df_usa.groupby("location")['total_laid_off'].sum().sort_values(ascending=False).head()
d2 = df_minus_usa.groupby("location")['total_laid_off'].sum().sort_values(ascending=False).head()

# create subplots
fig, (ax1, ax2) = plt.subplots(1, 2)
fig.tight_layout(rect=(0,0,1.7,0.9))

# set titles for each subplot
ax1.set_title("Top locations affected by layoffs - USA", fontweight='bold')
ax2.set_title("Top locations affected by layoffs - Outside of USA", fontweight='bold')

# plot a pie chart for each data series
ax1.pie(x=d1,labels=d1.index, autopct=lambda x: '{:.0f}'.format(x*d1.values.sum()/100))
ax2.pie(x=d2,labels=d2.index, autopct=lambda x: '{:.0f}'.format(x*d2.values.sum()/100))

# insert line between subplots
ax1.plot([1.35, 1.35], [0, 1], color='black', lw=2, transform=ax1.transAxes, clip_on=False)
```




    [<matplotlib.lines.Line2D at 0x272a1830610>]




    
![png](output_24_1.png)
    



```python
# Let's look at the yearly layoffs in USA by Industry

df_usa_industry_yearly = df_usa.groupby([ df_usa.industry, df_usa.date.dt.year]).sum()
```


```python
df_usa_industry_yearly = df_usa_industry_yearly.reset_index()
```


```python
plt.figure(figsize=(12, 10))
plt.xticks(rotation=90)
plt.title("Yearly layoffs in USA by Industry")
sns.set(style="white", palette="Accent", color_codes=True)

sns.barplot(data=df_usa_industry_yearly.sort_values(by=['total_laid_off','date'], ascending=False), x="industry", y="total_laid_off", hue="date")
```




    <AxesSubplot:title={'center':'Yearly layoffs in USA by Industry'}, xlabel='industry', ylabel='total_laid_off'>




    
![png](output_27_1.png)
    


# Yearly layoff trend for United States shows that

- Layoffs were generally higher in 2020, then they dropped in 2021 and skyrocketed in 2022.

- Initially Transportation and Travel industries got affected more as shown by the layoffs in 2020 but they somewhat recovered later.

- Consumer, Retail, Real Estate, Healthcare, Food and Fitness industries face sharp increase in layoffs in 2022 compared to 2020.

- Yearly layoff trend for the whole world matches with that of yearly trend in USA in the sense that layoffs were higher in 2020, then they dropped in 2021 and skyrocketed in 2022. This may be because majority of data in the dataset is about USA.

# Final Analysis

- United States suffered most layoffs followed by India, compared to the rest of the world. More than 75% of the employees laid off since 2020 belong to USA.

- Consumer industry in USA is hit hardest with layoffs in 2022 while Food industry is affected most outside USA in 2022.

- Transportation and Food industries are affected by layoffs globally.

- Travel industry faced considerably less layoffs globally in 2022 compared to 2022.
