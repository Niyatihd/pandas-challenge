
HEROES OF PYMOLI DATA ANALYSIS


```python
# Dependencies
import pandas as pd
import os
import numpy as np
#HeroesOfPymoli/purchase_data2.json
```


```python
# Store filepath in a variable
#file_path = input("Please enter file path: ")
file_path = "HeroesOfPymoli/purchase_data.json"
```


```python
file_one = file_path
file_one_df = pd.read_json(file_one)

file_one_df_male = file_one_df.loc[file_one_df["Gender"] == "Male"]
male_unique = len(file_one_df_male["SN"].unique())
male_price = file_one_df_male["Price"].sum()
#file_one_df_male.head()
print("Number of unique male = {} & total price = {}".format(male_unique, male_price))

file_one_df_female = file_one_df.loc[file_one_df["Gender"] == "Female"]
female_unique = len(file_one_df_female["SN"].unique())
female_price = file_one_df_female["Price"].sum()
#file_one_df_female.head()
print("Number of unique female = {} & total price = {}".format(female_unique, female_price))

file_one_df_total = file_one_df["Price"].sum()
print("Total purchase amount = {}".format(file_one_df_total))

file_one_df.head()
```

    Number of unique male = 465 & total price = 1867.6799999999985
    Number of unique female = 100 & total price = 382.90999999999985
    Total purchase amount = 2286.3299999999963





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



TEST CELL (IGNORE)


```python
file_test = file_one_df.loc[file_one_df["SN"] == "Aidaira26"]
#file_test
```

PLAYER COUNT


```python
#check dataframe for empty cells or NAN 
clean_file_one_df = file_one_df.dropna(how="any")
#clean_file_one_df.count()

```


```python
total_players = len(file_one_df['SN'].unique())
total_players_df = pd.DataFrame([{"Total Players": total_players}])
total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



PURCHASING ANALYSIS (TOTAL)


```python
total_unique_items = len(file_one_df['Item Name'].unique())
#total_unique_items
average_price = file_one_df['Price'].mean()
#average_price
total_purchases = len(file_one_df)
#total_purchases
total_revenue = file_one_df['Price'].sum()
purchasing_analysis_df = pd.DataFrame([{"Number of Unique Items":total_unique_items,
                                        "Average Price":average_price,
                                        "Number of Purchases":total_purchases,
                                        "Total Revenue":total_revenue}])


purchasing_analysis_df = purchasing_analysis_df[['Number of Unique Items',
                                                'Average Price',
                                                'Number of Purchases',
                                                'Total Revenue']]
purchasing_analysis_df["Average Price"] = purchasing_analysis_df["Average Price"].map("${0:,.2f}".format)
purchasing_analysis_df["Total Revenue"] = purchasing_analysis_df["Total Revenue"].map("${0:,.2f}".format)
purchasing_analysis_df
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



GENDER DEMOGRAPHICS




```python
demographic_count = file_one_df['Gender'].value_counts()
demographic_count_df = pd.DataFrame(demographic_count)
demographic_count_df = demographic_count_df.rename(columns={"Gender":"Total Count"})
demographic_percent = [((x/total_purchases)*100) for x in demographic_count_df["Total Count"]]

demographic_count_df["Percentage of Players"] = demographic_percent
demographic_count_df["Percentage of Players"] = demographic_count_df["Percentage of Players"].map("{0:,.2f}%".format)
demographic_count_df = demographic_count_df[['Percentage of Players','Total Count']]
demographic_count_df

#demographic_percent = demographic_count["Gender"].map
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



PURCHASING ANALYSIS (GENDER)


```python
# Using GroupBy in order to separate the data into fields according to "Gender" values
grouped_file_one_df = file_one_df.groupby(['Gender'])

purchase_count = pd.DataFrame(grouped_file_one_df["Price"].count())
purchase_count = purchase_count.rename(columns={"Price":"Purchase counts"})

avg_purchase_price = grouped_file_one_df["Price"].mean()
avg_purchase_price = pd.DataFrame(avg_purchase_price)
avg_purchase_price = avg_purchase_price.rename(columns={"Price":"Average Purchase Price"})

total_purchase_value = grouped_file_one_df["Price"].sum()
total_purchase_value = pd.DataFrame(total_purchase_value)
total_purchase_value = total_purchase_value.rename(columns={"Price":"Total Purchase Value"})

purchasing_analysis_df = pd.DataFrame([{"Purchase Count":purchase_count}])

#purchasing_analysis_df
```


```python
# merge_table
dfs = [avg_purchase_price, total_purchase_value]
purchase_analysis = purchase_count.join(dfs)
#purchase_analysis
```


```python
purchase_analysis["Normalized Totals"] = (purchase_analysis["Total Purchase Value"])/(purchase_analysis["Purchase counts"])
purchase_analysis["Total Purchase Value"] = purchase_analysis["Total Purchase Value"].map("${0:,.2f}".format)
purchase_analysis["Average Purchase Price"] = purchase_analysis["Average Purchase Price"].map("${0:,.2f}".format)
purchase_analysis["Normalized Totals"] = purchase_analysis["Normalized Totals"].map("${0:,.2f}".format)
purchase_analysis
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
      <th>Purchase counts</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
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
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$2.82</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$2.95</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$3.25</td>
    </tr>
  </tbody>
</table>
</div>



AGE DEMOGRAPHICS


```python
#Make bins of age groups (<10, 10-14, 15-19, etc.) till the highest range for age in data set.
maximum_age = file_one_df["Age"].max()
print("Highest range for Age in data set is {}".format(maximum_age))
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]

# Create the names for the seven bins
age_groups = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '>40']

# Cut Age and place it into bins
#pd.cut(file_one_df["Age"], bins, labels=age_groups)

# Add new column to the original DF
file_one_df["Age Range"] = pd.cut(file_one_df["Age"], bins, labels=age_groups)
#file_one_df.head()
```

    Highest range for Age in data set is 45



```python
# Using GroupBy in order to separate the data into fields according to "Age Range" values
grouped_file_one_byage_df = file_one_df.groupby(['Age Range'])

total_count_age_df = pd.DataFrame(grouped_file_one_byage_df["Age Range"].count())
total_count_age_df = total_count_age_df.rename(columns={"Age Range":"Total Count"})

total_count_age_df["Percentage of Players"] = (total_count_age_df["Total Count"]/
                                               total_count_age_df["Total Count"].sum())*100
total_count_age_df["Percentage of Players"] = total_count_age_df["Percentage of Players"].map("{0:,.2f}%".format)
total_count_age_df = total_count_age_df[["Percentage of Players", "Total Count"]]
total_count_age_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.59%</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.49%</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.05%</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>43.08%</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.03%</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.21%</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.38%</td>
      <td>42</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>2.18%</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



TOP SPENDERS


```python
user_info_df = file_one_df[["SN", "Price"]]
user_info_grouped = user_info_df.groupby(['SN'])

user_info_grouped_SN_df = pd.DataFrame(user_info_grouped["SN"].count())
user_info_grouped_SN_df = user_info_grouped_SN_df.rename(columns={"SN":"Purchase Count"})
user_info_grouped_price_df = pd.DataFrame(user_info_grouped["Price"].mean())
user_info_grouped_price_df = user_info_grouped_price_df.rename(columns={"Price":"Average Purchase Price"})

# merge_Df
top_spender_df = user_info_grouped_SN_df.join(user_info_grouped_price_df)

# sort Df
top_spender_df = top_spender_df.sort_values(by="Purchase Count", ascending=False)
# Add Total purchase value column
top_spender_df["Total Purchase Value"]=top_spender_df["Purchase Count"]*top_spender_df["Average Purchase Price"]

# Format text in DF
top_spender_df["Average Purchase Price"] = top_spender_df["Average Purchase Price"].map("${0:,.2f}".format)
top_spender_df["Total Purchase Value"] = top_spender_df["Total Purchase Value"].map("${0:,.2f}".format)
top_spender_df.head()

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
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Sondastan54</th>
      <td>4</td>
      <td>$2.56</td>
      <td>$10.24</td>
    </tr>
  </tbody>
</table>
</div>




```python
num1 = file_one_df.loc[file_one_df["SN"] == "Sondastan54"]
num1
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
      <th>75</th>
      <td>31</td>
      <td>Male</td>
      <td>31</td>
      <td>Trickster</td>
      <td>2.07</td>
      <td>Sondastan54</td>
    </tr>
    <tr>
      <th>304</th>
      <td>31</td>
      <td>Male</td>
      <td>173</td>
      <td>Stormfury Longsword</td>
      <td>4.83</td>
      <td>Sondastan54</td>
    </tr>
    <tr>
      <th>420</th>
      <td>31</td>
      <td>Male</td>
      <td>170</td>
      <td>Shadowsteel</td>
      <td>1.98</td>
      <td>Sondastan54</td>
    </tr>
    <tr>
      <th>754</th>
      <td>31</td>
      <td>Male</td>
      <td>104</td>
      <td>Gladiator's Glaive</td>
      <td>1.36</td>
      <td>Sondastan54</td>
    </tr>
  </tbody>
</table>
</div>



MOST POPULAR ITEM


```python
item_info_df = file_one_df[["Item ID", "Item Name", "Price"]]
item_info_grouped = item_info_df.groupby(['Item ID'])
item_info_count_grouped_df = pd.DataFrame(item_info_grouped["Item ID"].count())
item_info_count_grouped_df = item_info_count_grouped_df.rename(columns={"Item ID":"Purchase Count"})
item_info_count_grouped_df = item_info_count_grouped_df.sort_values(by="Purchase Count", ascending=False)

#get iten name & item price as DFs and join to most_popular_df later
item_info_name_df = pd.DataFrame(item_info_df["Item Name"])
item_info_price_df = pd.DataFrame(item_info_df["Price"])

#reset index
item_info_count_grouped_df = item_info_count_grouped_df.reset_index()
item_info_count_grouped_df.head()

#merge DFs
dfs = [item_info_name_df, item_info_price_df]
most_popular_df = item_info_count_grouped_df.join(dfs)
most_popular_df = most_popular_df.rename(columns={"Price":"Item Price"})

# Add Total purchase value column
most_popular_df["Total Purchase Value"]=most_popular_df["Purchase Count"]*most_popular_df["Item Price"]

#format text in DF
most_popular_df["Item Price"] = most_popular_df["Item Price"].map("${0:,.2f}".format)
most_popular_df["Total Purchase Value"] = most_popular_df["Total Purchase Value"].map("${0:,.2f}".format)

most_popular_df.head()
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
      <th>Purchase Count</th>
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>11</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>$3.37</td>
      <td>$37.07</td>
    </tr>
    <tr>
      <th>1</th>
      <td>84</td>
      <td>11</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>$2.32</td>
      <td>$25.52</td>
    </tr>
    <tr>
      <th>2</th>
      <td>31</td>
      <td>9</td>
      <td>Primitive Blade</td>
      <td>$2.46</td>
      <td>$22.14</td>
    </tr>
    <tr>
      <th>3</th>
      <td>175</td>
      <td>9</td>
      <td>Final Critic</td>
      <td>$1.36</td>
      <td>$12.24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>9</td>
      <td>Stormfury Mace</td>
      <td>$1.27</td>
      <td>$11.43</td>
    </tr>
  </tbody>
</table>
</div>



MOST PROFITABLE ITEMS


```python
#get unformatted data by grouping 
most_profitable_df = item_info_count_grouped_df.join(dfs)
most_profitable_df = most_profitable_df.rename(columns={"Price":"Item Price"})

# Add Total purchase value column
most_profitable_df["Total Purchase Value"]=most_profitable_df["Purchase Count"]*most_profitable_df["Item Price"]

#Sort by total purchase value
most_profitable_df = most_profitable_df.sort_values(by="Total Purchase Value", ascending=False)

#test max total purchase value
max_total_purchase_value = most_profitable_df["Total Purchase Value"].max()
print("max total purchase value = ${}".format(max_total_purchase_value))

#format text in DF
most_profitable_df["Item Price"] = most_profitable_df["Item Price"].map("${0:,.2f}".format)
most_profitable_df["Total Purchase Value"] = most_profitable_df["Total Purchase Value"].map("${0:,.2f}".format)


most_profitable_df.head()
```

    max total purchase value = $37.07





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
      <th>Purchase Count</th>
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>39</td>
      <td>11</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>$3.37</td>
      <td>$37.07</td>
    </tr>
    <tr>
      <th>6</th>
      <td>65</td>
      <td>8</td>
      <td>Mercenary Sabre</td>
      <td>$4.57</td>
      <td>$36.56</td>
    </tr>
    <tr>
      <th>9</th>
      <td>107</td>
      <td>8</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>$4.53</td>
      <td>$36.24</td>
    </tr>
    <tr>
      <th>19</th>
      <td>172</td>
      <td>7</td>
      <td>Winterthorn, Defender of Shifting Worlds</td>
      <td>$4.89</td>
      <td>$34.23</td>
    </tr>
    <tr>
      <th>15</th>
      <td>66</td>
      <td>7</td>
      <td>Blood-Forged Skeletal Spine</td>
      <td>$4.77</td>
      <td>$33.39</td>
    </tr>
  </tbody>
</table>
</div>


