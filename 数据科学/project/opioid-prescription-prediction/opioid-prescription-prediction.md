
# Use Machine Learning to Predict U.S. Opioid Prescribers with Scikit Learn


This Code Pattern will focus on and guide you through how to use scikit learn and python to predict opioid prescribers based off of a [2014 kaggle dataset](https://www.kaggle.com/apryor6/us-opiate-prescriptions/data).

In this notebook, we explore the dataset `opioids.csv`, `overdoses.csv`, `prescriber-info.csv` and apply:
- demographic data visualization
- machine learning trainings and model comparisons


<a id="section0"></a>
### Prerequisites

Before you run this notebook complete the following steps:
- Import required packages

#### Import required packages

Import and configure the required packages, including StringIO, requests, json, pandas, numpy.
This Code Pattern will focus on and guide you through how to use scikit learn and python  to predict opioid prescribers based off of a [2014 kaggle dataset](https://www.kaggle.com/apryor6/us-opiate-prescriptions/data).

In this notebook, we explore the dataset `opioids.csv`, `overdoses.csv`, `prescriber-info.csv` and apply:
- demographic data visualization
- machine learning trainings and model comparisons


```python
#import packages
from io import StringIO
import requests
import json
import sys
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```


```python
# define path
path = '/root/notebook/Data science with python/predict-opioid-prescribers/data/'
```


```python
path_opioids = path + 'opioids.csv'
# Using pandas to read the data 
df_data_1 = pd.read_csv(path_opioids,sep=',')
# Display the first five rows
df_data_1.head()
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
      <th>Drug Name</th>
      <th>Generic Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ABSTRAL</td>
      <td>FENTANYL CITRATE</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ACETAMINOPHEN-CODEINE</td>
      <td>ACETAMINOPHEN WITH CODEINE</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ACTIQ</td>
      <td>FENTANYL CITRATE</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ASCOMP WITH CODEINE</td>
      <td>CODEINE/BUTALBITAL/ASA/CAFFEIN</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ASPIRIN-CAFFEINE-DIHYDROCODEIN</td>
      <td>DIHYDROCODEINE/ASPIRIN/CAFFEIN</td>
    </tr>
  </tbody>
</table>
</div>



The opioids.csv dataset has two columns, `Drug Name` and `Generic Name`. Each row represents one medicine with its drug name and generic name.


```python
path_overdoses = path + 'overdoses.csv'
# Using pandas to read the data 
df_data_2 = pd.read_csv(path_overdoses)
# Display the first five rows
df_data_2.head()
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
      <th>State</th>
      <th>Population</th>
      <th>Deaths</th>
      <th>Abbrev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Alabama</td>
      <td>4,833,722</td>
      <td>723</td>
      <td>AL</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alaska</td>
      <td>735,132</td>
      <td>124</td>
      <td>AK</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Arizona</td>
      <td>6,626,624</td>
      <td>1,211</td>
      <td>AZ</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Arkansas</td>
      <td>2,959,373</td>
      <td>356</td>
      <td>AR</td>
    </tr>
    <tr>
      <td>4</td>
      <td>California</td>
      <td>38,332,521</td>
      <td>4,521</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>



The overdoses.csv dateset has four columns, including `State`, `Population`, `Deaths`, and `Abbrev`. Each row represents a state and its populations, number of deaths and state abbreviation names.


```python
path_overdoses = path + 'prescriber-info.csv'
# Using pandas to read the data 
df_data_3 = pd.read_csv(path_overdoses)
# Display the first five rows
df_data_3.head()
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
      <th>NPI</th>
      <th>Gender</th>
      <th>State</th>
      <th>Credentials</th>
      <th>Specialty</th>
      <th>ABILIFY</th>
      <th>ACETAMINOPHEN.CODEINE</th>
      <th>ACYCLOVIR</th>
      <th>ADVAIR.DISKUS</th>
      <th>AGGRENOX</th>
      <th>...</th>
      <th>VERAPAMIL.ER</th>
      <th>VESICARE</th>
      <th>VOLTAREN</th>
      <th>VYTORIN</th>
      <th>WARFARIN.SODIUM</th>
      <th>XARELTO</th>
      <th>ZETIA</th>
      <th>ZIPRASIDONE.HCL</th>
      <th>ZOLPIDEM.TARTRATE</th>
      <th>Opioid.Prescriber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1710982582</td>
      <td>M</td>
      <td>TX</td>
      <td>DDS</td>
      <td>Dentist</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1245278100</td>
      <td>F</td>
      <td>AL</td>
      <td>MD</td>
      <td>General Surgery</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1427182161</td>
      <td>F</td>
      <td>NY</td>
      <td>M.D.</td>
      <td>General Practice</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1669567541</td>
      <td>M</td>
      <td>AZ</td>
      <td>MD</td>
      <td>Internal Medicine</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>1679650949</td>
      <td>M</td>
      <td>NV</td>
      <td>M.D.</td>
      <td>Hematology/Oncology</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>28</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 256 columns</p>
</div>



The prescriber-info.csv displays number of drugs prescribed by 25000 medical professionals in the United States in 2014. The number of drug includes both opiate and non-opiate drugs. For each row, it represents a medical professional with their NPI number, gender, state, credentials, and speciality. In the paper *Opiate/Opioid prescription analysis using machine learning*, **they classify a doctor as opiate prescriber if he/she prescribe opiate drugs more than 10 times in total.**

## Exploration and Initial Preprocessing

Let's start out by removing the ',' from our numbers in the Deaths and Population columns so that we can use the cells as integers


```python
df_data_2['Deaths'] = df_data_2['Deaths'].str.replace(',', '')
df_data_2['Deaths'] = df_data_2['Deaths'].astype(int)
```


```python
df_data_2['Population'] = df_data_2['Population'].str.replace(',', '')
df_data_2['Population'] = df_data_2['Population'].astype(int)
```

Add an additional column where we see the deaths per capita per each state.


```python
df_data_2['Deaths/Population'] = (df_data_2['Deaths']/df_data_2['Population'])
```


```python
df_data_2.head()
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
      <th>State</th>
      <th>Population</th>
      <th>Deaths</th>
      <th>Abbrev</th>
      <th>Deaths/Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Alabama</td>
      <td>4833722</td>
      <td>723</td>
      <td>AL</td>
      <td>0.000150</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alaska</td>
      <td>735132</td>
      <td>124</td>
      <td>AK</td>
      <td>0.000169</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Arizona</td>
      <td>6626624</td>
      <td>1211</td>
      <td>AZ</td>
      <td>0.000183</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Arkansas</td>
      <td>2959373</td>
      <td>356</td>
      <td>AR</td>
      <td>0.000120</td>
    </tr>
    <tr>
      <td>4</td>
      <td>California</td>
      <td>38332521</td>
      <td>4521</td>
      <td>CA</td>
      <td>0.000118</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_data_2['Deaths/Population'].dtype
```




    dtype('float64')



Import pixiedust. Use pixiedust to visualize our initial exploration.

Curious about how many opioid deaths in each state? Let's plot a graph.


```python
names = df_data_2['State'].values.tolist()
values = df_data_2['Deaths']
plt.figure(figsize=(10,10)) 
# 绘图 x= 起始位置， bottom= 水平条的底部(左侧), y轴， height 水平条的宽度， width 水平条的长度
plt.bar(x=0, bottom=names, height=0.5, width=values, orientation="horizontal")
plt.title('Number of deaths by state') 
plt.yticks(np.arange(49), names)           
plt.ylabel('State',fontsize=14) 
plt.xlabel('Deaths',fontsize=14) 
plt.show()
```


![png](output_18_0.png)


It definitely looks like California has significantly more deaths than any other states. 

Let's remember, however, California is a huge state with a matching population. Even thtough the total number of deaths is huge, it doesn't imply that people in California have a higher death rate. Because of this we need to take a look at the values of deaths per capita.


```python
#What about deaths (% of population) by U.S. State?
names = df_data_2['State'].values.tolist()
values = df_data_2['Deaths/Population']
plt.figure(figsize=(10,10)) 
# 绘图 x= 起始位置， bottom= 水平条的底部(左侧), y轴， height 水平条的宽度， width 水平条的长度
plt.bar(x=0, bottom=names, height=0.5, width=values, orientation="horizontal")
plt.title('Deaths/Population') 
plt.yticks(np.arange(49), names)           
plt.ylabel('State',fontsize=14) 
plt.xlabel('Deaths/Population',fontsize=14) 
plt.show()
```


![png](output_20_0.png)


We can see that West Virginia, New Mexico, New Hampshire, Ohio, Kentucky and Delaware stand out. 

Let's check this out with a map. (I used google maps. To do this, create an API and enable JavaScript and GeoCoding. Then use your API key under 'Options'.)

### Let's move onto exploring our other dataset.

We seem to have a great deal of prescriptions as well as physicians' gender, state, speciality, whether they are an opioid prescriber or not and unique ID.


```python
df_data_3.count()
```




    NPI                  25000
    Gender               25000
    State                25000
    Credentials          24237
    Specialty            25000
                         ...  
    XARELTO              25000
    ZETIA                25000
    ZIPRASIDONE.HCL      25000
    ZOLPIDEM.TARTRATE    25000
    Opioid.Prescriber    25000
    Length: 256, dtype: int64



This table output reflects that there are 25000 unique medical professionals in the United States in year 2014.

### Data Cleaning

Why are there more than 50 states?


```python
# Let's take a look at the states.
df_data_3.State.unique()
```




    array(['TX', 'AL', 'NY', 'AZ', 'NV', 'PA', 'NH', 'WI', 'PR', 'CO', 'OH',
           'MA', 'CT', 'FL', 'MN', 'UT', 'IA', 'IL', 'MT', 'IN', 'VA', 'CA',
           'OR', 'NE', 'MI', 'NM', 'TN', 'KS', 'LA', 'MD', 'MO', 'AR', 'NC',
           'NJ', 'SC', 'WY', 'ME', 'OK', 'ND', 'KY', 'GA', 'DE', 'WA', 'RI',
           'WV', 'AK', 'ID', 'VT', 'HI', 'MS', 'DC', 'SD', 'AE', 'ZZ', 'GU',
           'AA', 'VI'], dtype=object)




```python
# Compare to df_data_2.
df_data_2.Abbrev.unique()
```




    array(['AL', 'AK', 'AZ', 'AR', 'CA', 'CO', 'CT', 'DE', 'FL', 'GA', 'HI',
           'ID', 'IL', 'IN', 'IA', 'KS', 'KY', 'LA', 'ME', 'MD', 'MA', 'MI',
           'MN', 'MS', 'MO', 'MT', 'NE', 'NV', 'NH', 'NJ', 'NM', 'NY', 'NC',
           'ND', 'OH', 'OK', 'OR', 'PA', 'RI', 'SC', 'SD', 'TN', 'TX', 'UT',
           'VT', 'VA', 'WA', 'WV', 'WI', 'WY'], dtype=object)



Compare the state list, clean up states to make the dataset state list equal.

I checked the list of US state abbreviations and did not recognize PR, AE, ZZ, GU, AA or VI. After checking I learned that PR is Puerto Rico, GU is Guam and VI is Virgin Islands. Though I identified 3 of the 6 unknowns, I'll remove all of them as dataset 2 does not have data regarding PR, GU or VI.


```python
df_data_3 = df_data_3[df_data_3.State != 'AE']
df_data_3 = df_data_3[df_data_3.State != 'ZZ']
df_data_3 = df_data_3[df_data_3.State != 'AA']
df_data_3 = df_data_3[df_data_3.State != 'PR']
df_data_3 = df_data_3[df_data_3.State != 'GU']
df_data_3 = df_data_3[df_data_3.State != 'VI']
```


```python
# Make sure it worked!
df_data_3.State.unique()
```




    array(['TX', 'AL', 'NY', 'AZ', 'NV', 'PA', 'NH', 'WI', 'CO', 'OH', 'MA',
           'CT', 'FL', 'MN', 'UT', 'IA', 'IL', 'MT', 'IN', 'VA', 'CA', 'OR',
           'NE', 'MI', 'NM', 'TN', 'KS', 'LA', 'MD', 'MO', 'AR', 'NC', 'NJ',
           'SC', 'WY', 'ME', 'OK', 'ND', 'KY', 'GA', 'DE', 'WA', 'RI', 'WV',
           'AK', 'ID', 'VT', 'HI', 'MS', 'DC', 'SD'], dtype=object)




```python
# Check out how many credentials there are.
df_data_3.Credentials.unique()
```




    array(['DDS', 'MD', 'M.D.', 'DO', 'RN, MSN, ANP-BC', 'O.D.', nan,
           'D.D.S.', 'ACNP', 'DPM', 'PAC', 'A.R.N.P.', 'MSN, APRN, BC',
           'D.O.', 'M.D,', 'APRN', 'PA-C', 'CNM', 'RN CNP', 'DDS MS',
           'DNP, APRN-BC, FNP', 'PHARM D.', 'CRNP', 'ARNP', 'D.D.S', 'DPM MD',
           'FNP', 'NP', 'DMD', 'PA', 'MPT', 'D.M.D.', 'APRN BC FNP',
           'DMD,FAGD', 'MD,MPH', 'BDS,  DDS', 'D.D.S., F.A.G.D.', 'DDS, MD',
           'PMH, CNP/CNS', 'P.A.', 'M.D', 'D.O., MPH & TM', 'M.D., PH.D.',
           'RPA-C', 'MD FACOG', 'F.N.P.', 'D.O', 'ANP', 'FNP-C', 'D.M.D',
           'D.P.M.', 'OD', 'O. D.', 'CNS', 'MS, APRN, FNP-C', 'MD MPH',
           'NP-C', 'PHARM D', 'M.D.,', 'MBBS', 'PHYSICIAN ASSISTANT',
           'M.D. FCCP', 'MD.', 'CNP', 'DPT', 'D.D.S, M.D.', 'APRN, BC',
           'P.A.-C', 'M.D.P.A.', 'F.N.P.-C', 'M. D.', 'CFNP', 'C.N.P', 'APN',
           'D.D.S., A.P.C.', 'NP-C, MSN', 'MD FACP', 'R.P.A.', 'N.P.',
           'APRN, CNS', 'D.D.S., M.D.', 'PMHNP', 'MD, PHD, ABFP', 'APNP',
           'DMD, MD, PLLC', 'M.D., P.A.', 'MPA, MSED, ATC, PA-C', 'D O',
           'M.D., M.H.S.', 'M.S., N.P.-C', 'FNP-BC', 'P.A', 'FPMH-NP', 'NFP',
           'ARNP-BC', 'CPNP', 'A.P.R.N.', 'P.A.-C.', 'NURSE PRACTITIONER',
           'DMD MS', 'APN, BC', 'M.B.B.S.', 'RN, FNP', 'NP, CNM', 'ANP-BC',
           'PA-C, MS', 'MD IN TRAINING', 'RN, CNP', 'CRNP,MSN,BC',
           'MSN, FNP-C', 'D.O., D.O.S.', 'D D S  M D', 'DNP, ARNP',
           'MSN, ARNP, ACNP-BC', 'OO', 'MD PA', 'RN,FNP', 'D.D.S., M.S.',
           'MBBS, MD', 'DNP, APRN', 'D.O .', 'CNP, APRN, CNS-RX', 'M.D. PH.D',
           'D.D.S., M.S.D.', 'MB,BS', 'FAMILY NURSE PRACTIT', 'WHNP',
           'P.A.C.', 'ND', 'PHARM. D.', 'ANP.', 'RN, APN, FNP', 'PA-C, ATC',
           'RPA', 'MD, PHD', 'NP, CNS', 'MD, FACEP, FAAEM', 'ANP,BC',
           'WHNP-BC', 'APRN-BC, ANP, FNP', 'ANP-FNP-BC', 'D.D.S, F.A.G.D',
           'DDS MD', 'P.A-C', 'M.D.,MBA', 'MSN, APN, CNP, NP-C',
           'M.D., M.P.H.', 'R.N.  NPF', 'MSN, FNP-BC', 'PMHHNP', 'FNP,NPP',
           'C. - F. N. P.', 'DDS. MS', 'O.D., M.P.H.', 'N.D.', 'N. P.',
           'ARNP, BSN, MN', 'MD, MSBS', 'M.D, MPH', 'NP-ADULT', 'MS',
           'M.D. P.A.', 'MS,APRN,FNP,BC', 'M.D., PHD', 'DNP, APRN-BC',
           'OD PC', 'DNP, APRN, NP-C', 'MD, MS', 'MS,MPH', 'M.S.N., A.P.N.',
           'O.D', 'D.D,S.', 'MD, PH.D.', 'MD, MPH', 'PA-C, MPAS',
           'D.D.S., F.R.C.D.C.', 'ATC, FNP-C', 'PHD, APN', 'APRN-BC,FNP',
           'ARNP, BC-GNP', 'RN', 'M.D., F.A.C.R.', 'PA-C (PHYSICIAN ASSI',
           'APRN-CNS', 'D,O.', 'MEDICAL DOCTOR', 'D. O.', 'D.D.S, P.A.',
           'MPAS, PA-C', 'OD, PA', 'DDS, MSD', 'A.P.R.N. N.P.', 'CNM, FNP-C',
           'B.S., PA-C', 'M.D MRCP', 'MD PHD', 'PCS', 'ACNP-BC', 'C.N.P.',
           'MD MBA', 'D.M.D., M.D.', 'RNP', 'PHARMD', 'M.D. ,PH.D.',
           'MD, PHD, FACC', 'D.O., M.S.', 'ARNP, CNM', 'M.D. ; M.B.,B.S.',
           'MS, FNP-BC', 'MD FACE', 'M.D., PA', 'ARNP-C', 'RN, CFNP',
           'ACNP-BC, MSN', 'CANP', 'FNP, PHD', 'MBBS, FRCS', 'PHP MD', 'PHC',
           'BOARD CERTIFIED FNP', 'RN, ANP', 'M.D, MSC', 'MNS', 'PMH-NP-BC',
           'M.H.S., P.A.', 'DNP, ANP-BC', 'NPC', 'M.S.N.', 'DNP, FNP-BC',
           'HERSCHEL ROSS DDS', 'M.D., PH. D.', 'DENTIST', 'M.S. C.N.P.',
           'MS, PA-C', 'R.P.A.C.', 'MHS, PA-C', 'RN, NP', 'D.D.S., PC',
           'MSPAS, PA-C', 'JAMES KIM', 'DDS PS', 'APRN, FNP-BC',
           'BSHS, MSN, FNP-BC', 'ANP-C', 'RPA C', 'MD MPH MS DRPH',
           'DDS,CAGS', 'A.P.N.', 'D.D.S., B.S.', 'RPAC', 'N.M.D.', 'DDS,MSD',
           'D.D.S.,M.S.', 'CNFP', 'ARNP FNP-BC', 'RN, MSN, CCRN, ANP',
           'MS RN NPP', 'APRN-BC', 'APN,FNP', 'M.D. - PH.D', 'PHD, FNP',
           'MD P C', 'DDS PLLC', 'RN, CS, FNP', 'CRNP- FAMILY', 'D.C., FNP',
           'DOCTOR OF OPTOMETRY', 'D.D.S., F.A.C.D.', 'RN, MSN, PMHNP, BC',
           'FNP, PMHNP', 'DNP', 'FLORA LAU', 'DDS,MD', 'MD, FACP',
           'APN, BC-FNP', 'DMD, MD', 'MD, PH.D', 'DDS, MAGD', 'D.P.M', 'DDA',
           'D.M.D., P.C', 'MD  PA', 'PMHCNS-BC', 'RN MSN FNO-C', 'MD, FACS',
           'M.D. (H)', 'ARNP, DNP', 'PHARM.D.', 'MSN ANP', 'FNP - C',
           'DNP,PMHCNS-BC', 'D.D.S., PH.D.', 'MD, MBA', 'GNP-BC',
           'M.D., M.B.A', 'RN,NP', 'MHSA, PA-C', 'PA C', 'MD FACS',
           'APRN, PHD, CNS', 'F.N.P', 'DPM, FACFS', 'M.P.A.S., P.A.C., LP',
           'D.O.,', 'RN,CFNP', 'M.D., PHARMD.', 'NP,  APRN BC', 'MD., PH.D.',
           'M.D., MPH', 'RN MSN CNM ARNP', 'A.R.N.P.-B.C.', 'R.PH.',
           'PMHNP-BC', 'DMD, MSD', 'FNP-BC, NP-C', 'C.N.S.', 'P.D., D.D.S.',
           'DNP / FNP', 'CRNP, FNP-BC, RN', 'MD FACC', 'AGNP', 'DP',
           'RN, MSN, ACNP-BC', 'RN, CS, MS(N),FNP', 'M,D.', 'ARNP,BC',
           'RN, MSN, NP-C', 'N.P', 'M.D.,F.A.C.O.G.', 'MSN,FNP-BC', 'NP-F',
           'MSN, APN,C', 'ANNA TAVAKKOLI M.D.', 'MS, ANP-C', 'MSN, ARNP',
           'DNP, CRNP, NP-C', 'ANPC', 'M.D., M.S.H.S.', 'M.D.,M.P.H.', 'LPN',
           'B.S', 'M.N., ARNP', 'MD RD', 'ANP/CNP', 'M.D.,MPH',
           'MD,  FCCP, D-ABSM', 'D.D.S,PC', 'D.O., M.D', 'MD FACP FACC',
           'DNP, FNP-C', 'A.N.P.', 'MD, PC', 'D.M.D., F.A.G.D.', 'DMD, MDS',
           'FNP, BC', 'M.B.B.S', 'CRNA', 'MSN, CRNP', 'PA-C, L.AC.',
           'M.S., NP-C', 'A.T.,C. , C.S.C.S.', 'M.D., F.A.C.P.', 'PHYSICIAN',
           'RN, MSN, FNP', 'APN-C', 'MS RN CS', 'M.D., P.C.', 'DDS  PC',
           'APRN, PMHNP-BC', 'M.D. MPH', 'A.R.N.P. , PMHNP-BC', 'M.D., MS',
           'MSN, CNS, ACNP-BC', 'M.D..', 'PA, O.T.', 'FNP-C, ACNP', 'ACNS-BC',
           'R.N.', 'M.S.N., R.N., N.P.', 'MD, MSHS', 'M.B.;B.S.',
           'M.D., FACOG', 'D.D.S, M.S', 'C.R.N.P.', 'MBBS (MD)', 'MBBCH',
           'MSN, ACNP-BC, CCRN', 'ANP-BC, GNP', 'DDS, MS', 'M.D., M.A.',
           'MS ANPC', 'M.D., FACS', 'CNP, CNM', 'DRNP', 'M.B., B.S.',
           'MBBS,MS', 'D.M.D., M. S.', 'R.N., N.P.', 'RN NP', 'M.D., FAAFP',
           'M.D. PH.D.', 'C-FNP', 'PHARMD, DDS', 'M.D., PH.D', 'ANRP',
           'A.P.R.N., B.C.', 'RN, APRN, NP, B.C.', 'PMHNP-BC. NP',
           'N.D., LAC', 'DMD MD', 'M.D.PH.D.', 'MD, MS, MBA', 'MB.BS, FRCSC',
           'M.D.,P.A.', 'M.D., M.S.', 'RN CFNP', 'M D F A A D',
           'A.P.R.N., W.H.N.P.', 'MSN, APRN, FNP-BC', 'D. D. S.',
           'D.M.D, M.D.', 'MD, MSPH', 'RPH,BCOP, BCPS', 'DMD, MS',
           'MSN, APRN, BC, FNP', 'D.D.S. P.S.', 'RN, PMHNP', 'WHNP, CFNP',
           'DPM PC', 'RN,C,FNP', 'ARNP CS', 'DDS, PS', 'C.N.M.,F.N.P.',
           'DNP, FNP', 'DMD BA', 'PA (PHYSICIAN ASSIST', 'D.O., RPH',
           'CNM, FNP, DNP', 'M.D., F.C.C.P.', 'DO, LACC', 'R PA-C', 'P.A.C/',
           'B.D.S.', 'DDS,MS', 'DC, PA-C', 'DMD DR MED DENT', 'RN CNS',
           'WHCNP/ANP', 'ARANP', 'MS, RN, FNP-C', 'GNP-C', 'D.O., M.D.',
           'PA-C, MSPAC', 'M. D. M.P.H.', 'DO, MPH', 'PH.D., NP-C',
           'FNP-APNP', 'OD, FAAO, FOAA', 'BDS, DDS', 'M D', 'MT',
           'R.N, M.S.N., G.N.P.', 'MD, M.P.H., A.T.C', 'OD, MS',
           'PHD, NP, PMHNP-BC', 'ARNP, CDE', 'RN ACNP', 'M.D.,M.R.C.S.',
           'PA-C MPAS', 'RN, MSN, CS, FNP', 'DNP FNP-BC', 'MD, PA', 'NPF',
           'PSYD', 'D.D.S., P.A.', 'MSN, NP-C', 'MD, DDS', 'EIN-', 'WHCNP',
           'APN, CNP', 'R.N., N.P., C.S.', 'M.D. PHD', 'CCNS, NP',
           'DDS. PRACTICE LIMITE', 'ARNP, LP', 'ND, ARNP', 'D.M.D. P.A.',
           'APRN BC', 'DDS, FAGD', 'C-PA', 'APN, C', 'APN FNP-C', 'MD SC',
           'MSN, MPH, CNM, FNP-C', 'D.O.,P.A.', 'D D S', 'GNP-BG',
           'MSN, RN, FNP', 'ARNP BC', 'PHD', 'RN, NP-C', 'RN, MSN, FNP-C',
           'MD FACG', 'DMD,MD', 'APRN,BC', 'M.D., M.P.H., M.B.A.', 'A.R.N.P',
           'DMD, MPH', 'MSN, RN, ACNP-BC', 'M.D., PHD, NEUROLOGY', 'APN-NP',
           'BCFNP', 'RN BC ANP', 'CNP-FNP-DNP', 'APN, ACNS-BC', 'GNP/APRN',
           'FNPC', 'MMS, PA-C', 'D.M.D., M.S.', 'D.O,', 'MD, FACOG', 'DR',
           'FNP-BC, PMHNP-BC', 'C.R.N.P', 'NPP', 'MD, MPH, FACP',
           'M.D.,PH.D.', 'DO, FACOOG', 'APN, MSN', 'DMD FAGD', 'D.M.D, M.S.',
           'APRN,CS', 'APRN-BC, ND', 'LPN, RN, ANP', 'O.D, F.A.A.O.', 'C- NP',
           'CRNP PMH', 'RN, BSN, CRNA, MSN', 'APRN, PHD.', 'C.N.M.',
           'D.D.S, M.S.', 'MD/PHD', 'DDS, MS, MSPH', 'R.N.P.', 'MED, LPC',
           'PHYSICIAN ASST PA C', 'MD,PA', 'M.D.  MMSC.', 'NP, MSN',
           'R.N., C.N.P.', 'PHD APN FNP-C', 'FNP BC', 'M.D.PA', 'MSN',
           'RN, CRNP', 'RN-C, FNP', 'DMD PC', 'D.N.P.', 'FNP, ARNP', 'DPM`',
           'MFTI', 'PHD PAC', 'APRN-BC, CFNP', 'D.M.D., P.A', 'MD, FACC',
           'APRN, NP', 'MB, BCH', 'DC', 'M.D., M.B.A.', 'APRN,PMHNP-BC',
           'M.D., P.T., A.T.,C.', 'CRNP-PMH', 'D. P. M.', 'PHARM. D',
           'MSN-FNP', 'M. D.,', 'MSN, APRN, PMHNP-BC', 'MD, DVM', 'MS, APRN',
           'RN, MA, NP-C,', 'GNP', 'APRN, PMHNP', 'M.D., D.T. M & H',
           'M.M.D.', 'RN BSN MSN FNP', 'D.D.S.,M.M.SC.', 'OPTOMETRIST',
           'OTRL', 'PHD., ARNP, BC', 'MSN, APRN, FNP-C', 'RN, WHNP, ANP',
           'D.M.D.,M.S.', 'DNP, MS, FNP-C', 'CNS/ANP', 'PA-C, MSC',
           'D.D.S., M.S., P.C.', 'APRN-FNP', 'D.D.S. M.D.', 'PT', 'MSSW, LSW',
           'D.O., P.C.', 'R.N., C.F.N.P.', 'RN,BSN,APN-C', 'APRN, FNP',
           'CRNP, NP-C', 'DNP,CRNP', 'D.D.S, M.D', 'RN, CNN, APN', 'RNCS',
           'PHYSICIAN ASSISTANTS', 'APRN (N.P)', 'M.D.  MPH', 'PA-C, DMO',
           'M.D.FACP', 'DNP-CFNP', 'PA-C, ND, MSOM, MS', 'D.D.S., P.C',
           'FNP, CNM', 'RN, FNP-C', 'ANP B-C', 'DPM PA', 'C.F.N.P',
           'PA-C, MPA', 'D.D.S.,M.S', 'PA-C, MPH', 'PMHNP, RN', 'MSN, NPC',
           'M.D., F.A.C.E', 'RN,CNP', 'NP, PHD', 'DDS, PHD', 'PHD, ARNP',
           'D.O., P.A.', 'M.D.F.A.C.C.', 'FNP/C', 'PH. D., PA-C', 'RN, APNP',
           'M.D., F.A.C.C.', 'APN, NP-C', 'FNP CRNP', 'D. D. S., M. S.',
           'RN,CS,MSN', 'RN, MS, FNP-C', 'M.D.PHD', 'MSM, PA-C',
           'FNP-BC, CNM', 'MD DO', 'MS, ARNP, PMHNP-BC', 'D.M.D, P.A.',
           'PA-C, PHARM.D.', 'F.N.P.C.', 'F.A.A.O', 'MSN, ACNS-BC, AGNP-C',
           'MD, RPVI', 'MS, RN, ANP', 'GREG SAWYER, D.D.S.', 'M,D,', 'DDS PA',
           'ARNP RN BSN MSN', 'C-NP', 'M.D., OD', 'ACNP-BC, CCNS',
           'DDS, PLLC', 'MSN APRN BC', 'P. A.', 'M.D/', 'MSN, CNS, A.P.R.N.',
           'MS, APN, CANP', 'MD, MA', 'M.ED., O.D.', 'RPH.', 'APRN, FNP-C',
           'APRN, CSN', 'D.M.D., P.A.', 'MSN, APRN FNP-BC', 'MSN CRNP',
           'RN, DDS, MS, PHD', 'O.D, P.A.', 'CRNP-F, APRN-PMH', 'MS,PA-C',
           'APRN, CNP, CNNP', 'PA, RN', 'P.T.', 'MSPA, PA-C', 'FNP, DCNP',
           'MS PA-C', 'MD, ABIHM', 'NURSE PRACTIONONER', 'MSN, APN, C',
           'APRN-CNP', 'M.D., M.P.H', 'MSN, GNP-BC', 'ARNP - FNP-C',
           'D.D.S., M.S., PH.D.', 'MD/FACS', 'NP, DNP', 'M.D., M.S',
           'MD, CWS, FCCWS', 'M.D. PA', 'PMHNP/ANP', 'NURSE PRACTIONER',
           'D.O, P.A.', 'AGACNP', 'DNP, FNP, PMHNP-BC', 'B.S.N., C.R.N.P.',
           'RN, FNP-BC', 'MS, MSN, RN, FNP-BC', 'M.D., PH.D., M.SC.',
           'RN, MSN, FNP, NP-C', 'PA PHD', 'RN-BC, MSN, CRNP', 'MHS, PAC',
           'PHARM.D', 'PH. D. , M.D.', 'ENP', 'M.D,F.A.C.P,F.A.C.G', 'RPH',
           'ADULT NP', 'D.D.S.,M.D', 'MB CHB', 'FNP-BC, WHNP-BC',
           'M.D., LLC.', 'MD FACEP', 'CRNA, FNP-C', 'ARNP, NP-C', 'RNCNP',
           'ARNP    PHD', 'RN,MSN,FNP-C', 'RN, APRN-BC, A', 'APRN-C', 'M.D>',
           'D.O., MPH', 'N.P., M.P.H.', 'MD DMD', 'RN MSN APN', 'AGPCNP-BC',
           'M.S.N., C.N.S.', '(DMD)', 'B.D.S., M.S.', 'RPA-C MPAS',
           'D.D.S, FAGD', 'RN,MSN,CNP', 'MD FACC FACA FCCP', 'MSN, FNP',
           'FNP, ACNP', 'DNP, RN, ANP-BC', 'MSARNP', 'APRN-BC, FPMHNP', 'NMD',
           'MD, PHD, FACP', 'M.D., J.D.', 'F.N.P.-C.', 'RN, MSN, CANP',
           'MD OD', 'D.D.S.,M.P.H', 'DPM, MS', 'R.N.,N.P.,C.N.S.,',
           'MD, FACC, NASPE', 'OPHTHALMOLGIST M.D.', 'RN,MS,CNS',
           'D.M.D., M.M.SC.', 'MD,MRCPI,FASN', 'M.D. M.P.H.',
           'GERALD SULLIVAN', 'M..', 'APRN-RN', 'MD, MSC', 'MD OBGYN',
           'DDS, MPH', 'MD`', 'MD, FAAFP', 'M.D./FAMILY PRACTICE', 'APN,C.',
           'M.S.N., R.N., A.P.N.', 'RN, MS, FNP', 'N P', 'ACNP/FNP', 'MDPHD',
           'RNC-OB', 'CNP, CNS, APRN BC', 'FNP-BC, ARNP', 'APRN-BC, MSN',
           'P.A.C', 'RN, ACNP-BC', 'M.D./PH.D.', 'M.D., F.A.A.F.P.',
           'M.D., P.C', 'AHCNS', 'RN, ACNS-BC', 'DNP, PMHNP-BC', 'M.D, PHD',
           'M.D., PH.D., MRCP', 'M.S.N., N.P.', 'P,A,', 'DR,NP',
           'D.D.S., P.C.', 'M.D., PHARM.D.', 'DO, M.S.', 'MHSC, PA-C',
           'MSN - FNP-C', 'DDS, PC', 'C.F.N.P.', 'MD FACC FSCAI FSVM',
           'DNSC, ARNP, CS', 'NP C', 'M.B.,B.S.', 'PH.D., MSN, PMHNP.',
           'CFPMHNP', 'M.D.,  PHD.', 'DDS, MS, PC', 'RN, BSN, APN', 'PA, PT',
           'RXN, NP', 'MS,PAC', 'RN, APRN', 'RN, MSN, PMHNP', 'RN,DDS',
           'RNCS FNP', 'DDS,MS, DIPLOMATE', 'DDA, PA', 'MSDMD', 'DDS.FAGD',
           'RN/PC, PMHCNS-BC', 'PSYNP', 'M.D., FACP', 'MSN APRN',
           'M.D.  F.A.C.P.', 'D.M.D., J.D.', 'MD,DC', 'DMD MSD',
           'M.D., F.A.P.A.', 'DDS MAGD,PC', 'MD DDS', 'MDFAC',
           'R.N. MS. C.U.N.P.', 'ARNP, FNP-BC', 'D.D.S., D.M.D.', 'MSN,  APN',
           'FACEP MD', 'D O P C', 'FNP, GNP, RN', 'NP,RN', 'RNC WHNP',
           'P.A, A.T.C', 'DNP, PMHNP-C, FNP-C', 'MSN, AGPCNP-BC', 'O D  PS',
           'OD, FAAO', 'CNS   FNP-C', 'MSN, APRN', 'MD,PH.D', 'NP, RN',
           'MBBS, MRCP', 'RN, MSN, OCN, NP-C', 'M.D.,PHD', 'D.P.M., FACFAS',
           'CNM C', 'MS, RPA-C', 'FNP MSN RN', 'BRIAN MASTERSON',
           'DO, MBA, MS', 'DDS, PA', 'RN,MSN', 'RN/BSN, MSN ,FNP-BC',
           'PH.D., MD', 'MD FAAOS FACS', 'D.O., PH.D.', 'D. M. D.',
           'DENTIST DDS', 'MDCM FRCS', 'N.P.-C', 'RNC-ANP', 'APNC',
           'APRN, NP-C', 'MD-A PROF CORP.', 'MD MS', 'BA,,DC, DO,JD, LLM',
           'M.D., FACC', 'MD, PT', 'P.A.,C.', 'MSN NP', 'PA-C, MSPAS, MPH',
           'PHYSICIAN ASSISANT', 'DDS FAGD'], dtype=object)




```python
# Check out the specialties.
df_data_3.Specialty.unique()
```




    array(['Dentist', 'General Surgery', 'General Practice',
           'Internal Medicine', 'Hematology/Oncology', 'Family Practice',
           'Nurse Practitioner', 'Optometry', 'Cardiology',
           'Obstetrics/Gynecology', 'Podiatry', 'Physician Assistant',
           'Diagnostic Radiology',
           'Student in an Organized Health Care Education/Training Program',
           'Neurology', 'Certified Nurse Midwife', 'Rheumatology',
           'Pharmacist', 'Urology', 'Cardiac Electrophysiology',
           'Dermatology', 'Emergency Medicine', 'Psychiatry & Neurology',
           'Infectious Disease', 'Psychiatry', 'Gastroenterology',
           'Ophthalmology', 'Thoracic Surgery',
           'Oral Surgery (dentists only)', 'Anesthesiology',
           'Orthopedic Surgery', 'Otolaryngology', 'Pulmonary Disease',
           'Neuropsychiatry', 'Physical Therapist', 'Pediatric Medicine',
           'Physical Medicine and Rehabilitation', 'Maxillofacial Surgery',
           'Certified Clinical Nurse Specialist', 'Preventive Medicine',
           'Allergy/Immunology', 'Hand Surgery',
           'Hospice and Palliative Care', 'Medical Oncology', 'Nephrology',
           'Plastic and Reconstructive Surgery', 'Colon & Rectal Surgery',
           'Hospitalist', 'Colorectal Surgery (formerly proctology)',
           'Specialist', 'Geriatric Medicine', 'Gynecological/Oncology',
           'Neurosurgery', 'Clinic/Center', 'Interventional Pain Management',
           'Orthopaedic Surgery', 'Radiation Oncology', 'Endocrinology',
           'Critical Care (Intensivists)', 'Vascular Surgery',
           'Family Medicine', 'Registered Nurse', 'Pain Management',
           'Sports Medicine', 'Unknown Physician Specialty Code',
           'Osteopathic Manipulative Medicine', 'Interventional Radiology',
           'Naturopath', 'Hematology', 'Preferred Provider Organization',
           'Cardiac Surgery', 'Geriatric Psychiatry',
           'Medical Genetics, Ph.D. Medical Genetics', 'Surgical Oncology',
           'Surgery', 'Legal Medicine',
           'Neuromusculoskeletal Medicine, Sports Medicine',
           'Health Maintenance Organization',
           'Psychologist (billing independently)', 'Neurological Surgery',
           'Plastic Surgery', 'Homeopath', 'Licensed Practical Nurse', 'CRNA',
           'Addiction Medicine', 'Specialist/Technologist',
           'Nuclear Medicine', 'Unknown Supplier/Provider',
           'Medical Genetics', 'Oral & Maxillofacial Surgery',
           'Clinical Pharmacology', 'Sleep Medicine', 'Pathology',
           'Counselor', 'Slide Preparation Facility', 'Rehabilitation Agency',
           'Psychologist', 'Military Health Care Provider',
           'General Acute Care Hospital',
           'Multispecialty Clinic/Group Practice',
           'Thoracic Surgery (Cardiothoracic Vascular Surgery)', 'Midwife',
           'Chiropractic', 'Licensed Clinical Social Worker',
           'Community Health Worker', 'Hospital (Dmercs Only)',
           'Pharmacy Technician', 'Behavioral Analyst'], dtype=object)




```python
# How much of the dataset is male vs female?
df_data_3.groupby('Gender').size() / df_data_3.groupby('Gender').size().sum()
```




    Gender
    F    0.378166
    M    0.621834
    dtype: float64




```python
# How many prescribers in our dataset prescribe opioid drugs vs do not?
df_data_3.groupby('Opioid.Prescriber').size() / df_data_3.groupby('Opioid.Prescriber').size().sum()
```




    Opioid.Prescriber
    0    0.41282
    1    0.58718
    dtype: float64




```python
# Plot the opioid prescriber count vs non opioid prescriber count.
# The dataset has a slightly higher number of opioid prescribers.
pd.value_counts(df_data_3['Opioid.Prescriber']).plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f5de0b13cd0>




![png](output_35_1.png)


## Combination of Approaches: Kaggle and Indiana University

 - "Quick and Dirty" approach from Kaggle (https://www.kaggle.com/jiashenliu/quick-and-dirty-attempt-on-voting-classifier)
 - "Detecting Frequent Opioid Prescription" (https://inclass.kaggle.com/apryor6/detecting-frequent-opioid-prescription)
 - Indiana University: "Opiate prescription analysis using machine learning" (https://github.com/anirudhkm/opiate-prescription-analysis/blob/master/AML_report.pdf)


```python
import matplotlib.pyplot as plt
%matplotlib inline
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
```


```python
# Find the shape of our data frame so that we know how to set our classifiers up.
print(df_data_3.shape)
```

    (24759, 256)
    

Mark opioid vs non opiod drugs in df_data_3 with use of df_data_1.


```python
opioids = df_data_1 
name = opioids['Drug Name']
import re
new_name = name.apply(lambda x:re.sub("\ |-",".",str(x)))
columns = df_data_3.columns
Abandoned_variables = set(columns).intersection(set(new_name)) #Form the intersection of two Index objects.
Kept_variable = []
for each in columns:
    if each in Abandoned_variables:
        pass
    else:
        Kept_variable.append(each)
```


```python
# Look at our new shape.
df = df_data_3[Kept_variable]
print(df.shape)
```

    (24759, 245)
    


```python
df.head()
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
      <th>NPI</th>
      <th>Gender</th>
      <th>State</th>
      <th>Credentials</th>
      <th>Specialty</th>
      <th>ABILIFY</th>
      <th>ACYCLOVIR</th>
      <th>ADVAIR.DISKUS</th>
      <th>AGGRENOX</th>
      <th>ALENDRONATE.SODIUM</th>
      <th>...</th>
      <th>VERAPAMIL.ER</th>
      <th>VESICARE</th>
      <th>VOLTAREN</th>
      <th>VYTORIN</th>
      <th>WARFARIN.SODIUM</th>
      <th>XARELTO</th>
      <th>ZETIA</th>
      <th>ZIPRASIDONE.HCL</th>
      <th>ZOLPIDEM.TARTRATE</th>
      <th>Opioid.Prescriber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1710982582</td>
      <td>M</td>
      <td>TX</td>
      <td>DDS</td>
      <td>Dentist</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1245278100</td>
      <td>F</td>
      <td>AL</td>
      <td>MD</td>
      <td>General Surgery</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1427182161</td>
      <td>F</td>
      <td>NY</td>
      <td>M.D.</td>
      <td>General Practice</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1669567541</td>
      <td>M</td>
      <td>AZ</td>
      <td>MD</td>
      <td>Internal Medicine</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>21</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>1679650949</td>
      <td>M</td>
      <td>NV</td>
      <td>M.D.</td>
      <td>Hematology/Oncology</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>28</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 245 columns</p>
</div>



Now let's remove the credentials column so that we can use the speciality column instead.

We will also remove the NPI column in order to trim our features down.


```python
df = df.drop(df.columns[[0, 3]], axis = 1) 
df.head()
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
      <th>Gender</th>
      <th>State</th>
      <th>Specialty</th>
      <th>ABILIFY</th>
      <th>ACYCLOVIR</th>
      <th>ADVAIR.DISKUS</th>
      <th>AGGRENOX</th>
      <th>ALENDRONATE.SODIUM</th>
      <th>ALLOPURINOL</th>
      <th>ALPRAZOLAM</th>
      <th>...</th>
      <th>VERAPAMIL.ER</th>
      <th>VESICARE</th>
      <th>VOLTAREN</th>
      <th>VYTORIN</th>
      <th>WARFARIN.SODIUM</th>
      <th>XARELTO</th>
      <th>ZETIA</th>
      <th>ZIPRASIDONE.HCL</th>
      <th>ZOLPIDEM.TARTRATE</th>
      <th>Opioid.Prescriber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>M</td>
      <td>TX</td>
      <td>Dentist</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>F</td>
      <td>AL</td>
      <td>General Surgery</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>134</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>F</td>
      <td>NY</td>
      <td>General Practice</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>M</td>
      <td>AZ</td>
      <td>Internal Medicine</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>21</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>M</td>
      <td>NV</td>
      <td>Hematology/Oncology</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>28</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 243 columns</p>
</div>




```python
# Let's now create our training and test data.
train,test = train_test_split(df,test_size = 0.2,random_state = 42)
print(train.shape)
print(test.shape)
```

    (19807, 243)
    (4952, 243)
    


```python
# Now we convert our categorical columns.
Categorical_columns = ['Gender','State','Specialty']

for col in Categorical_columns:
    train[col] = pd.factorize(train[col], sort = True)[0]
    test[col] = pd.factorize(test[col],sort = True)[0]
```

    /root/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      """
    /root/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    


```python
# Set our features.
features = train.iloc[:,1:242] #make sure we only use the columns that we want as our features
features.head()
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
      <th>State</th>
      <th>Specialty</th>
      <th>ABILIFY</th>
      <th>ACYCLOVIR</th>
      <th>ADVAIR.DISKUS</th>
      <th>AGGRENOX</th>
      <th>ALENDRONATE.SODIUM</th>
      <th>ALLOPURINOL</th>
      <th>ALPRAZOLAM</th>
      <th>AMIODARONE.HCL</th>
      <th>...</th>
      <th>VENTOLIN.HFA</th>
      <th>VERAPAMIL.ER</th>
      <th>VESICARE</th>
      <th>VOLTAREN</th>
      <th>VYTORIN</th>
      <th>WARFARIN.SODIUM</th>
      <th>XARELTO</th>
      <th>ZETIA</th>
      <th>ZIPRASIDONE.HCL</th>
      <th>ZOLPIDEM.TARTRATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>11299</td>
      <td>34</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>17247</td>
      <td>14</td>
      <td>38</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>14337</td>
      <td>9</td>
      <td>52</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>61</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>14452</td>
      <td>35</td>
      <td>85</td>
      <td>0</td>
      <td>0</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>12366</td>
      <td>9</td>
      <td>66</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 241 columns</p>
</div>



## Creating our Classifiers to Predict Opioid Prescribers


```python
import sklearn
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.naive_bayes import GaussianNB
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.ensemble import VotingClassifier
from sklearn.ensemble import BaggingClassifier
```

Train our models. Let's use several classifiers so that we can check out which has the highest accuracy.

Added bagging classifier to check for overfitting (along with cross validation).


```python
# With 'Gender' included.
features = train.iloc[:,0:242] 
# Make sure to remove Opioid.Prescriber (our target)!
target = train['Opioid.Prescriber']
Name = []
Accuracy = []
model1 = LogisticRegression(random_state = 22,C = 0.000000001,solver = 'liblinear',max_iter = 200)
model2 = GaussianNB()
model3 = RandomForestClassifier(n_estimators = 200,random_state = 22)
model4 = GradientBoostingClassifier(n_estimators = 200)
model5 = KNeighborsClassifier()
model6 = DecisionTreeClassifier()
model7 = LinearDiscriminantAnalysis()
model8 = BaggingClassifier()
Ensembled_model = VotingClassifier(estimators = [('lr', model1), ('gn', model2), ('rf', model3),('gb',model4),('kn',model5),('dt',model6),('lda',model7), ('bc',model8)], voting = 'hard')
for model, label in zip([model1, model2, model3, model4,model5,model6,model7,model8,Ensembled_model], ['Logistic Regression','Naive Bayes','Random Forest', 'Gradient Boosting','KNN','Decision Tree','LDA', 'Bagging Classifier', 'Ensemble']):
    scores = cross_val_score(model, features, target, cv = 5, scoring = 'accuracy')
    Accuracy.append(scores.mean())
    Name.append(model.__class__.__name__)
    print("Accuracy: %f of model %s" % (scores.mean(),label))
```

    Accuracy: 0.607866 of model Logistic Regression
    Accuracy: 0.612056 of model Naive Bayes
    Accuracy: 0.833039 of model Random Forest
    Accuracy: 0.824557 of model Gradient Boosting
    Accuracy: 0.778513 of model KNN
    Accuracy: 0.780078 of model Decision Tree
    Accuracy: 0.716665 of model LDA
    Accuracy: 0.810320 of model Bagging Classifier
    Accuracy: 0.833342 of model Ensemble
    


```python
# Gender not included.
features=train.iloc[:,1:242] 
#Make sure to remove Opioid.Prescriber (our target)!
target = train['Opioid.Prescriber']
Name = []
Accuracy = []
model1 = LogisticRegression(random_state = 22,C = 0.000000001,solver = 'liblinear',max_iter = 200)
model2 = GaussianNB()
model3 = RandomForestClassifier(n_estimators = 200,random_state = 22)
model4 = GradientBoostingClassifier(n_estimators = 200)
model5 = KNeighborsClassifier()
model6 = DecisionTreeClassifier()
model7 = LinearDiscriminantAnalysis()
model8 = BaggingClassifier()
Ensembled_model = VotingClassifier(estimators = [('lr', model1), ('gn', model2), ('rf', model3),('gb',model4),('kn',model5),('dt',model6),('lda',model7), ('bc',model8)], voting = 'hard')
for model, label in zip([model1, model2, model3, model4,model5,model6,model7,model8,Ensembled_model], ['Logistic Regression','Naive Bayes','Random Forest', 'Gradient Boosting','KNN','Decision Tree','LDA', 'Bagging Classifier', 'Ensemble']):
    scores = cross_val_score(model, features, target, cv = 5, scoring = 'accuracy')
    Accuracy.append(scores.mean())
    Name.append(model.__class__.__name__)
    print("Accuracy: %f of model %s" % (scores.mean(),label))
```

    Accuracy: 0.607866 of model Logistic Regression
    Accuracy: 0.612157 of model Naive Bayes
    Accuracy: 0.835109 of model Random Forest
    Accuracy: 0.824456 of model Gradient Boosting
    Accuracy: 0.778058 of model KNN
    Accuracy: 0.780936 of model Decision Tree
    Accuracy: 0.712727 of model LDA
    Accuracy: 0.813955 of model Bagging Classifier
    Accuracy: 0.834806 of model Ensemble
    

Looks like our highest accuracy score is 83.5% with Random Forest, followed by 83.3% with the Ensemble.

Overall, it seems our models are less accurate when 'Gender' is included, with the exception of our Ensemble which gets a fairly high accuracy at 83.4% accuracy.


```python
# Let's check out our best models from our run without 'Gender'.
Name_2 = []
Accuracy_2 = []
Ensembled_model_3 = VotingClassifier(estimators = [('rf', model3),('em',Ensembled_model)], voting = 'hard')
for model, label in zip([model3, model4,Ensembled_model_3, model8], ['Random Forest', 'Gradient Boosting', 'Ensemble', 'Bagging Classifier']):
    scores = cross_val_score(model, features, target, cv = 5, scoring = 'accuracy')
    Accuracy_2.append(scores.mean())
    Name_2.append(model.__class__.__name__)
    print("Accuracy: %f of model %s" % (scores.mean(),label))
```

    Accuracy: 0.835109 of model Random Forest
    Accuracy: 0.824456 of model Gradient Boosting
    Accuracy: 0.837027 of model Ensemble
    Accuracy: 0.813248 of model Bagging Classifier
    


```python
from sklearn.metrics import accuracy_score
classifers = [model3,model4,model8]
out_sample_accuracy = []
Name_2 = []
for each in classifers:
    fit = each.fit(features,target)
    pred = fit.predict(test.iloc[:,1:242])
    accuracy = accuracy_score(test['Opioid.Prescriber'],pred)
    Name_2.append(each.__class__.__name__)
    out_sample_accuracy.append(accuracy)
```

### Evaluate


```python
# Confusion Matrix
from sklearn.metrics import confusion_matrix
y_actu = test['Opioid.Prescriber']
confusion_matrix(y_actu, pred)
```




    array([[1304,  747],
           [ 503, 2398]])




```python
# Precision-Recall Curve
sklearn.metrics.precision_recall_curve(y_actu, pred, pos_label = None, sample_weight = None)
```




    (array([0.58582391, 0.76248013, 1.        ]),
     array([1.        , 0.82661151, 0.        ]),
     array([0, 1]))




```python
# Precision-Recall Score
from sklearn.metrics import average_precision_score
average_precision = average_precision_score(y_actu, pred)

print('Average precision-recall score: {0:0.2f}'.format(average_precision))
```

    Average precision-recall score: 0.73
    


```python
# Precision-Recall Curve Plot
from sklearn.metrics import precision_recall_curve
import matplotlib.pyplot as plt

precision, recall, _ = precision_recall_curve(y_actu, pred)

plt.step(recall, precision, color = 'b', alpha = 0.2,
         where = 'post')
plt.fill_between(recall, precision, step = 'post', alpha = 0.2,
                 color = 'b')

plt.xlabel('Recall')
plt.ylabel('Precision')
plt.ylim([0.0, 1.05])
plt.xlim([0.0, 1.0])
plt.title('2-class Precision-Recall curve: AP={0:0.2f}'.format(
          average_precision))
```




    Text(0.5, 1.0, '2-class Precision-Recall curve: AP=0.73')




![png](output_62_1.png)


After running various classifiers, we find that Random Forest, Gradient Boosting and our Ensemble models had the best performance comparatively. This means that if we were to build a larger project, we could focus on these particular classifiers, building upon them to help predict opioid prescribers (given more years of data). 

The precision-recall curve can help us determine if we were successful enough. For the unfamiliar precision-recall scores represent a balance between high recall and high precision relating to a low false positive rate and a low false negative rate respectively. When evaluating, you have four outcomes: true positive, true negative, false positive and false negative. Depending on the project, you would aim for different balances, but ideally you want everything to be as accurate, or true, as possible. In the graph above, the y axis is the precision and the x axis is recall. If the graph went straight across the middle, that would be a random-like output. Below it would be poor performance and above it would be a more accurate and better quality performance. If it were 100 it would be a perfect classifier. Because the classifier above is at 0.84 we can feel confident that our precision was good. Now I challenge you to go improve it!
