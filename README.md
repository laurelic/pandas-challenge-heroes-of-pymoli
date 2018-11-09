
### Heroes Of Pymoli Data Analysis

![pymoli](Resources/max-felner-370157-unsplash.jpg)

Photo by [Max Felner](https://unsplash.com/photos/_yrCYuCjMYo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Top Line Stats
* Among 576 customers, we have sold 780 items worth \$2.4k based on an offering of 183 products. (refer to 'Purchasing Analysis - Total')
* AUR (average unit retail) hovering at $3.05

Gender Demographics
* We are seeing a distinct gender trend, with male-identifying customers making up 84.03% of the customer base.
* Though the customer base is predominantly male-idenftifying, we are seeing more price-resistance from this demographic. They are spending 9% less than non-disclosed customers and 5.6% less than female-identifying customers. They are also spending less than our current AUR of \$3.05. Viewed in the light of these trend, there is an argument to be made for working toward growing the non-male customer base.
* These data indicate that each gender is equally likely to make purchases, as their contribution of purchases aligns closely with their portion of the customer pool. It will be interesting to see if we see more distortion as business grows: Female customers: 14.06% of players, 14.5% of purchases, Male customers: 84.03% of players, 83.5% of purchases, Other/Non-Disclosed customers: 1.91% of players, 1.92% of purchases.

    
Age Demographics
* The customer base is concentrated primarily between ages 20-24.
* Because the 20-24 demographic makes up such a large portion of the customer base, their purchases essentially define the AUR \(\$3.05). The ages demonstrating greater purchasing power are those <10 years-old \(average purchase \$3.35), and ages 30-34 \(average purchase \$3.60\)\.
* Viewed through a \% to total lens, the 20-24 age group is demonstrating a greater inclination to purchase. Their % of purchases versus total sales exceed the % of the customer base they make up: 46.8% of sales \$ versus 44.8% of customer base.
* Based on these trends, it seems advisable to concentrate marketing to the 20-24 demographic. Not only are they the greatest contributor to total business and largest demographic, they are trending to be the most inclined to purchase. As such, they have the greatest impact on AUR.


Top Spenders
* When viewing the customers who have spent the most in total, we see that they are not price resistant, as each of these customers purchase above AUR on average.


Best Selling Items
* Our top selling item \(both in number of units sold and \$ volume\) is the 'Oathbreaker, Last Hope of the Breaking Storm.' With 12 units sold and a price point of \$4.23, this item is key to keeping AUR and volume high.
* The #5 selling item, 'Pursuit, Cudgel of Necromancy', has a low price point of $1.02. All of the items ranking above it are above AUR. Considering its competitive price point, it is suprising to not see it rank higher. Will look forward to seeing how this trend plays out.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)
purchase_data.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count


```python
#count unique SNs to assess the number of active players
user_count = purchase_data['SN'].nunique()
players_count = pd.DataFrame({'Total Players': user_count}, index=[0])
players_count
```




<div>
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
#summarize top line information by assessing the related columns, store in DataFrame
item_offer = purchase_data['Item ID'].nunique()
average_price = purchase_data['Price'].mean()
purchase_count = purchase_data['Purchase ID'].count()
total_purchases = purchase_data['Price'].sum()

#generate DataFrame with top line stats
purchase_summary = pd.DataFrame({'Number of Unique Items': item_offer, 
                                 'Average Purchase Price': average_price,
                                'Total Number of Purchases': purchase_count, 
                                 'Total Revenue': total_purchases}, index=[0], columns=['Number of Unique Items', 
                                                                                        'Average Purchase Price', 
                                                                                        'Total Number of Purchases', 
                                                                                        'Total Revenue'])
#set formatting
purchase_summary = purchase_summary.style.format({'Average Purchase Price': '${:,.2f}', 'Total Revenue': '${:,.2f}'})
purchase_summary
```

<table id="T_a249c15e_8497_11e8_a94a_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a249c15e_8497_11e8_a94a_34f39af3d017level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_a249c15e_8497_11e8_a94a_34f39af3d017row0_col0" class="data row0 col0" >183</td> 
        <td id="T_a249c15e_8497_11e8_a94a_34f39af3d017row0_col1" class="data row0 col1" >$3.05</td> 
        <td id="T_a249c15e_8497_11e8_a94a_34f39af3d017row0_col2" class="data row0 col2" >780</td> 
        <td id="T_a249c15e_8497_11e8_a94a_34f39af3d017row0_col3" class="data row0 col3" >$2,379.77</td> 
    </tr></tbody> 
</table> 



## Gender Demographics


```python
#create DataFrame containing user counts by gender
gender_demo = pd.DataFrame(purchase_data.groupby('Gender').SN.nunique())

#rename 'SN' column to 'User Count'
gender_demo = gender_demo.rename(index=str, columns={'SN': 'Player Count'})

#add % to totals in a new columns
gender_demo['Percentage of Players'] = gender_demo['Player Count'] / sum(gender_demo['Player Count'])*100

#format new column with %
gender_demo = gender_demo.style.format({'Percentage of Players': '{:.2f}%'})
gender_demo
```




<table id="T_a2ed0506_8497_11e8_94eb_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Player Count</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a2ed0506_8497_11e8_94eb_34f39af3d017level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row0_col0" class="data row0 col0" >81</td> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row0_col1" class="data row0 col1" >14.06%</td> 
    </tr>    <tr> 
        <th id="T_a2ed0506_8497_11e8_94eb_34f39af3d017level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row1_col0" class="data row1 col0" >484</td> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row1_col1" class="data row1 col1" >84.03%</td> 
    </tr>    <tr> 
        <th id="T_a2ed0506_8497_11e8_94eb_34f39af3d017level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row2_col0" class="data row2 col0" >11</td> 
        <td id="T_a2ed0506_8497_11e8_94eb_34f39af3d017row2_col1" class="data row2 col1" >1.91%</td> 
    </tr></tbody> 
</table> 




## Purchasing Analysis (Gender)


```python
#create DataFrame to store purchase stats by gender using the 'Purchase ID' and 'Price' columns
gender_stats = pd.DataFrame(purchase_data.groupby('Gender').agg({'Purchase ID':'count', 'Price': {'Total Purchase Value':'sum', 'Average Purchase Price':'mean'}}))

#collapse the multiple indexes to create a cleanly labeled data frame
gender_stats.columns = gender_stats.columns.get_level_values(1) 

#rename the columns to clarify what they are
gender_stats = gender_stats.rename(index=str, columns={'count': 'Purchase Count'})

#add a column to store the average purchase value
gender_stats['Normalized Totals'] = gender_stats['Total Purchase Value']/gender_stats['Purchase Count']

#arrange columns accordingly
gender_stats = gender_stats[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]

#format the 'Total Purchase Value', 'Average Purchase Price', and 'Normalized Totals' columns to display as currency
gender_stats = gender_stats.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}', 'Normalized Totals':'${:.2f}'})
gender_stats
```
    




<table id="T_a39e7d06_8497_11e8_a37b_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a39e7d06_8497_11e8_a37b_34f39af3d017level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row0_col0" class="data row0 col0" >113</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row0_col1" class="data row0 col1" >$3.20</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row0_col2" class="data row0 col2" >$361.94</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row0_col3" class="data row0 col3" >$3.20</td> 
    </tr>    <tr> 
        <th id="T_a39e7d06_8497_11e8_a37b_34f39af3d017level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row1_col0" class="data row1 col0" >652</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row1_col1" class="data row1 col1" >$3.02</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row1_col2" class="data row1 col2" >$1967.64</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row1_col3" class="data row1 col3" >$3.02</td> 
    </tr>    <tr> 
        <th id="T_a39e7d06_8497_11e8_a37b_34f39af3d017level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row2_col0" class="data row2 col0" >15</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row2_col1" class="data row2 col1" >$3.35</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row2_col2" class="data row2 col2" >$50.19</td> 
        <td id="T_a39e7d06_8497_11e8_a37b_34f39af3d017row2_col3" class="data row2 col3" >$3.35</td> 
    </tr></tbody> 
</table> 



## Age Demographics


```python
# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#categorize the players according to the age bin
purchase_data['Age'] = pd.cut(purchase_data['Age'], age_bins, labels=group_names)

#create a DataFrame to store grouped by top-line stats based on age
age_demo = pd.DataFrame(purchase_data.groupby('Age').SN.nunique())

#rename 'SN' column to 'User Count'
age_demo = age_demo.rename(index=str, columns={'SN': 'Player Count'})

#add % to totals in a new columns
age_demo['Percentage of Players'] = age_demo['Player Count'] / sum(age_demo['Player Count'])*100

#format new column with %
age_demo = age_demo.style.format({'Percentage of Players': '{:.2f}%'})
age_demo
```




<table id="T_a4513f54_8497_11e8_9ed3_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Player Count</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row0_col0" class="data row0 col0" >17</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row0_col1" class="data row0 col1" >2.95%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row1_col0" class="data row1 col0" >22</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row1_col1" class="data row1 col1" >3.82%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row2_col0" class="data row2 col0" >107</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row2_col1" class="data row2 col1" >18.58%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row3_col0" class="data row3 col0" >258</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row3_col1" class="data row3 col1" >44.79%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row4_col0" class="data row4 col0" >77</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row4_col1" class="data row4 col1" >13.37%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row5_col0" class="data row5 col0" >52</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row5_col1" class="data row5 col1" >9.03%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row6_col0" class="data row6 col0" >31</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row6_col1" class="data row6 col1" >5.38%</td> 
    </tr>    <tr> 
        <th id="T_a4513f54_8497_11e8_9ed3_34f39af3d017level0_row7" class="row_heading level0 row7" >40+</th> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row7_col0" class="data row7 col0" >12</td> 
        <td id="T_a4513f54_8497_11e8_9ed3_34f39af3d017row7_col1" class="data row7 col1" >2.08%</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis (Age)


```python
#create DataFrame to store purchase stats by age using the 'Purchase ID' and 'Price' columns
age_stats = pd.DataFrame(purchase_data.groupby('Age').agg({'Purchase ID':'count', 'Price': {'Total Purchase Value':'sum', 'Average Purchase Price':'mean'}}))

#collapse the multiple indexes to create a cleanly labeled data frame
age_stats.columns = age_stats.columns.get_level_values(1) 

#rename the columns to clarify what they are
age_stats = age_stats.rename(index=str, columns={'count': 'Purchase Count'})

#add a column to store the average purchase value
age_stats['Normalized Totals'] = age_stats['Total Purchase Value']/age_stats['Purchase Count']

#arrange columns accordingly
age_stats = age_stats[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]

#format the 'Total Purchase Value', 'Average Purchase Price', and 'Normalized Totals' columns to display as currency
age_stats = age_stats.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}', 'Normalized Totals':'${:.2f}'})
age_stats
```
   
 
<table id="T_a4fd998a_8497_11e8_b83d_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row0_col0" class="data row0 col0" >23</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row0_col1" class="data row0 col1" >$3.35</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row0_col2" class="data row0 col2" >$77.13</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row0_col3" class="data row0 col3" >$3.35</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row1_col0" class="data row1 col0" >28</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row1_col1" class="data row1 col1" >$2.96</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row1_col2" class="data row1 col2" >$82.78</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row1_col3" class="data row1 col3" >$2.96</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row2_col0" class="data row2 col0" >136</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row2_col1" class="data row2 col1" >$3.04</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row2_col2" class="data row2 col2" >$412.89</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row2_col3" class="data row2 col3" >$3.04</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row3_col0" class="data row3 col0" >365</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row3_col1" class="data row3 col1" >$3.05</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row3_col2" class="data row3 col2" >$1114.06</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row3_col3" class="data row3 col3" >$3.05</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row4_col0" class="data row4 col0" >101</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row4_col1" class="data row4 col1" >$2.90</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row4_col2" class="data row4 col2" >$293.00</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row4_col3" class="data row4 col3" >$2.90</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row5_col0" class="data row5 col0" >73</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row5_col1" class="data row5 col1" >$2.93</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row5_col2" class="data row5 col2" >$214.00</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row5_col3" class="data row5 col3" >$2.93</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row6_col0" class="data row6 col0" >41</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row6_col1" class="data row6 col1" >$3.60</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row6_col2" class="data row6 col2" >$147.67</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row6_col3" class="data row6 col3" >$3.60</td> 
    </tr>    <tr> 
        <th id="T_a4fd998a_8497_11e8_b83d_34f39af3d017level0_row7" class="row_heading level0 row7" >40+</th> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row7_col0" class="data row7 col0" >13</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row7_col1" class="data row7 col1" >$2.94</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row7_col2" class="data row7 col2" >$38.24</td> 
        <td id="T_a4fd998a_8497_11e8_b83d_34f39af3d017row7_col3" class="data row7 col3" >$2.94</td> 
    </tr></tbody> 
</table> 



## Top Spenders


```python
#identify top 5 spenders by grouping purchases by 'SN'
spend_summ = pd.DataFrame(purchase_data.groupby('SN').agg({'Purchase ID':'count', 'Price': {'Total Purchase Value':'sum', 'Average Purchase Price':'mean'}}))

#collapse the multiple indexes to create a cleanly labeled data frame
spend_summ.columns = spend_summ.columns.get_level_values(1) 

#rename the columns to clarify what they are
spend_summ = spend_summ.rename(index=str, columns={'count': 'Purchase Count'})

#sort the 'Total Purchase Value' column in descending order to identify top spenders
spend_summ = spend_summ.sort_values(by='Total Purchase Value', ascending=False)

#slice the DataFrame so that only the top 5 rows populate
spend_summ = spend_summ[0:5]

#convert formatting to currency
spend_summ = spend_summ.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}'})
spend_summ
```
 
<table id="T_a59ccc36_8497_11e8_ae38_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a59ccc36_8497_11e8_ae38_34f39af3d017level0_row0" class="row_heading level0 row0" >Lisosia93</th> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row0_col0" class="data row0 col0" >5</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row0_col1" class="data row0 col1" >$18.96</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row0_col2" class="data row0 col2" >$3.79</td> 
    </tr>    <tr> 
        <th id="T_a59ccc36_8497_11e8_ae38_34f39af3d017level0_row1" class="row_heading level0 row1" >Idastidru52</th> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row1_col0" class="data row1 col0" >4</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row1_col1" class="data row1 col1" >$15.45</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row1_col2" class="data row1 col2" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_a59ccc36_8497_11e8_ae38_34f39af3d017level0_row2" class="row_heading level0 row2" >Chamjask73</th> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row2_col0" class="data row2 col0" >3</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row2_col1" class="data row2 col1" >$13.83</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row2_col2" class="data row2 col2" >$4.61</td> 
    </tr>    <tr> 
        <th id="T_a59ccc36_8497_11e8_ae38_34f39af3d017level0_row3" class="row_heading level0 row3" >Iral74</th> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row3_col0" class="data row3 col0" >4</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row3_col1" class="data row3 col1" >$13.62</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row3_col2" class="data row3 col2" >$3.40</td> 
    </tr>    <tr> 
        <th id="T_a59ccc36_8497_11e8_ae38_34f39af3d017level0_row4" class="row_heading level0 row4" >Iskadarya95</th> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row4_col0" class="data row4 col0" >3</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row4_col1" class="data row4 col1" >$13.10</td> 
        <td id="T_a59ccc36_8497_11e8_ae38_34f39af3d017row4_col2" class="data row4 col2" >$4.37</td> 
    </tr></tbody> 
</table> 



## Most Popular Items


```python
#track most popular items by number of purchases by grouping purchase data according to item id and name
item_data = purchase_data[['Item ID', 'Item Name', 'Price']]

#group the data by item number and name to summarize purchases accordingly
top_purchases = pd.DataFrame(item_data.groupby(['Item ID', 'Item Name']).agg({'Price':['count', 'sum']}))

#colapse the column headers
top_purchases.columns = top_purchases.columns.get_level_values(1)

#add a new column that lists the original price
top_purchases['Item Price'] = top_purchases['sum']/top_purchases['count']

#sort columns
top_purchases = top_purchases[['count', 'Item Price', 'sum']]

#rename columns to reflect the data they are conveying
top_purchases = top_purchases.rename(index=str, columns={'count': 'Purchase Count', 'sum': 'Total Purchase Value'})

#sort the DataFrame according to 'Purchase Count' to identify most popular items
top_purchases = top_purchases.sort_values(by='Purchase Count', ascending=False)

#display top 5 items
top_5_purchases = top_purchases[0:5]

#convert formatting to currency
top_5_purchases = top_5_purchases.style.format({'Total Purchase Value': '${:.2f}', 'Item Price': '${:.2f}'})

top_5_purchases
```

<table id="T_a63b5586_8497_11e8_8318_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level0_row0" class="row_heading level0 row0" >178</th> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level1_row0" class="row_heading level1 row0" >Oathbreaker, Last Hope of the Breaking Storm</th> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row0_col0" class="data row0 col0" >12</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row0_col1" class="data row0 col1" >$4.23</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row0_col2" class="data row0 col2" >$50.76</td> 
    </tr>    <tr> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level0_row1" class="row_heading level0 row1" >145</th> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level1_row1" class="row_heading level1 row1" >Fiery Glass Crusader</th> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row1_col0" class="data row1 col0" >9</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row1_col1" class="data row1 col1" >$4.58</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row1_col2" class="data row1 col2" >$41.22</td> 
    </tr>    <tr> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level0_row2" class="row_heading level0 row2" >108</th> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level1_row2" class="row_heading level1 row2" >Extraction, Quickblade Of Trembling Hands</th> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row2_col0" class="data row2 col0" >9</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row2_col1" class="data row2 col1" >$3.53</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row2_col2" class="data row2 col2" >$31.77</td> 
    </tr>    <tr> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level0_row3" class="row_heading level0 row3" >82</th> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level1_row3" class="row_heading level1 row3" >Nirvana</th> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row3_col0" class="data row3 col0" >9</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row3_col1" class="data row3 col1" >$4.90</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row3_col2" class="data row3 col2" >$44.10</td> 
    </tr>    <tr> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level0_row4" class="row_heading level0 row4" >19</th> 
        <th id="T_a63b5586_8497_11e8_8318_34f39af3d017level1_row4" class="row_heading level1 row4" >Pursuit, Cudgel of Necromancy</th> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row4_col0" class="data row4 col0" >8</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row4_col1" class="data row4 col1" >$1.02</td> 
        <td id="T_a63b5586_8497_11e8_8318_34f39af3d017row4_col2" class="data row4 col2" >$8.16</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items


```python
#use the top purchases data in the previous exercise to identify top volume items
top_purchases = top_purchases.sort_values(by='Total Purchase Value', ascending=False)

#display top 5 items
top_5_volume = top_purchases[0:5]

#convert formatting to currency
top_5_volume = top_5_volume.style.format({'Total Purchase Value': '${:.2f}', 'Item Price': '${:.2f}'})

top_5_volume
```
 
<table id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level0_row0" class="row_heading level0 row0" >178</th> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level1_row0" class="row_heading level1 row0" >Oathbreaker, Last Hope of the Breaking Storm</th> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row0_col0" class="data row0 col0" >12</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row0_col1" class="data row0 col1" >$4.23</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row0_col2" class="data row0 col2" >$50.76</td> 
    </tr>    <tr> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level0_row1" class="row_heading level0 row1" >82</th> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level1_row1" class="row_heading level1 row1" >Nirvana</th> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row1_col0" class="data row1 col0" >9</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row1_col1" class="data row1 col1" >$4.90</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row1_col2" class="data row1 col2" >$44.10</td> 
    </tr>    <tr> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level0_row2" class="row_heading level0 row2" >145</th> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level1_row2" class="row_heading level1 row2" >Fiery Glass Crusader</th> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row2_col0" class="data row2 col0" >9</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row2_col1" class="data row2 col1" >$4.58</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row2_col2" class="data row2 col2" >$41.22</td> 
    </tr>    <tr> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level0_row3" class="row_heading level0 row3" >92</th> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level1_row3" class="row_heading level1 row3" >Final Critic</th> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row3_col0" class="data row3 col0" >8</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row3_col1" class="data row3 col1" >$4.88</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row3_col2" class="data row3 col2" >$39.04</td> 
    </tr>    <tr> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level0_row4" class="row_heading level0 row4" >103</th> 
        <th id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017level1_row4" class="row_heading level1 row4" >Singed Scalpel</th> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row4_col0" class="data row4 col0" >8</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row4_col1" class="data row4 col1" >$4.35</td> 
        <td id="T_a6bfd3f4_8497_11e8_a4e6_34f39af3d017row4_col2" class="data row4 col2" >$34.80</td> 
    </tr></tbody> 
</table> 


