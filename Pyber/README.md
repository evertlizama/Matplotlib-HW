
Pyber Ride Sharing HW

Observable Trend 1: Per the scatterplot analysis, there are a greater number of rides occurring in urban city type areas with a relatively cheaper price than that of rural and suburban city types. Here you can see an example of supply and demand since there are more rides occurring in an urban area the competition becomes saturated bringing prices down and vice versa for other city types.

Observable Trend 2: The city type that brought in the most fares is attributed to the Urban city type again. Per the scatter plot and pie chart one can come to a conclusion that this would be the city type to bring in the most fares. 

Observable Trend 3: Nearly 80% of drivers are in urban areas. Perhaps the business should pursue other opportunities in the areas where the demand isnt great such as rural areas. This could be a challenge to a business but it can potentially lead to more profitability and job opportunities for others. 


```python
#import dependencies
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

#read csv files
city_data = "raw_data/city_data.csv"
ride_data = "raw_data/ride_data.csv"

city_df = pd.read_csv(city_data)
ride_df = pd.read_csv(ride_data)

#merge files together by finding matching city values from the ride df against city_df
merged_df = pd.merge(ride_df, city_df, how="left", on="city")

```


```python
#show merged dataframe
merged_df.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
      <td>35</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
      <td>55</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
      <td>68</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#filter merged dataframe by type

urban = merged_df[merged_df["type"] == "Urban"]
suburban = merged_df[merged_df["type"] == "Suburban"]
rural = merged_df[merged_df["type"] == "Rural"]

```


```python
#perform calculations by filtered city type 

urban_ride_count = urban.groupby("city").count()["ride_id"]
urban_avg_fare = urban.groupby("city").mean()["fare"]
urban_driver_count = urban.groupby("city").mean()["driver_count"]

suburban_ride_count = suburban.groupby("city").count()["ride_id"]
suburban_avg_fare = suburban.groupby("city").mean()["fare"]
suburban_driver_count = suburban.groupby("city").mean()["driver_count"]

rural_ride_count = rural.groupby("city").count()["ride_id"]
rural_avg_fare = rural.groupby("city").mean()["fare"]
rural_driver_count = rural.groupby("city").mean()["driver_count"]
```


```python
#scatter plot
plt.figure(figsize=(10,10))   

plt.scatter(urban_ride_count, urban_avg_fare, 
            s=10*urban_driver_count, c="coral", 
            edgecolor="black", linewidth=1,
            marker="o", alpha=0.8, label="Urban")

plt.scatter(suburban_ride_count, suburban_avg_fare, 
            s=10*suburban_driver_count, c="skyblue", 
            edgecolor="black", linewidth=1,
            marker="o", alpha=0.8, label="Suburban")

plt.scatter(rural_ride_count, rural_avg_fare, 
            s=10*rural_driver_count, c="gold", 
            edgecolor="black", linewidth=1,
            marker="o", alpha=0.8, label="Rural")
plt.legend(loc="best")
plt.title('Pyber Ride Sharing Data (2018)', fontsize=20)
plt.xlabel('Total Number of Rides per City')
plt.ylabel('Average Fare ($)')
plt.xlim(0,45)
plt.ylim(15,45)
plt.grid()
plt.show()
```


![png](output_6_0.png)



```python
df_citytype = merged_df.groupby(["type"])

city_summary_table = pd.DataFrame({
                           "Total Fares by City Type": df_citytype["fare"].sum()
})
city_summary_table
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
      <th>Total Fares by City Type</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Labels for the sections of our pie chart
labels = ["Urban", "Rural","Suburban"]

# The values of each section of the pie chart
values = [40078.34, 4255.09, 20335.69]

# The colors of each section of the pie chart
colors = ["lightcoral", "gold", "lightskyblue"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0, 0)

plt.pie(values, explode=explode, labels=labels, colors=colors,
       autopct="%1.1f%%", shadow=True, startangle=180)
plt.title('% of Total Fares by City Type', fontsize=10)
plt.axis("equal")
plt.show()

```


![png](output_8_0.png)



```python
df_ridetype = merged_df.groupby(["type"])

rider_summary_table = pd.DataFrame({
                           "Total Rides by City Type": df_ridetype["ride_id"].count()
})

rider_summary_table
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
      <th>Total Rides by City Type</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>657</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Labels for the sections of our pie chart
labels = ["Urban", "Rural","Suburban"]

# The values of each section of the pie chart
values = [1625, 125, 657]

# The colors of each section of the pie chart
colors = ["lightcoral", "gold", "lightskyblue"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0, 0)

plt.pie(values, explode=explode, labels=labels, colors=colors,
       autopct="%1.1f%%", shadow=True, startangle=180)
plt.title('% of Total Rides by City Type', fontsize=10)
plt.axis("equal")
plt.show()
```


![png](output_10_0.png)



```python
urban_driver_count = urban.groupby("city").mean()["driver_count"]
suburban_driver_count = suburban.groupby("city").mean()["driver_count"]
rural_driver_count = rural.groupby("city").mean()["driver_count"]

total_urban_driver = urban_driver_count.sum()
total_suburban_driver = suburban_driver_count.sum()
total_rural_driver = rural_driver_count.sum()

driver_summary_table = pd.DataFrame({
                           "Total urban driver": [total_urban_driver],
                           "Total Surburban driver":[total_suburban_driver],
                           "Total Rural Driver": [total_rural_driver]        
})
driver_summary_table

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
      <th>Total Rural Driver</th>
      <th>Total Surburban driver</th>
      <th>Total urban driver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>104.0</td>
      <td>629.0</td>
      <td>2607.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Labels for the sections of our pie chart
labels = ["Urban", "Rural","Suburban"]

# The values of each section of the pie chart
values = [2607, 104, 629]

# The colors of each section of the pie chart
colors = ["lightcoral", "gold", "lightskyblue"]

# Tells matplotlib to seperate the "Python" section from the others
explode = (0.1, 0, 0)

plt.pie(values, explode=explode, labels=labels, colors=colors,
       autopct="%1.1f%%", shadow=True, startangle=180)
plt.title('% of Drivers by City Type', fontsize=10)
plt.axis("equal")
plt.show()
```


![png](output_12_0.png)

