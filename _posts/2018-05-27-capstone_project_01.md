---
title: week_four - 911 Calls Capstone Project.
bigimg: /img/wk-4.jpeg
comments: true
tags: [Plotly, data-analysis, data-viz, seaborn]
---
This is an analysis of some 911 call data from [Kaggle](https://www.kaggle.com/mchirico/montcoalert) that I took as a progress milestone to cover for the first batch of learnings.

## Data and Setup

```jupyter
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```
### Getting started - Data Access

```jupyter
df = pd.read_csv('911.csv')

df.head(3)
```
<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>lat</th>
      <th>lng</th>
      <th>desc</th>
      <th>zip</th>
      <th>title</th>
      <th>timeStamp</th>
      <th>twp</th>
      <th>addr</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>40.297876</td>
      <td>-75.581294</td>
      <td>REINDEER CT &amp; DEAD END;  NEW HANOVER; Station ...</td>
      <td>19525.0</td>
      <td>EMS: BACK PAINS/INJURY</td>
      <td>2015-12-10 17:40:00</td>
      <td>NEW HANOVER</td>
      <td>REINDEER CT &amp; DEAD END</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>40.258061</td>
      <td>-75.264680</td>
      <td>BRIAR PATH &amp; WHITEMARSH LN;  HATFIELD TOWNSHIP...</td>
      <td>19446.0</td>
      <td>EMS: DIABETIC EMERGENCY</td>
      <td>2015-12-10 17:40:00</td>
      <td>HATFIELD TOWNSHIP</td>
      <td>BRIAR PATH &amp; WHITEMARSH LN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>40.121182</td>
      <td>-75.351975</td>
      <td>HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-St...</td>
      <td>19401.0</td>
      <td>Fire: GAS-ODOR/LEAK</td>
      <td>2015-12-10 17:40:00</td>
      <td>NORRISTOWN</td>
      <td>HAWS AVE</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>

```jupyter
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 99492 entries, 0 to 99491
    Data columns (total 9 columns):
    lat          99492 non-null float64
    lng          99492 non-null float64
    desc         99492 non-null object
    zip          86637 non-null float64
    title        99492 non-null object
    timeStamp    99492 non-null object
    twp          99449 non-null object
    addr         98973 non-null object
    e            99492 non-null int64
    dtypes: float64(3), int64(1), object(5)
    memory usage: 6.8+ MB


### Top 5 zipcodes for 911 calls


```jupyter
df['zip'].value_counts().head(5)
```
    19401.0    6979
    19464.0    6643
    19403.0    4854
    19446.0    4748
    19406.0    3174
    Name: zip, dtype: int64

### Top 5 townships (twp) for 911 calls

```jupyter
df['twp'].value_counts().head(5)
```
    LOWER MERION    8443
    ABINGTON        5977
    NORRISTOWN      5890
    UPPER MERION    5227
    CHELTENHAM      4575
    Name: twp, dtype: int64

### How many unique 'title' codes are there?

```jupyter
len(df['title'].value_counts())
```

    110

### Creating new features

In the titles column there are "Reasons/Departments" specified before the title code.

These are EMS, Fire, and Traffic.

Use *.apply()* with a custom lambda expression to create a new column called "Reason" that contains this string value.

For example, if the title column value is EMS: BACK PAINS/INJURY , the Reason column value would be EMS.

```jupyter
df['Reason'] = df['title'].apply(lambda title: title.split(':')[0])

df['Reason'].head()
```
    0     EMS
    1     EMS
    2    Fire
    3     EMS
    4     EMS
    Name: Reason, dtype: object

### The most common Reason for a 911 call based off of this new column

```jupyter
df['Reason'].value_counts()
```
    EMS        48877
    Traffic    35695
    Fire       14920
    Name: Reason, dtype: int64

### Use Seaborn to Create a countplot of 911 calls by Reason.

```jupyter
sns.countplot(x='Reason', data= df, palette='coolwarm')
```
![png](/notebooks/911_Project/output_15_1.png)


## Focus on time information.

Data type of the objects in the timeStamp column


```jupyter
type(df['timeStamp'][0])
```
    pandas._libs.tslib.Timestamp

### convert the 'timeStamp' column from strings to DateTime objects


```jupyter
df['timeStamp'] = pd.to_datetime(df['timeStamp'])

type(df['timeStamp'][0])
```
    pandas._libs.tslib.Timestamp

```jupyter
df['timeStamp'][5]
```
    Timestamp('2015-12-10 17:40:01')

### Extract time components eg. Month, hour


```jupyter
print(df['timeStamp'][5].month)
print(df['timeStamp'][5].hour)
```

    12
    17


### Create 3 new columns called Hour, Month, and Day_of_Week


```jupyter
df['Month'] = df['timeStamp'].apply(lambda timestamp: timestamp.month)

df['Hour'] = df['timeStamp'].apply(lambda timestamp: timestamp.hour)

df['Day_of_week'] = df['timeStamp'].apply(lambda timestamp: timestamp.weekday())

```
#### Use the .map() with this dictionary to map the actual string names to the day of the week:


```jupyter
dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'}

df['Day_of_week'] = df['timeStamp'].apply(lambda timestamp: timestamp.weekday()).map(dmap)

```
## With Seaborn create:

### Countplot of the Day of Week with the hue based off of the Reason


```jupyter
sns.countplot(data=df, x='Day_of_week',hue='Reason')
plt.legend(loc='center left', bbox_to_anchor=(1.0, 0.5))
```
![png](/notebooks/911_Project/output_28_1.png)

### Now Countplot for Month:


```jupyter
sns.countplot(data=df, x='Month',hue='Reason')
plt.legend(loc='center left', bbox_to_anchor=(1.0, 0.5))
```

![png](/notebooks/911_Project/output_30_1.png)


The dataset is missing some Months,

fill in this information by possibly a simple line plot that fills in the missing months.

Now create a gropuby object called byMonth, where you group the DataFrame by the month column and use the count() method for aggregation.


```jupyter
byMonth = df.groupby('Month').count()
```
#### A simple plot of the DataFrame indicating the count of calls per month.

```jupyter
byMonth['twp'].plot()
```

![png](/notebooks/911_Project/output_34_1.png)


### Create a linear fit on the number of calls per month.

>   Keep in mind you may need to reset the index to a column.


```jupyter
sns.lmplot(data=byMonth.reset_index(), x='Month',y='twp')
```
![png](/notebooks/911_Project/output_36_1.png)


Create a new column called 'Date' that contains the date from the timeStamp column.

You'll need to use apply along with the **.date()** method.


```jupyter
df['Date'] = df['timeStamp'].apply(lambda timestamp: timestamp.date())
```
*Group by* this Date column with the count() aggregate

and create a plot of counts of 911 calls.


```jupyter
df.groupby('Date').count()['twp'].plot()
plt.tight_layout()
```
![png](/notebooks/911_Project/output_40_0.png)

## Create plots representing a Reason for the 911 call

#### Plot representing EMS calls

```jupyter
df[df['Reason']=='EMS'].groupby('Date').count()['twp'].plot()
plt.tight_layout()
plt.title('EMS')
```

![png](/notebooks/911_Project/output_43_1.png)


#### Plot representing Fire calls
```jupyter
df[df['Reason']=='Fire'].groupby('Date').count()['twp'].plot()
plt.tight_layout()
plt.title('Fire')
```
![png](/notebooks/911_Project/output_44_1.png)


#### Plot representing Traffic calls
```jupyter
df[df['Reason']=='Traffic'].groupby('Date').count()['twp'].plot()
plt.tight_layout()
plt.title('Traffic')
```
![png](/notebooks/911_Project/output_45_1.png)


## Creating Heatmaps with Seaborn

Restructure the dataframe so that the columns become the Hours,
Index becomes the Day of the Week.

>   There are lots of ways to do this, try to combine groupby with an unstack method.


```jupyter
dayHour = df.groupby(by=['Day_of_week','Hour']).count()['Reason'].unstack()
dayHour.head()
```
<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Hour</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>14</th>
      <th>15</th>
      <th>16</th>
      <th>17</th>
      <th>18</th>
      <th>19</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
    </tr>
    <tr>
      <th>Day_of_week</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Fri</th>
      <td>275</td>
      <td>235</td>
      <td>191</td>
      <td>175</td>
      <td>201</td>
      <td>194</td>
      <td>372</td>
      <td>598</td>
      <td>742</td>
      <td>752</td>
      <td>...</td>
      <td>932</td>
      <td>980</td>
      <td>1039</td>
      <td>980</td>
      <td>820</td>
      <td>696</td>
      <td>667</td>
      <td>559</td>
      <td>514</td>
      <td>474</td>
    </tr>
    <tr>
      <th>Mon</th>
      <td>282</td>
      <td>221</td>
      <td>201</td>
      <td>194</td>
      <td>204</td>
      <td>267</td>
      <td>397</td>
      <td>653</td>
      <td>819</td>
      <td>786</td>
      <td>...</td>
      <td>869</td>
      <td>913</td>
      <td>989</td>
      <td>997</td>
      <td>885</td>
      <td>746</td>
      <td>613</td>
      <td>497</td>
      <td>472</td>
      <td>325</td>
    </tr>
    <tr>
      <th>Sat</th>
      <td>375</td>
      <td>301</td>
      <td>263</td>
      <td>260</td>
      <td>224</td>
      <td>231</td>
      <td>257</td>
      <td>391</td>
      <td>459</td>
      <td>640</td>
      <td>...</td>
      <td>789</td>
      <td>796</td>
      <td>848</td>
      <td>757</td>
      <td>778</td>
      <td>696</td>
      <td>628</td>
      <td>572</td>
      <td>506</td>
      <td>467</td>
    </tr>
    <tr>
      <th>Sun</th>
      <td>383</td>
      <td>306</td>
      <td>286</td>
      <td>268</td>
      <td>242</td>
      <td>240</td>
      <td>300</td>
      <td>402</td>
      <td>483</td>
      <td>620</td>
      <td>...</td>
      <td>684</td>
      <td>691</td>
      <td>663</td>
      <td>714</td>
      <td>670</td>
      <td>655</td>
      <td>537</td>
      <td>461</td>
      <td>415</td>
      <td>330</td>
    </tr>
    <tr>
      <th>Thu</th>
      <td>278</td>
      <td>202</td>
      <td>233</td>
      <td>159</td>
      <td>182</td>
      <td>203</td>
      <td>362</td>
      <td>570</td>
      <td>777</td>
      <td>828</td>
      <td>...</td>
      <td>876</td>
      <td>969</td>
      <td>935</td>
      <td>1013</td>
      <td>810</td>
      <td>698</td>
      <td>617</td>
      <td>553</td>
      <td>424</td>
      <td>354</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>

### Create a HeatMap using this new DataFrame


```jupyter
plt.figure(figsize=(12,6))
sns.heatmap(dayHour, cmap='coolwarm')
```
![png](/notebooks/911_Project/output_50_1.png)


### Create a Clustermap using this DataFrame.


```jupyter
sns.clustermap(dayHour,cmap='viridis')
```
![png](/notebooks/911_Project/output_52_1.png)

Repeat these same plots and operations,

for a DataFrame that shows the Month as the column.


```jupyter
dayMonth = df.groupby(by=['Day_of_week','Month']).count()['Reason'].unstack()
```

### Create a HeatMap by Month column

```jupyter
plt.figure(figsize=(12,6))
sns.heatmap(dayMonth, cmap='coolwarm')
```
![png](/notebooks/911_Project/output_55_1.png)

### Create a Clustermap by Month column


```jupyter
sns.clustermap(dayMonth,cmap='viridis')
```
![png](/notebooks/911_Project/output_56_1.png)

So that was the fourth week.. 🔏
