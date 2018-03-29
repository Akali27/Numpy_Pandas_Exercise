

```python
import pandas as pd
import numpy as np
```


```python
jsonfile = "C:/Users/RogStrix/Heroes-of-Pymoli/purchase_data.json"
```


```python
purchase_data_df = pd.read_json(jsonfile)
purchase_data_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player Count
total_players = purchase_data_df['SN'].nunique()
total_players
```




    573




```python
#Number of Unique Items
total_items = purchase_data_df['Item ID'].nunique()
total_items
```




    183




```python
#Average Purchase Price
average_price = purchase_data_df['Price'].mean()
average_price
```




    2.931192307692303




```python
#Total Number of Purchases
total_purchases = len(purchase_data_df)
total_purchases 
```




    780




```python
#Total Revenue
total_revenue = purchase_data_df['Price'].sum()
total_revenue
```




    2286.3299999999963




```python
# Gender Demographics
gender_df = purchase_data_df[['Item ID', "Gender"]].groupby(by = "Gender").count()
gender_df.rename(columns = {"Item ID": "Purchase Count"}, inplace = True)


gender_unique_count = purchase_data_df.drop_duplicates(['SN'], keep = "last")
gender_df['Unique Count'] = gender_unique_count[['SN', "Gender"]].groupby(by = "Gender").count()
gender_df
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
      <th>Purchase Count</th>
      <th>Unique Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)
males_count = gender_df.loc['Male'][0]
males_percent = males_count / total_purchases
females_count = gender_df.loc['Female'][0]
females_percent = females_count / total_purchases
other_count = gender_df.loc['Other / Non-Disclosed'][0]
other_percent = other_count / total_purchases

males_count 

```




    633




```python
males_percent
```




    0.81153846153846154




```python
females_count
```




    136




```python
females_percent
```




    0.17435897435897435




```python
other_count
```




    11




```python
other_percent
```




    0.014102564102564103




```python
my_dict = {"Purchase Count": {"males": males_count, "females": females_count, "others": other_count}, 
           "Percent": {"females": females_percent, "males": males_percent, "others": other_count}}
```


```python
gender_demographics_df = pd.DataFrame(data = my_dict)
gender_demographics_df.head()
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
      <th>Percent</th>
      <th>Purchase Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>females</th>
      <td>0.174359</td>
      <td>136</td>
    </tr>
    <tr>
      <th>males</th>
      <td>0.811538</td>
      <td>633</td>
    </tr>
    <tr>
      <th>others</th>
      <td>11.000000</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
gender_metrics = pd.DataFrame(index = gender_df.index)
gender_metrics['Purchase Count'] = gender_df['Purchase Count']
gender_metrics['Average Purchase Price'] = purchase_data_df.groupby(by = "Gender").mean()['Price']
gender_metrics['Total Purchase Value'] = purchase_data_df.groupby(by = "Gender").sum()['Price']
gender_metrics['Normalized Average'] = gender_metrics['Total Purchase Value'] / gender_df["Unique Count"]
gender_metrics
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Average</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.815515</td>
      <td>382.91</td>
      <td>3.829100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.950521</td>
      <td>1867.68</td>
      <td>4.016516</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>35.74</td>
      <td>4.467500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
bins = np.arange(0, purchase_data_df['Age'].max(), 4)
labels = ["0-4", "4-8", "8-12", "12-16", "16-20", "20-24", "28-32", "32-36", "36-40", "40-44", "44+"]

purchase_data_df['Age Group'] = pd.cut(purchase_data_df['Age'], bins = bins, labels = labels)

age_demo_df = purchase_data_df[['Age Group', 'Price']].groupby('Age Group').count()
age_demo_df.rename(columns = {"Price": "Purchase Count"}, inplace = True)
age_demo_df.head()

age_demo_df['Total Purchase Value'] = purchase_data_df[['Age Group', 'Price']].groupby('Age Group').sum()
age_unique_count = purchase_data_df.drop_duplicates(['SN'], keep = "last")
age_demo_df['Unique Count'] = age_unique_count[['SN', "Age Group"]].groupby(by = "Age Group").count()
age_demo_df['Average Purchase Price'] = age_demo_df['Total Purchase Value'] / age_demo_df['Purchase Count']
age_demo_df['Normalized Average'] = age_demo_df['Total Purchase Value'] / age_demo_df['Unique Count']
age_demo_df
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
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Unique Count</th>
      <th>Average Purchase Price</th>
      <th>Normalized Average</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0-4</th>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4-8</th>
      <td>22</td>
      <td>61.34</td>
      <td>13</td>
      <td>2.788182</td>
      <td>4.718462</td>
    </tr>
    <tr>
      <th>8-12</th>
      <td>24</td>
      <td>81.25</td>
      <td>18</td>
      <td>3.385417</td>
      <td>4.513889</td>
    </tr>
    <tr>
      <th>12-16</th>
      <td>87</td>
      <td>238.89</td>
      <td>64</td>
      <td>2.745862</td>
      <td>3.732656</td>
    </tr>
    <tr>
      <th>16-20</th>
      <td>161</td>
      <td>468.03</td>
      <td>120</td>
      <td>2.907019</td>
      <td>3.900250</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>238</td>
      <td>696.09</td>
      <td>186</td>
      <td>2.924748</td>
      <td>3.742419</td>
    </tr>
    <tr>
      <th>28-32</th>
      <td>104</td>
      <td>309.37</td>
      <td>75</td>
      <td>2.974712</td>
      <td>4.124933</td>
    </tr>
    <tr>
      <th>32-36</th>
      <td>66</td>
      <td>202.09</td>
      <td>44</td>
      <td>3.061970</td>
      <td>4.592955</td>
    </tr>
    <tr>
      <th>36-40</th>
      <td>38</td>
      <td>113.28</td>
      <td>28</td>
      <td>2.981053</td>
      <td>4.045714</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>37</td>
      <td>107.35</td>
      <td>22</td>
      <td>2.901351</td>
      <td>4.879545</td>
    </tr>
    <tr>
      <th>44+</th>
      <td>2</td>
      <td>5.92</td>
      <td>2</td>
      <td>2.960000</td>
      <td>2.960000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders
players_df = purchase_data_df[['SN', "Price"]].groupby('SN').count()
players_df.rename(columns = {"Price": "Purchase Count"}, inplace = True)
players_df.head()
players_df['Total Purchase Value'] = purchase_data_df[['SN', 'Price']].groupby('SN').sum()
players_df.head()
players_df['Average Purchase Price'] = players_df['Total Purchase Value'] / players_df['Purchase Count']
players_df.sort_values(by = "Total Purchase Value", ascending = False, inplace = True)
top_5_players = players_df.iloc[0:5 , :]
top_5_players
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
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>17.06</td>
      <td>3.412000</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>13.56</td>
      <td>3.390000</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>12.74</td>
      <td>3.185000</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>12.73</td>
      <td>4.243333</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>11.58</td>
      <td>3.860000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items
items_df = purchase_data_df[['Item ID', "Item Name", "Price"]].groupby(['Item ID', 'Item Name']).count()
items_df.rename(columns = {"Price": "Purchase Count"}, inplace = True)
items_df.head()
items_df["Purchase Value"] = purchase_data_df[['Item ID', "Item Name", "Price"]].groupby(['Item ID', 'Item Name']).sum()
items_df["Average Purchase Value"] = items_df['Purchase Value'] / items_df['Purchase Count']
items_df.sort_values(by = "Purchase Value", inplace = True, ascending = False)
most_popular_items_by_value = items_df.iloc[ :5, :]
most_popular_items_by_value
items_df.sort_values(by = "Purchase Count", inplace = True, ascending = False)
most_popular_items_by_count = items_df.iloc[ :5, :]
most_popular_items_by_count
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
      <th></th>
      <th>Purchase Count</th>
      <th>Purchase Value</th>
      <th>Average Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>25.85</td>
      <td>2.35</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>24.53</td>
      <td>2.23</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>37.26</td>
      <td>4.14</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>13.41</td>
      <td>1.49</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>18.63</td>
      <td>2.07</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
left_frame = purchase_data_df[['Item ID', 'Item Name', 'Price']]
right_frame = purchase_data_df[['Item ID', 'Price']].groupby('Item ID').agg(['sum', 'count']).reset_index()
right_frame.columns = [' '.join(col).strip() for col in right_frame.columns.values]
most_profitable_items = pd.merge(left_frame,right_frame, on="Item ID", how="outer")
most_profitable_items
most_profitable_items = most_profitable_items.drop_duplicates().sort_values("Price sum", ascending = False).head()
most_profitable_items.columns = ['Item ID', 'Item Name', 'Purchase Price', 'Total Purchase Value', 'Purchase Count']
most_profitable_items
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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>246</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>37.26</td>
      <td>9</td>
    </tr>
    <tr>
      <th>411</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>4.25</td>
      <td>29.75</td>
      <td>7</td>
    </tr>
    <tr>
      <th>217</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
      <td>29.70</td>
      <td>6</td>
    </tr>
    <tr>
      <th>390</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>4.87</td>
      <td>29.22</td>
      <td>6</td>
    </tr>
    <tr>
      <th>525</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>3.61</td>
      <td>28.88</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Observed Trends
#1- Ages 20-24 have high spending habits with high transaction frequency 
#2- Females play games much less than males 
#3- Ages 8-12 have the highest average spend 

```
