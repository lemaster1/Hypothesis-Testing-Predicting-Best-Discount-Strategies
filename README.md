
## Imports of Dictionaries & Dataset


```python
import scipy.stats as stats

from scipy import stats 
import numpy as np
import matplotlib.pyplot as plt

import pandas as pd
import sqlite3
conn = sqlite3.connect('Northwind_small.sqlite')
cur = conn.cursor()
```

## Business Case Summary

Sales Operations project to review discount pricing strategies.  Extensively researched SQL data tables to evaluate pricing models and strategies for the last year and will deploy statistical modeling for the following:

    (1) Review discount strategies to increase products ordered.
    (2) Access what are the best discount structures & strategies.
    (3) Determine if your customers are price sensitive.

Results from above are part of this technical notebook.  In addition, there's a summary of observations of the data and recommendations of discount pricing levels at the conclusion of this workbook.
 

## Exploring Dataset

### Start researching the data by looking at the data for tables and rows of the SQL database.

#### Table names


```python
cur.execute("""SELECT name FROM sqlite_master WHERE type='table';""")
df_tables = pd.DataFrame(cur.fetchall(), columns=['Table'])
df_tables
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
      <th>Table</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Employee</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Category</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Customer</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Shipper</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Supplier</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Order</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Product</td>
    </tr>
    <tr>
      <td>7</td>
      <td>OrderDetail</td>
    </tr>
    <tr>
      <td>8</td>
      <td>CustomerCustomerDemo</td>
    </tr>
    <tr>
      <td>9</td>
      <td>CustomerDemographic</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Region</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Territory</td>
    </tr>
    <tr>
      <td>12</td>
      <td>EmployeeTerritory</td>
    </tr>
  </tbody>
</table>
</div>



#### Product


```python
cur.execute("""SELECT * FROM Product;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
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
      <th>Id</th>
      <th>ProductName</th>
      <th>SupplierId</th>
      <th>CategoryId</th>
      <th>QuantityPerUnit</th>
      <th>UnitPrice</th>
      <th>UnitsInStock</th>
      <th>UnitsOnOrder</th>
      <th>ReorderLevel</th>
      <th>Discontinued</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>Chai</td>
      <td>1</td>
      <td>1</td>
      <td>10 boxes x 20 bags</td>
      <td>18.00</td>
      <td>39</td>
      <td>0</td>
      <td>10</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>Chang</td>
      <td>1</td>
      <td>1</td>
      <td>24 - 12 oz bottles</td>
      <td>19.00</td>
      <td>17</td>
      <td>40</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Aniseed Syrup</td>
      <td>1</td>
      <td>2</td>
      <td>12 - 550 ml bottles</td>
      <td>10.00</td>
      <td>13</td>
      <td>70</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>Chef Anton's Cajun Seasoning</td>
      <td>2</td>
      <td>2</td>
      <td>48 - 6 oz jars</td>
      <td>22.00</td>
      <td>53</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>Chef Anton's Gumbo Mix</td>
      <td>2</td>
      <td>2</td>
      <td>36 boxes</td>
      <td>21.35</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
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
      <td>72</td>
      <td>73</td>
      <td>Röd Kaviar</td>
      <td>17</td>
      <td>8</td>
      <td>24 - 150 g jars</td>
      <td>15.00</td>
      <td>101</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <td>73</td>
      <td>74</td>
      <td>Longlife Tofu</td>
      <td>4</td>
      <td>7</td>
      <td>5 kg pkg.</td>
      <td>10.00</td>
      <td>4</td>
      <td>20</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <td>74</td>
      <td>75</td>
      <td>Rhönbräu Klosterbier</td>
      <td>12</td>
      <td>1</td>
      <td>24 - 0.5 l bottles</td>
      <td>7.75</td>
      <td>125</td>
      <td>0</td>
      <td>25</td>
      <td>0</td>
    </tr>
    <tr>
      <td>75</td>
      <td>76</td>
      <td>Lakkalikööri</td>
      <td>23</td>
      <td>1</td>
      <td>500 ml</td>
      <td>18.00</td>
      <td>57</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
    </tr>
    <tr>
      <td>76</td>
      <td>77</td>
      <td>Original Frankfurter grüne Soße</td>
      <td>12</td>
      <td>2</td>
      <td>12 boxes</td>
      <td>13.00</td>
      <td>32</td>
      <td>0</td>
      <td>15</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>77 rows × 10 columns</p>
</div>



#### Order


```python
cur.execute("""SELECT * FROM 'Order';""")
order_df = pd.DataFrame(cur.fetchall())
order_df.columns = [x[0] for x in cur.description]
order_df
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10250</td>
      <td>HANAR</td>
      <td>4</td>
      <td>2012-07-08</td>
      <td>2012-08-05</td>
      <td>2012-07-12</td>
      <td>2</td>
      <td>65.83</td>
      <td>Hanari Carnes</td>
      <td>Rua do Paço, 67</td>
      <td>Rio de Janeiro</td>
      <td>South America</td>
      <td>05454-876</td>
      <td>Brazil</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10251</td>
      <td>VICTE</td>
      <td>3</td>
      <td>2012-07-08</td>
      <td>2012-08-05</td>
      <td>2012-07-15</td>
      <td>1</td>
      <td>41.34</td>
      <td>Victuailles en stock</td>
      <td>2, rue du Commerce</td>
      <td>Lyon</td>
      <td>Western Europe</td>
      <td>69004</td>
      <td>France</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10252</td>
      <td>SUPRD</td>
      <td>4</td>
      <td>2012-07-09</td>
      <td>2012-08-06</td>
      <td>2012-07-11</td>
      <td>2</td>
      <td>51.30</td>
      <td>Suprêmes délices</td>
      <td>Boulevard Tirou, 255</td>
      <td>Charleroi</td>
      <td>Western Europe</td>
      <td>B-6000</td>
      <td>Belgium</td>
    </tr>
    <tr>
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
      <td>825</td>
      <td>11073</td>
      <td>PERIC</td>
      <td>2</td>
      <td>2014-05-05</td>
      <td>2014-06-02</td>
      <td>None</td>
      <td>2</td>
      <td>24.95</td>
      <td>Pericles Comidas clásicas</td>
      <td>Calle Dr. Jorge Cash 321</td>
      <td>México D.F.</td>
      <td>Central America</td>
      <td>05033</td>
      <td>Mexico</td>
    </tr>
    <tr>
      <td>826</td>
      <td>11074</td>
      <td>SIMOB</td>
      <td>7</td>
      <td>2014-05-06</td>
      <td>2014-06-03</td>
      <td>None</td>
      <td>2</td>
      <td>18.44</td>
      <td>Simons bistro</td>
      <td>Vinbæltet 34</td>
      <td>Kobenhavn</td>
      <td>Northern Europe</td>
      <td>1734</td>
      <td>Denmark</td>
    </tr>
    <tr>
      <td>827</td>
      <td>11075</td>
      <td>RICSU</td>
      <td>8</td>
      <td>2014-05-06</td>
      <td>2014-06-03</td>
      <td>None</td>
      <td>2</td>
      <td>6.19</td>
      <td>Richter Supermarkt</td>
      <td>Starenweg 5</td>
      <td>Genève</td>
      <td>Western Europe</td>
      <td>1204</td>
      <td>Switzerland</td>
    </tr>
    <tr>
      <td>828</td>
      <td>11076</td>
      <td>BONAP</td>
      <td>4</td>
      <td>2014-05-06</td>
      <td>2014-06-03</td>
      <td>None</td>
      <td>2</td>
      <td>38.28</td>
      <td>Bon app'</td>
      <td>12, rue des Bouchers</td>
      <td>Marseille</td>
      <td>Western Europe</td>
      <td>13008</td>
      <td>France</td>
    </tr>
    <tr>
      <td>829</td>
      <td>11077</td>
      <td>RATTC</td>
      <td>1</td>
      <td>2014-05-06</td>
      <td>2014-06-03</td>
      <td>None</td>
      <td>2</td>
      <td>8.53</td>
      <td>Rattlesnake Canyon Grocery</td>
      <td>2817 Milton Dr.</td>
      <td>Albuquerque</td>
      <td>North America</td>
      <td>87110</td>
      <td>USA</td>
    </tr>
  </tbody>
</table>
<p>830 rows × 14 columns</p>
</div>



#### Customer


```python
cur.execute("""SELECT * FROM Customer;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
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
      <th>Id</th>
      <th>CompanyName</th>
      <th>ContactName</th>
      <th>ContactTitle</th>
      <th>Address</th>
      <th>City</th>
      <th>Region</th>
      <th>PostalCode</th>
      <th>Country</th>
      <th>Phone</th>
      <th>Fax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ALFKI</td>
      <td>Alfreds Futterkiste</td>
      <td>Maria Anders</td>
      <td>Sales Representative</td>
      <td>Obere Str. 57</td>
      <td>Berlin</td>
      <td>Western Europe</td>
      <td>12209</td>
      <td>Germany</td>
      <td>030-0074321</td>
      <td>030-0076545</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ANATR</td>
      <td>Ana Trujillo Emparedados y helados</td>
      <td>Ana Trujillo</td>
      <td>Owner</td>
      <td>Avda. de la Constitución 2222</td>
      <td>México D.F.</td>
      <td>Central America</td>
      <td>05021</td>
      <td>Mexico</td>
      <td>(5) 555-4729</td>
      <td>(5) 555-3745</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ANTON</td>
      <td>Antonio Moreno Taquería</td>
      <td>Antonio Moreno</td>
      <td>Owner</td>
      <td>Mataderos  2312</td>
      <td>México D.F.</td>
      <td>Central America</td>
      <td>05023</td>
      <td>Mexico</td>
      <td>(5) 555-3932</td>
      <td>None</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AROUT</td>
      <td>Around the Horn</td>
      <td>Thomas Hardy</td>
      <td>Sales Representative</td>
      <td>120 Hanover Sq.</td>
      <td>London</td>
      <td>British Isles</td>
      <td>WA1 1DP</td>
      <td>UK</td>
      <td>(171) 555-7788</td>
      <td>(171) 555-6750</td>
    </tr>
    <tr>
      <td>4</td>
      <td>BERGS</td>
      <td>Berglunds snabbköp</td>
      <td>Christina Berglund</td>
      <td>Order Administrator</td>
      <td>Berguvsvägen  8</td>
      <td>Luleå</td>
      <td>Northern Europe</td>
      <td>S-958 22</td>
      <td>Sweden</td>
      <td>0921-12 34 65</td>
      <td>0921-12 34 67</td>
    </tr>
    <tr>
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
      <td>86</td>
      <td>WARTH</td>
      <td>Wartian Herkku</td>
      <td>Pirkko Koskitalo</td>
      <td>Accounting Manager</td>
      <td>Torikatu 38</td>
      <td>Oulu</td>
      <td>Scandinavia</td>
      <td>90110</td>
      <td>Finland</td>
      <td>981-443655</td>
      <td>981-443655</td>
    </tr>
    <tr>
      <td>87</td>
      <td>WELLI</td>
      <td>Wellington Importadora</td>
      <td>Paula Parente</td>
      <td>Sales Manager</td>
      <td>Rua do Mercado, 12</td>
      <td>Resende</td>
      <td>South America</td>
      <td>08737-363</td>
      <td>Brazil</td>
      <td>(14) 555-8122</td>
      <td>None</td>
    </tr>
    <tr>
      <td>88</td>
      <td>WHITC</td>
      <td>White Clover Markets</td>
      <td>Karl Jablonski</td>
      <td>Owner</td>
      <td>305 - 14th Ave. S. Suite 3B</td>
      <td>Seattle</td>
      <td>North America</td>
      <td>98128</td>
      <td>USA</td>
      <td>(206) 555-4112</td>
      <td>(206) 555-4115</td>
    </tr>
    <tr>
      <td>89</td>
      <td>WILMK</td>
      <td>Wilman Kala</td>
      <td>Matti Karttunen</td>
      <td>Owner/Marketing Assistant</td>
      <td>Keskuskatu 45</td>
      <td>Helsinki</td>
      <td>Scandinavia</td>
      <td>21240</td>
      <td>Finland</td>
      <td>90-224 8858</td>
      <td>90-224 8858</td>
    </tr>
    <tr>
      <td>90</td>
      <td>WOLZA</td>
      <td>Wolski  Zajazd</td>
      <td>Zbyszek Piestrzeniewicz</td>
      <td>Owner</td>
      <td>ul. Filtrowa 68</td>
      <td>Warszawa</td>
      <td>Eastern Europe</td>
      <td>01-012</td>
      <td>Poland</td>
      <td>(26) 642-7012</td>
      <td>(26) 642-7012</td>
    </tr>
  </tbody>
</table>
<p>91 rows × 11 columns</p>
</div>



#### OrderDetail


```python
cur.execute("""SELECT * FROM OrderDetail;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 6 columns</p>
</div>



#### (A note about table data not included:  I didn't include employees, territories or suppliers data because not required for analysis.  BTW, the customer demographic table data is without analytical value )

## Hypothesis Testing

### Hypothesis Q1

#### Did Q1 2014 orders totals increase from Q1 2013?  
##### H_A Q1 2014 orders > Q1 2013 orders
##### H_O Q1 2013 orders < Q1 2014 orders

##### Before data science efforts for this hypothesis question wanted to just look at total sales for 2013 & 2014.

##### Rendering SQL tables Orders & OrderDetails to Pandas DataFrame (named the variable df_ood)


```python
cur.execute("""SELECT * 
               FROM 'Order' o
               JOIN OrderDetail od
               ON o.Id = od.OrderId;
               """)
df_ood = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df_ood.columns = [i[0] for i in cur.description]
df_ood.head()
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.0</td>
      <td>12</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.8</td>
      <td>10</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.8</td>
      <td>5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.6</td>
      <td>9</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.4</td>
      <td>40</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_ood.head(1)
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.0</td>
      <td>12</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



##### Creating DataFrames to compare YOY data


```python

```


```python
# Code for adding a column to look at order total (named' column order_total)
df_ood['order_total'] = df_ood['UnitPrice'] * df_ood['Quantity']
```


```python
Q1_2013 = df_ood[(df_ood['OrderDate'] >= '2013-01-01') & (df_ood['OrderDate'] <= '2013-03-31')]
Q1_2013_totals = Q1_2013['order_total']
```


```python
Q1_2013.tail()
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>...</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>641</td>
      <td>10490</td>
      <td>HILAA</td>
      <td>7</td>
      <td>2013-03-31</td>
      <td>2013-04-28</td>
      <td>2013-04-03</td>
      <td>2</td>
      <td>210.19</td>
      <td>HILARION-Abastos</td>
      <td>Carrera 22 con Ave. Carlos Soublette #8-35</td>
      <td>...</td>
      <td>South America</td>
      <td>5022</td>
      <td>Venezuela</td>
      <td>10490/59</td>
      <td>10490</td>
      <td>59</td>
      <td>44.0</td>
      <td>60</td>
      <td>0.00</td>
      <td>2640.0</td>
    </tr>
    <tr>
      <td>642</td>
      <td>10490</td>
      <td>HILAA</td>
      <td>7</td>
      <td>2013-03-31</td>
      <td>2013-04-28</td>
      <td>2013-04-03</td>
      <td>2</td>
      <td>210.19</td>
      <td>HILARION-Abastos</td>
      <td>Carrera 22 con Ave. Carlos Soublette #8-35</td>
      <td>...</td>
      <td>South America</td>
      <td>5022</td>
      <td>Venezuela</td>
      <td>10490/68</td>
      <td>10490</td>
      <td>68</td>
      <td>10.0</td>
      <td>30</td>
      <td>0.00</td>
      <td>300.0</td>
    </tr>
    <tr>
      <td>643</td>
      <td>10490</td>
      <td>HILAA</td>
      <td>7</td>
      <td>2013-03-31</td>
      <td>2013-04-28</td>
      <td>2013-04-03</td>
      <td>2</td>
      <td>210.19</td>
      <td>HILARION-Abastos</td>
      <td>Carrera 22 con Ave. Carlos Soublette #8-35</td>
      <td>...</td>
      <td>South America</td>
      <td>5022</td>
      <td>Venezuela</td>
      <td>10490/75</td>
      <td>10490</td>
      <td>75</td>
      <td>6.2</td>
      <td>36</td>
      <td>0.00</td>
      <td>223.2</td>
    </tr>
    <tr>
      <td>644</td>
      <td>10491</td>
      <td>FURIB</td>
      <td>8</td>
      <td>2013-03-31</td>
      <td>2013-04-28</td>
      <td>2013-04-08</td>
      <td>3</td>
      <td>16.96</td>
      <td>Furia Bacalhau e Frutos do Mar</td>
      <td>Jardim das rosas n. 32</td>
      <td>...</td>
      <td>Southern Europe</td>
      <td>1675</td>
      <td>Portugal</td>
      <td>10491/44</td>
      <td>10491</td>
      <td>44</td>
      <td>15.5</td>
      <td>15</td>
      <td>0.15</td>
      <td>232.5</td>
    </tr>
    <tr>
      <td>645</td>
      <td>10491</td>
      <td>FURIB</td>
      <td>8</td>
      <td>2013-03-31</td>
      <td>2013-04-28</td>
      <td>2013-04-08</td>
      <td>3</td>
      <td>16.96</td>
      <td>Furia Bacalhau e Frutos do Mar</td>
      <td>Jardim das rosas n. 32</td>
      <td>...</td>
      <td>Southern Europe</td>
      <td>1675</td>
      <td>Portugal</td>
      <td>10491/77</td>
      <td>10491</td>
      <td>77</td>
      <td>10.4</td>
      <td>7</td>
      <td>0.15</td>
      <td>72.8</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
df_ood['order_total'] = df_ood['UnitPrice'] * df_ood['Quantity']
```


```python
Q1_2014 = df_ood[(df_ood['OrderDate'] >= '2014-01-01') & (df_ood['OrderDate'] <= '2014-03-31')]

Q1_2014_totals = Q1_2014['order_total']
```


```python
Q1_2014.iloc[:,-1].sum()

```




    315242.12




```python
Q1_2014.tail(1)
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>...</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1915</td>
      <td>10989</td>
      <td>QUEDE</td>
      <td>2</td>
      <td>2014-03-31</td>
      <td>2014-04-28</td>
      <td>2014-04-02</td>
      <td>1</td>
      <td>34.76</td>
      <td>Que Delícia</td>
      <td>Rua da Panificadora, 12</td>
      <td>...</td>
      <td>South America</td>
      <td>02389-673</td>
      <td>Brazil</td>
      <td>10989/41</td>
      <td>10989</td>
      <td>41</td>
      <td>9.65</td>
      <td>4</td>
      <td>0.0</td>
      <td>38.6</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 21 columns</p>
</div>




```python
# Code for adding a column to look at order total (named' column order_total)
df_ood['order_total'] = df_ood['UnitPrice'] * df_ood['Quantity']
```

#### Grouping together customer DataFrame along with order totals


```python
df_ood.groupby(df_ood['CustomerId']).sum().head(50)
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
      <th>Id</th>
      <th>EmployeeId</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
    </tr>
    <tr>
      <th>CustomerId</th>
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
      <td>ALFKI</td>
      <td>129621</td>
      <td>40</td>
      <td>17</td>
      <td>419.60</td>
      <td>129621</td>
      <td>554</td>
      <td>320.85</td>
      <td>174</td>
      <td>1.05</td>
      <td>4596.20</td>
    </tr>
    <tr>
      <td>ANATR</td>
      <td>106954</td>
      <td>42</td>
      <td>24</td>
      <td>306.59</td>
      <td>106954</td>
      <td>402</td>
      <td>215.05</td>
      <td>63</td>
      <td>0.00</td>
      <td>1402.95</td>
    </tr>
    <tr>
      <td>ANTO</td>
      <td>180350</td>
      <td>71</td>
      <td>33</td>
      <td>667.29</td>
      <td>180350</td>
      <td>650</td>
      <td>369.23</td>
      <td>359</td>
      <td>1.00</td>
      <td>7515.35</td>
    </tr>
    <tr>
      <td>AROUT</td>
      <td>320404</td>
      <td>126</td>
      <td>67</td>
      <td>1447.14</td>
      <td>320404</td>
      <td>1371</td>
      <td>575.30</td>
      <td>650</td>
      <td>0.70</td>
      <td>13806.50</td>
    </tr>
    <tr>
      <td>BERGS</td>
      <td>552534</td>
      <td>217</td>
      <td>105</td>
      <td>4835.18</td>
      <td>552534</td>
      <td>2029</td>
      <td>1425.65</td>
      <td>1001</td>
      <td>3.00</td>
      <td>26968.15</td>
    </tr>
    <tr>
      <td>BLAUS</td>
      <td>150911</td>
      <td>97</td>
      <td>34</td>
      <td>351.41</td>
      <td>150911</td>
      <td>565</td>
      <td>347.05</td>
      <td>140</td>
      <td>0.00</td>
      <td>3239.80</td>
    </tr>
    <tr>
      <td>BLONP</td>
      <td>272374</td>
      <td>122</td>
      <td>49</td>
      <td>1980.00</td>
      <td>272374</td>
      <td>1141</td>
      <td>848.75</td>
      <td>666</td>
      <td>0.75</td>
      <td>19088.00</td>
    </tr>
    <tr>
      <td>BOLID</td>
      <td>63550</td>
      <td>29</td>
      <td>11</td>
      <td>444.10</td>
      <td>63550</td>
      <td>234</td>
      <td>209.19</td>
      <td>190</td>
      <td>0.70</td>
      <td>5297.80</td>
    </tr>
    <tr>
      <td>BONAP</td>
      <td>470646</td>
      <td>196</td>
      <td>84</td>
      <td>3952.65</td>
      <td>470646</td>
      <td>1599</td>
      <td>1061.45</td>
      <td>980</td>
      <td>3.20</td>
      <td>23850.95</td>
    </tr>
    <tr>
      <td>BOTTM</td>
      <td>375682</td>
      <td>135</td>
      <td>84</td>
      <td>2171.97</td>
      <td>375682</td>
      <td>1377</td>
      <td>883.60</td>
      <td>956</td>
      <td>2.95</td>
      <td>22607.70</td>
    </tr>
    <tr>
      <td>BSBEV</td>
      <td>233781</td>
      <td>100</td>
      <td>60</td>
      <td>563.15</td>
      <td>233781</td>
      <td>844</td>
      <td>499.20</td>
      <td>293</td>
      <td>0.00</td>
      <td>6089.90</td>
    </tr>
    <tr>
      <td>CACTU</td>
      <td>118846</td>
      <td>71</td>
      <td>24</td>
      <td>158.80</td>
      <td>118846</td>
      <td>528</td>
      <td>197.50</td>
      <td>115</td>
      <td>0.00</td>
      <td>1814.80</td>
    </tr>
    <tr>
      <td>CENTC</td>
      <td>20518</td>
      <td>8</td>
      <td>6</td>
      <td>6.50</td>
      <td>20518</td>
      <td>58</td>
      <td>28.80</td>
      <td>11</td>
      <td>0.00</td>
      <td>100.80</td>
    </tr>
    <tr>
      <td>CHOPS</td>
      <td>234913</td>
      <td>95</td>
      <td>44</td>
      <td>940.44</td>
      <td>234913</td>
      <td>1015</td>
      <td>609.20</td>
      <td>465</td>
      <td>1.30</td>
      <td>12886.30</td>
    </tr>
    <tr>
      <td>COMMI</td>
      <td>105639</td>
      <td>49</td>
      <td>12</td>
      <td>468.84</td>
      <td>105639</td>
      <td>424</td>
      <td>259.15</td>
      <td>133</td>
      <td>0.00</td>
      <td>3810.75</td>
    </tr>
    <tr>
      <td>CONSH</td>
      <td>73925</td>
      <td>42</td>
      <td>12</td>
      <td>116.45</td>
      <td>73925</td>
      <td>146</td>
      <td>190.15</td>
      <td>87</td>
      <td>0.00</td>
      <td>1719.10</td>
    </tr>
    <tr>
      <td>DRACD</td>
      <td>107066</td>
      <td>41</td>
      <td>24</td>
      <td>595.84</td>
      <td>107066</td>
      <td>398</td>
      <td>191.08</td>
      <td>160</td>
      <td>0.00</td>
      <td>3763.21</td>
    </tr>
    <tr>
      <td>DUMO</td>
      <td>95802</td>
      <td>46</td>
      <td>16</td>
      <td>157.61</td>
      <td>95802</td>
      <td>287</td>
      <td>168.65</td>
      <td>80</td>
      <td>0.00</td>
      <td>1615.90</td>
    </tr>
    <tr>
      <td>EASTC</td>
      <td>226763</td>
      <td>105</td>
      <td>38</td>
      <td>2361.77</td>
      <td>226763</td>
      <td>809</td>
      <td>575.72</td>
      <td>569</td>
      <td>0.50</td>
      <td>15033.66</td>
    </tr>
    <tr>
      <td>ERNSH</td>
      <td>1087562</td>
      <td>463</td>
      <td>196</td>
      <td>24536.92</td>
      <td>1087562</td>
      <td>3947</td>
      <td>2666.67</td>
      <td>4543</td>
      <td>6.95</td>
      <td>113236.68</td>
    </tr>
    <tr>
      <td>FAMIA</td>
      <td>199742</td>
      <td>96</td>
      <td>51</td>
      <td>663.39</td>
      <td>199742</td>
      <td>826</td>
      <td>245.14</td>
      <td>357</td>
      <td>1.20</td>
      <td>4438.90</td>
    </tr>
    <tr>
      <td>FOLIG</td>
      <td>170165</td>
      <td>65</td>
      <td>36</td>
      <td>2500.45</td>
      <td>170165</td>
      <td>661</td>
      <td>443.35</td>
      <td>354</td>
      <td>0.00</td>
      <td>11666.90</td>
    </tr>
    <tr>
      <td>FOLKO</td>
      <td>482403</td>
      <td>226</td>
      <td>76</td>
      <td>5310.94</td>
      <td>482403</td>
      <td>2121</td>
      <td>1060.84</td>
      <td>1234</td>
      <td>3.85</td>
      <td>32555.55</td>
    </tr>
    <tr>
      <td>FRANK</td>
      <td>509067</td>
      <td>199</td>
      <td>92</td>
      <td>4931.31</td>
      <td>509067</td>
      <td>2160</td>
      <td>982.98</td>
      <td>1525</td>
      <td>3.15</td>
      <td>28722.71</td>
    </tr>
    <tr>
      <td>FRANR</td>
      <td>64704</td>
      <td>11</td>
      <td>11</td>
      <td>251.36</td>
      <td>64704</td>
      <td>299</td>
      <td>282.59</td>
      <td>69</td>
      <td>0.00</td>
      <td>3172.16</td>
    </tr>
    <tr>
      <td>FRANS</td>
      <td>108327</td>
      <td>26</td>
      <td>12</td>
      <td>145.88</td>
      <td>108327</td>
      <td>457</td>
      <td>244.00</td>
      <td>54</td>
      <td>0.00</td>
      <td>1545.70</td>
    </tr>
    <tr>
      <td>FURIB</td>
      <td>210342</td>
      <td>76</td>
      <td>52</td>
      <td>893.89</td>
      <td>210342</td>
      <td>964</td>
      <td>427.90</td>
      <td>349</td>
      <td>1.95</td>
      <td>7151.55</td>
    </tr>
    <tr>
      <td>GALED</td>
      <td>84895</td>
      <td>37</td>
      <td>14</td>
      <td>68.17</td>
      <td>84895</td>
      <td>420</td>
      <td>156.70</td>
      <td>42</td>
      <td>0.00</td>
      <td>836.70</td>
    </tr>
    <tr>
      <td>GODOS</td>
      <td>280146</td>
      <td>115</td>
      <td>54</td>
      <td>1701.56</td>
      <td>280146</td>
      <td>1141</td>
      <td>712.09</td>
      <td>395</td>
      <td>1.05</td>
      <td>11830.10</td>
    </tr>
    <tr>
      <td>GOURL</td>
      <td>203948</td>
      <td>72</td>
      <td>42</td>
      <td>882.95</td>
      <td>203948</td>
      <td>685</td>
      <td>486.68</td>
      <td>315</td>
      <td>1.30</td>
      <td>8702.23</td>
    </tr>
    <tr>
      <td>GREAL</td>
      <td>235946</td>
      <td>86</td>
      <td>46</td>
      <td>2455.43</td>
      <td>235946</td>
      <td>901</td>
      <td>1091.54</td>
      <td>345</td>
      <td>1.55</td>
      <td>19711.13</td>
    </tr>
    <tr>
      <td>GROSR</td>
      <td>42106</td>
      <td>18</td>
      <td>12</td>
      <td>135.60</td>
      <td>42106</td>
      <td>186</td>
      <td>165.55</td>
      <td>34</td>
      <td>0.00</td>
      <td>1488.70</td>
    </tr>
    <tr>
      <td>HANAR</td>
      <td>342869</td>
      <td>108</td>
      <td>55</td>
      <td>1553.85</td>
      <td>342869</td>
      <td>1364</td>
      <td>1396.10</td>
      <td>839</td>
      <td>2.15</td>
      <td>34101.15</td>
    </tr>
    <tr>
      <td>HILAA</td>
      <td>480253</td>
      <td>233</td>
      <td>82</td>
      <td>3417.38</td>
      <td>480253</td>
      <td>2042</td>
      <td>930.62</td>
      <td>1096</td>
      <td>1.50</td>
      <td>23611.58</td>
    </tr>
    <tr>
      <td>HUNGC</td>
      <td>94228</td>
      <td>30</td>
      <td>15</td>
      <td>302.87</td>
      <td>94228</td>
      <td>340</td>
      <td>205.35</td>
      <td>122</td>
      <td>0.00</td>
      <td>3063.20</td>
    </tr>
    <tr>
      <td>HUNGO</td>
      <td>582184</td>
      <td>278</td>
      <td>115</td>
      <td>7214.49</td>
      <td>582184</td>
      <td>2282</td>
      <td>1719.86</td>
      <td>1684</td>
      <td>6.25</td>
      <td>57317.39</td>
    </tr>
    <tr>
      <td>ISLAT</td>
      <td>244716</td>
      <td>113</td>
      <td>44</td>
      <td>1141.40</td>
      <td>244716</td>
      <td>1088</td>
      <td>498.00</td>
      <td>295</td>
      <td>0.00</td>
      <td>6146.30</td>
    </tr>
    <tr>
      <td>KOENE</td>
      <td>415011</td>
      <td>173</td>
      <td>88</td>
      <td>3033.25</td>
      <td>415011</td>
      <td>1366</td>
      <td>1229.24</td>
      <td>903</td>
      <td>1.55</td>
      <td>31745.75</td>
    </tr>
    <tr>
      <td>LACOR</td>
      <td>120218</td>
      <td>44</td>
      <td>16</td>
      <td>262.45</td>
      <td>120218</td>
      <td>444</td>
      <td>285.03</td>
      <td>83</td>
      <td>0.00</td>
      <td>1992.05</td>
    </tr>
    <tr>
      <td>LAMAI</td>
      <td>328038</td>
      <td>141</td>
      <td>62</td>
      <td>1524.21</td>
      <td>328038</td>
      <td>1321</td>
      <td>655.34</td>
      <td>442</td>
      <td>3.90</td>
      <td>10272.35</td>
    </tr>
    <tr>
      <td>LAUGB</td>
      <td>85155</td>
      <td>19</td>
      <td>24</td>
      <td>28.82</td>
      <td>85155</td>
      <td>325</td>
      <td>71.80</td>
      <td>62</td>
      <td>0.00</td>
      <td>522.50</td>
    </tr>
    <tr>
      <td>LAZYK</td>
      <td>21027</td>
      <td>9</td>
      <td>5</td>
      <td>19.40</td>
      <td>21027</td>
      <td>51</td>
      <td>35.70</td>
      <td>20</td>
      <td>0.00</td>
      <td>357.00</td>
    </tr>
    <tr>
      <td>LEHMS</td>
      <td>413219</td>
      <td>194</td>
      <td>59</td>
      <td>2938.11</td>
      <td>413219</td>
      <td>1549</td>
      <td>1028.19</td>
      <td>794</td>
      <td>3.75</td>
      <td>21282.02</td>
    </tr>
    <tr>
      <td>LETSS</td>
      <td>107437</td>
      <td>50</td>
      <td>20</td>
      <td>546.63</td>
      <td>107437</td>
      <td>472</td>
      <td>229.64</td>
      <td>181</td>
      <td>1.10</td>
      <td>3490.02</td>
    </tr>
    <tr>
      <td>LILAS</td>
      <td>360613</td>
      <td>140</td>
      <td>69</td>
      <td>2214.70</td>
      <td>360613</td>
      <td>1372</td>
      <td>707.14</td>
      <td>836</td>
      <td>3.30</td>
      <td>17825.06</td>
    </tr>
    <tr>
      <td>LINOD</td>
      <td>377604</td>
      <td>135</td>
      <td>62</td>
      <td>2104.76</td>
      <td>377604</td>
      <td>1213</td>
      <td>631.60</td>
      <td>970</td>
      <td>3.10</td>
      <td>17889.55</td>
    </tr>
    <tr>
      <td>LONEP</td>
      <td>149480</td>
      <td>50</td>
      <td>25</td>
      <td>181.25</td>
      <td>149480</td>
      <td>643</td>
      <td>437.70</td>
      <td>134</td>
      <td>0.00</td>
      <td>4258.60</td>
    </tr>
    <tr>
      <td>MAGAA</td>
      <td>222771</td>
      <td>89</td>
      <td>46</td>
      <td>1208.14</td>
      <td>222771</td>
      <td>747</td>
      <td>389.30</td>
      <td>433</td>
      <td>1.05</td>
      <td>7603.85</td>
    </tr>
    <tr>
      <td>MAISD</td>
      <td>183769</td>
      <td>97</td>
      <td>33</td>
      <td>1085.52</td>
      <td>183769</td>
      <td>762</td>
      <td>496.38</td>
      <td>320</td>
      <td>1.00</td>
      <td>10430.58</td>
    </tr>
    <tr>
      <td>MEREP</td>
      <td>336332</td>
      <td>123</td>
      <td>72</td>
      <td>4121.11</td>
      <td>336332</td>
      <td>1273</td>
      <td>952.10</td>
      <td>966</td>
      <td>1.85</td>
      <td>32203.90</td>
    </tr>
  </tbody>
</table>
</div>



##### "Getting" YOY Quarterly totals

##### Total revs of $315,242 for Q1 2014.


```python
Q1_2014.iloc[:,-1].sum()
```




    315242.12



##### Total revs of $147,879 for Q1 2013.


```python
Q1_2013.iloc[:,-1].sum()
```




    147879.90000000002



#### Alright, alright.  Getting back to the hypothesis question:
#### Did Q1 2014 orders increase from Q1 2013? 

#### Do a visual of normality with bar charts with SEM


```python
def plot_2_grps(grp1, name1, grp2, name2, ylabel='grp vales'):
    """Plots a bar chart for each group with standard error of the mean error bars.
    Args:
        grp1 (data): group1 data to plot
        name1 (str): Name to display for grp 1
        grp2 (data): group1 data to plot
        name2 (str): Name to display for grp 2
        ylabel (str): Ylabel
        
    Return:
        fig (Figugre)
        ax (Axis)"""
    fig, ax = plt.subplots()
    plt.bar(name1, height= grp1.mean(), yerr=stats.sem(grp1))
    plt.bar(name2, height= grp2.mean(), yerr=stats.sem(grp2))
    ax.set_ylabel(ylabel, fontdict={'weight': 'bold', 
                                           'size' : 12})
    return fig, ax
```


```python
fig, ax = plot_2_grps(Q1_2013_totals, 'Q1 2013', Q1_2014_totals, 'Q1 2014', "Order Totals ($)" )
#fig
# ax.set_ylabel("Order Totals ($)", fontdict={'weight': 'bold', 
#                                            'size' : 12})
```


![png](output_43_0.png)



```python

```


```python
# NEED TO ADD ORDER TOTALS TO Y AXIS
# plt.bar('Q1 2013', height= Q1_2013_totals.mean(), yerr=stats.sem(Q1_2013_totals))
# plt.bar('Q1 2014', height= Q1_2014_totals.mean(), yerr=stats.sem(Q1_2014_totals))

```


```python
print(stats.normaltest(Q1_2013_totals))
stats.normaltest(Q1_2014_totals)
```

    NormaltestResult(statistic=334.5587852797349, pvalue=2.2463780164969008e-73)





    NormaltestResult(statistic=629.2405735562659, pvalue=2.3022132942131586e-137)



#### Reject Null Hypothesis; therefore data is non-normal.

#### Need to use Mann Whitney stats test because of above.


```python
stats.mannwhitneyu(Q1_2013_totals,Q1_2014_totals)
```




    MannwhitneyuResult(statistic=51455.5, pvalue=0.11521858254931366)



### Q1 Results:  Failed to reject Null Hypothesis that there is no difference order totals.

### Hypothesis Q2

#### Does discount amount have a statistically significant effect on the quantity of a product in an order.  If so, at what levels of discounts.

#### "Getting" the data. All of it is in one table, the OrderDetail table.


```python
cur.execute("""SELECT * FROM OrderDetail;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 6 columns</p>
</div>



#### Groupby discounts column:


```python
grps = df.groupby('Discount').groups 

for grp in grps:
    grps[grp] = df.groupby('Discount').get_group(grp)['Quantity']  # Loop Creating dict
```


```python
type(grps)
```




    dict




```python
grps
```




    {0.0: 0       12
     1       10
     2        5
     3        9
     4       40
             ..
     2147     2
     2148     2
     2151     1
     2153     4
     2154     2
     Name: Quantity, Length: 1317, dtype: int64, 0.01: 2152    2
     Name: Quantity, dtype: int64, 0.02: 2133    1
     2146    3
     Name: Quantity, dtype: int64, 0.03: 2139    1
     2140    2
     2150    2
     Name: Quantity, dtype: int64, 0.04: 2141    1
     Name: Quantity, dtype: int64, 0.05: 8        6
     9       15
     11      40
     12      25
     51      12
             ..
     2116    10
     2123    14
     2134     1
     2137     2
     2144     2
     Name: Quantity, Length: 185, dtype: int64, 0.06: 2149    2
     Name: Quantity, dtype: int64, 0.1: 107     10
     108      3
     115     20
     116     24
     117      2
             ..
     2095    30
     2096    77
     2098    25
     2099     4
     2135     2
     Name: Quantity, Length: 173, dtype: int64, 0.15: 6       35
     7       15
     17      15
     18      21
     48      25
             ..
     2112    20
     2113    30
     2124    10
     2125    30
     2126     2
     Name: Quantity, Length: 157, dtype: int64, 0.2: 29      50
     30      65
     31       6
     40      12
     99      45
             ..
     2069    35
     2071    25
     2091    10
     2092    12
     2130    24
     Name: Quantity, Length: 161, dtype: int64, 0.25: 34      16
     36      15
     37      21
     43      60
     45      60
             ..
     2101     4
     2102    20
     2127    20
     2128    20
     2129    10
     Name: Quantity, Length: 154, dtype: int64}



#### Discount amounts are below.
#### assist with second half of question about the different levels of discounts.


```python
grps.keys()   # Below are the dict keys or the discounts
```




    dict_keys([0.0, 0.01, 0.02, 0.03, 0.04, 0.05, 0.06, 0.1, 0.15, 0.2, 0.25])



#### Testing the null hypothesis that two or more groups have the same population mean with one-way ANOVA test:


```python
# PLOT BAR CHART FOR EACH DISCOUNT LEVEL.  Y AXIS SHOULD BE DISCOUNT PERCENTAGES, CORRECT?
```


```python

import seaborn as sns 

# sns.barplot(x=df['Discounts'], y=y2, palette="vlag", ax=ax2)

```


```python
sns.catplot(x='Discount',y='Quantity',data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1a1e694518>




![png](output_64_1.png)



```python
sns.barplot(x='Discount',y='Quantity',data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c2003df60>




![png](output_65_1.png)



```python
sns.swarmplot(x='Discount',y='Quantity',data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1c1ffdf2e8>




![png](output_66_1.png)



```python

stats.f_oneway(grps[0.0], grps[0.05], grps[0.10], grps[0.15], grps[0.2], grps[0.25])
```




    F_onewayResult(statistic=9.798709497651332, pvalue=2.840680781326738e-09)



#### Results:  Have rejected null hypothesis; therefore 2 or more groups have the different mean.

#### Now, can start the next stage of this hypothesis question to "figure-out" if discounts effects quantity products ordered:

###### Separate orders by discount:


```python
df['discounted'] = np.where(df['Discount'] == 0.00, 0, 1) 
# putting 0 where there's no discount & 1 where there is a discount
```


```python
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>discounted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
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
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 7 columns</p>
</div>



##### Create bar chart with Full Price & Discount Price to have visualization of data for hypothesis question:


```python
grp0 = df.groupby('discounted').get_group(0)['Quantity']
grp1 = df.groupby('discounted').get_group(1)['Quantity']
```


```python
plt.bar(x='Full Price', height = grp0.mean(), yerr=stats.sem(grp0));
plt.bar(x='Discounted Price', height = grp1.mean(), yerr=stats.sem(grp1));
plt.ylabel("Quantity")
```




    Text(0, 0.5, 'Quantity')




![png](output_75_1.png)



```python
# def plot_2_grps(grp1, name1, grp2, name2, ylabel='grp vales'):
```


```python
plot_2_grps(grp0,"Full Price", grp1, "Discounted Price", "Quantity")
```




    (<Figure size 432x288 with 1 Axes>,
     <matplotlib.axes._subplots.AxesSubplot at 0x1c21712518>)




![png](output_77_1.png)



```python

```

##### BTW,  since Error Bars don't overlap in the above chart there' very low chance of results occurring by chance.   

#### Lastly, compare means with Tukey stats tool to evaluate H_N.


```python
import statsmodels.api as sms
```


```python
model = sms.stats.multicomp.pairwise_tukeyhsd(df.Quantity, df.Discount)
model.summary()
```




<table class="simpletable">
<caption>Multiple Comparison of Means - Tukey HSD, FWER=0.05</caption>
<tr>
  <th>group1</th> <th>group2</th> <th>meandiff</th>  <th>p-adj</th>   <th>lower</th>   <th>upper</th>  <th>reject</th>
</tr>
<tr>
    <td>0.0</td>   <td>0.01</td>  <td>-19.7153</td>   <td>0.9</td>  <td>-80.3306</td> <td>40.9001</td>  <td>False</td>
</tr>
<tr>
    <td>0.0</td>   <td>0.02</td>  <td>-19.7153</td>   <td>0.9</td>   <td>-62.593</td> <td>23.1625</td>  <td>False</td>
</tr>
<tr>
    <td>0.0</td>   <td>0.03</td>  <td>-20.0486</td>  <td>0.725</td> <td>-55.0714</td> <td>14.9742</td>  <td>False</td>
</tr>
<tr>
    <td>0.0</td>   <td>0.04</td>  <td>-20.7153</td>   <td>0.9</td>  <td>-81.3306</td> <td>39.9001</td>  <td>False</td>
</tr>
<tr>
    <td>0.0</td>   <td>0.05</td>   <td>6.2955</td>  <td>0.0011</td>  <td>1.5381</td>  <td>11.053</td>   <td>True</td> 
</tr>
<tr>
    <td>0.0</td>   <td>0.06</td>  <td>-19.7153</td>   <td>0.9</td>  <td>-80.3306</td> <td>40.9001</td>  <td>False</td>
</tr>
<tr>
    <td>0.0</td>    <td>0.1</td>   <td>3.5217</td>  <td>0.4269</td>  <td>-1.3783</td> <td>8.4217</td>   <td>False</td>
</tr>
<tr>
    <td>0.0</td>   <td>0.15</td>   <td>6.6669</td>  <td>0.0014</td>   <td>1.551</td>  <td>11.7828</td>  <td>True</td> 
</tr>
<tr>
    <td>0.0</td>    <td>0.2</td>   <td>5.3096</td>  <td>0.0303</td>  <td>0.2508</td>  <td>10.3684</td>  <td>True</td> 
</tr>
<tr>
    <td>0.0</td>   <td>0.25</td>    <td>6.525</td>  <td>0.0023</td>  <td>1.3647</td>  <td>11.6852</td>  <td>True</td> 
</tr>
<tr>
   <td>0.01</td>   <td>0.02</td>     <td>0.0</td>     <td>0.9</td>  <td>-74.2101</td> <td>74.2101</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.03</td>   <td>-0.3333</td>   <td>0.9</td>  <td>-70.2993</td> <td>69.6326</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.04</td>    <td>-1.0</td>     <td>0.9</td>  <td>-86.6905</td> <td>84.6905</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.05</td>   <td>26.0108</td>   <td>0.9</td>   <td>-34.745</td> <td>86.7667</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.06</td>     <td>0.0</td>     <td>0.9</td>  <td>-85.6905</td> <td>85.6905</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>    <td>0.1</td>   <td>23.237</td>    <td>0.9</td>  <td>-37.5302</td> <td>84.0042</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.15</td>   <td>26.3822</td>   <td>0.9</td>  <td>-34.4028</td> <td>87.1671</td>  <td>False</td>
</tr>
<tr>
   <td>0.01</td>    <td>0.2</td>   <td>25.0248</td>   <td>0.9</td>  <td>-35.7554</td> <td>85.805</td>   <td>False</td>
</tr>
<tr>
   <td>0.01</td>   <td>0.25</td>   <td>26.2403</td>   <td>0.9</td>  <td>-34.5485</td> <td>87.029</td>   <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.03</td>   <td>-0.3333</td>   <td>0.9</td>  <td>-55.6463</td> <td>54.9796</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.04</td>    <td>-1.0</td>     <td>0.9</td>  <td>-75.2101</td> <td>73.2101</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.05</td>   <td>26.0108</td> <td>0.6622</td> <td>-17.0654</td> <td>69.087</td>   <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.06</td>     <td>0.0</td>     <td>0.9</td>  <td>-74.2101</td> <td>74.2101</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>    <td>0.1</td>   <td>23.237</td>  <td>0.7914</td> <td>-19.8552</td> <td>66.3292</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.15</td>   <td>26.3822</td> <td>0.6461</td> <td>-16.7351</td> <td>69.4994</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>    <td>0.2</td>   <td>25.0248</td> <td>0.7089</td> <td>-18.0857</td> <td>68.1354</td>  <td>False</td>
</tr>
<tr>
   <td>0.02</td>   <td>0.25</td>   <td>26.2403</td> <td>0.6528</td> <td>-16.8823</td> <td>69.3628</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>   <td>0.04</td>   <td>-0.6667</td>   <td>0.9</td>  <td>-70.6326</td> <td>69.2993</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>   <td>0.05</td>   <td>26.3441</td> <td>0.3639</td>  <td>-8.9214</td> <td>61.6096</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>   <td>0.06</td>   <td>0.3333</td>    <td>0.9</td>  <td>-69.6326</td> <td>70.2993</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>    <td>0.1</td>   <td>23.5703</td> <td>0.5338</td> <td>-11.7147</td> <td>58.8553</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>   <td>0.15</td>   <td>26.7155</td> <td>0.3436</td>  <td>-8.6001</td> <td>62.0311</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>    <td>0.2</td>   <td>25.3582</td>  <td>0.428</td>  <td>-9.9492</td> <td>60.6656</td>  <td>False</td>
</tr>
<tr>
   <td>0.03</td>   <td>0.25</td>   <td>26.5736</td> <td>0.3525</td>  <td>-8.7485</td> <td>61.8957</td>  <td>False</td>
</tr>
<tr>
   <td>0.04</td>   <td>0.05</td>   <td>27.0108</td>   <td>0.9</td>   <td>-33.745</td> <td>87.7667</td>  <td>False</td>
</tr>
<tr>
   <td>0.04</td>   <td>0.06</td>     <td>1.0</td>     <td>0.9</td>  <td>-84.6905</td> <td>86.6905</td>  <td>False</td>
</tr>
<tr>
   <td>0.04</td>    <td>0.1</td>   <td>24.237</td>    <td>0.9</td>  <td>-36.5302</td> <td>85.0042</td>  <td>False</td>
</tr>
<tr>
   <td>0.04</td>   <td>0.15</td>   <td>27.3822</td>   <td>0.9</td>  <td>-33.4028</td> <td>88.1671</td>  <td>False</td>
</tr>
<tr>
   <td>0.04</td>    <td>0.2</td>   <td>26.0248</td>   <td>0.9</td>  <td>-34.7554</td> <td>86.805</td>   <td>False</td>
</tr>
<tr>
   <td>0.04</td>   <td>0.25</td>   <td>27.2403</td>   <td>0.9</td>  <td>-33.5485</td> <td>88.029</td>   <td>False</td>
</tr>
<tr>
   <td>0.05</td>   <td>0.06</td>  <td>-26.0108</td>   <td>0.9</td>  <td>-86.7667</td> <td>34.745</td>   <td>False</td>
</tr>
<tr>
   <td>0.05</td>    <td>0.1</td>   <td>-2.7738</td>   <td>0.9</td>   <td>-9.1822</td> <td>3.6346</td>   <td>False</td>
</tr>
<tr>
   <td>0.05</td>   <td>0.15</td>   <td>0.3714</td>    <td>0.9</td>   <td>-6.2036</td> <td>6.9463</td>   <td>False</td>
</tr>
<tr>
   <td>0.05</td>    <td>0.2</td>   <td>-0.986</td>    <td>0.9</td>   <td>-7.5166</td> <td>5.5447</td>   <td>False</td>
</tr>
<tr>
   <td>0.05</td>   <td>0.25</td>   <td>0.2294</td>    <td>0.9</td>   <td>-6.3801</td>  <td>6.839</td>   <td>False</td>
</tr>
<tr>
   <td>0.06</td>    <td>0.1</td>   <td>23.237</td>    <td>0.9</td>  <td>-37.5302</td> <td>84.0042</td>  <td>False</td>
</tr>
<tr>
   <td>0.06</td>   <td>0.15</td>   <td>26.3822</td>   <td>0.9</td>  <td>-34.4028</td> <td>87.1671</td>  <td>False</td>
</tr>
<tr>
   <td>0.06</td>    <td>0.2</td>   <td>25.0248</td>   <td>0.9</td>  <td>-35.7554</td> <td>85.805</td>   <td>False</td>
</tr>
<tr>
   <td>0.06</td>   <td>0.25</td>   <td>26.2403</td>   <td>0.9</td>  <td>-34.5485</td> <td>87.029</td>   <td>False</td>
</tr>
<tr>
    <td>0.1</td>   <td>0.15</td>   <td>3.1452</td>    <td>0.9</td>   <td>-3.5337</td>  <td>9.824</td>   <td>False</td>
</tr>
<tr>
    <td>0.1</td>    <td>0.2</td>   <td>1.7879</td>    <td>0.9</td>   <td>-4.8474</td> <td>8.4231</td>   <td>False</td>
</tr>
<tr>
    <td>0.1</td>   <td>0.25</td>   <td>3.0033</td>    <td>0.9</td>   <td>-3.7096</td> <td>9.7161</td>   <td>False</td>
</tr>
<tr>
   <td>0.15</td>    <td>0.2</td>   <td>-1.3573</td>   <td>0.9</td>   <td>-8.1536</td> <td>5.4389</td>   <td>False</td>
</tr>
<tr>
   <td>0.15</td>   <td>0.25</td>   <td>-0.1419</td>   <td>0.9</td>   <td>-7.014</td>  <td>6.7302</td>   <td>False</td>
</tr>
<tr>
    <td>0.2</td>   <td>0.25</td>   <td>1.2154</td>    <td>0.9</td>   <td>-5.6143</td> <td>8.0451</td>   <td>False</td>
</tr>
</table>



#### Results:  Null Hypothesis is rejected so the discounts do matter.  In fact, all discounts have a false value in the Tukey model above so each and every discounts is significant.  

### Analysis for second half of question:  what best level of discounts:

#### Graph below for Confidence Intervals as well as having a visual of which discounts might offer the best value and can determine that shouldn't go-over 15% discount.


```python
model.plot_simultaneous(xlabel = "Mean of Quantity Per Order", ylabel = 'Discounts')
```




![png](output_86_0.png)




![png](output_86_1.png)


#### Recommendations:  There are two from the above results:  
##### (1) know statistically that discounts do increase the value or orders; 
##### (2) know with "Confidence" that should limit discounts to no more than 15% because order values don't increase much after that amount.


```python

```

### Hypothesis Q3

#### Are order totals greater with discount products?
##### H_A  order totals > discount order totals
##### H_O order totals < discount order totals


```python
cur.execute("""SELECT * FROM OrderDetail;""")  # Getting the table
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 6 columns</p>
</div>




```python
#adding col with order_total
df['order_total'] = df['Quantity']* df['UnitPrice']
```


```python
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
      <td>168.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
      <td>98.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
      <td>174.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
      <td>167.4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
      <td>1696.0</td>
    </tr>
    <tr>
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
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
      <td>66.5</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
      <td>17.0</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
      <td>30.0</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
      <td>31.0</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
      <td>26.0</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 7 columns</p>
</div>



#### Same process as above adding a column putting 0 where there's no discounts & 1 where there are discounts.


```python
df['discounted'] = np.where(df['Discount'] == 0, 0, 1) 
df
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
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
      <th>discounted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.00</td>
      <td>12</td>
      <td>0.00</td>
      <td>168.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.80</td>
      <td>10</td>
      <td>0.00</td>
      <td>98.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.80</td>
      <td>5</td>
      <td>0.00</td>
      <td>174.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.60</td>
      <td>9</td>
      <td>0.00</td>
      <td>167.4</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.40</td>
      <td>40</td>
      <td>0.00</td>
      <td>1696.0</td>
      <td>0</td>
    </tr>
    <tr>
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
      <td>2150</td>
      <td>11077/64</td>
      <td>11077</td>
      <td>64</td>
      <td>33.25</td>
      <td>2</td>
      <td>0.03</td>
      <td>66.5</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2151</td>
      <td>11077/66</td>
      <td>11077</td>
      <td>66</td>
      <td>17.00</td>
      <td>1</td>
      <td>0.00</td>
      <td>17.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2152</td>
      <td>11077/73</td>
      <td>11077</td>
      <td>73</td>
      <td>15.00</td>
      <td>2</td>
      <td>0.01</td>
      <td>30.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2153</td>
      <td>11077/75</td>
      <td>11077</td>
      <td>75</td>
      <td>7.75</td>
      <td>4</td>
      <td>0.00</td>
      <td>31.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2154</td>
      <td>11077/77</td>
      <td>11077</td>
      <td>77</td>
      <td>13.00</td>
      <td>2</td>
      <td>0.00</td>
      <td>26.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>2155 rows × 8 columns</p>
</div>




```python
total0 = df.groupby('discounted').get_group(0) # testing get_group to make sure it works
```


```python
total1 = df.groupby('discounted').get_group(1)
```

##### Dividing the 0's and 1's into groups to graph them:


```python
total1 = df.groupby('discounted').get_group(1)['order_total']
total0 = df.groupby('discounted').get_group(0)['order_total']
```


```python
plt.bar('Full Price', height=total0.mean(), yerr=stats.sem(total0))
plt.bar('Discounted Price', height=total1.mean(), yerr=stats.sem(total1))

```




    <BarContainer object of 1 artists>




![png](output_100_1.png)



```python
# Better chart from above using new function 
# def plot_2_grps(grp1, name1, grp2, name2, ylabel='grp vales'):
```


```python
plot_2_grps(total0, "Full Price", total1, "Discounted Price", "Orders")
```




    (<Figure size 432x288 with 1 Axes>,
     <matplotlib.axes._subplots.AxesSubplot at 0x1c1ff27390>)




![png](output_102_1.png)



```python

```

##### Are groups large enough to ignore assumption of normality?  Yes.  See results below for confirmation.  Therefore, used Levene test.


```python
print(len(total1))
print(len(total0))
```

    838
    1317



```python
stats.levene(total1, total0)
```




    LeveneResult(statistic=6.117378890508237, pvalue=0.013462566863922663)




```python
stats.ttest_ind(total1, total0, equal_var=False)

```




    Ttest_indResult(statistic=3.172067236911555, pvalue=0.0015429865021245243)




```python

```

#### Results:  Its significant because less than significant value of .05; therefore reject H_0, which means that customers orders are larger with discounted products.

#### Recommendations:  For those products where one of the goals form them are  higher sales, then promote them with discounts.  In most cases, you can probably limit your discounts to no more than 5% (see results from above Hypothesis Question)


```python

```

### Hypothesis Q4

##### Which customers order without discounts? Searching for customers who are NOT price sensitive.
#### H_A:  customers unequal discounts
#### H_O:  customers equal discounts


```python
cur.execute("""SELECT * 
               FROM 'Order' o
               JOIN OrderDetail od
               ON o.Id = od.OrderId
               WHERE Discount = 0;
               """)
no_dis_df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
no_dis_df.columns = [i[0] for i in cur.description]
no_dis_df.head()
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.0</td>
      <td>12</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.8</td>
      <td>10</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.8</td>
      <td>5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.6</td>
      <td>9</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.4</td>
      <td>40</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
no_dis_df.head()
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.0</td>
      <td>12</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.8</td>
      <td>10</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.8</td>
      <td>5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.6</td>
      <td>9</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.4</td>
      <td>40</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Darn it!  This above DataFrame doesn't work b/c discounts are by line item and not order totals so it will show customers that have ordered with discounts thinking that they have not.

#### JOIN Order & OrderDetail.  Give var name of df_ood for Order OrderDetail.


```python
cur.execute("""SELECT * 
               FROM 'Order' o
               JOIN OrderDetail od
               ON o.Id = od.OrderId;
               """)
df_ood = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
df_ood.columns = [i[0] for i in cur.description]
df_ood.head()
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
      <th>Id</th>
      <th>CustomerId</th>
      <th>EmployeeId</th>
      <th>OrderDate</th>
      <th>RequiredDate</th>
      <th>ShippedDate</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>ShipName</th>
      <th>ShipAddress</th>
      <th>ShipCity</th>
      <th>ShipRegion</th>
      <th>ShipPostalCode</th>
      <th>ShipCountry</th>
      <th>Id</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/11</td>
      <td>10248</td>
      <td>11</td>
      <td>14.0</td>
      <td>12</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/42</td>
      <td>10248</td>
      <td>42</td>
      <td>9.8</td>
      <td>10</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>10248</td>
      <td>VINET</td>
      <td>5</td>
      <td>2012-07-04</td>
      <td>2012-08-01</td>
      <td>2012-07-16</td>
      <td>3</td>
      <td>32.38</td>
      <td>Vins et alcools Chevalier</td>
      <td>59 rue de l'Abbaye</td>
      <td>Reims</td>
      <td>Western Europe</td>
      <td>51100</td>
      <td>France</td>
      <td>10248/72</td>
      <td>10248</td>
      <td>72</td>
      <td>34.8</td>
      <td>5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/14</td>
      <td>10249</td>
      <td>14</td>
      <td>18.6</td>
      <td>9</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>10249</td>
      <td>TOMSP</td>
      <td>6</td>
      <td>2012-07-05</td>
      <td>2012-08-16</td>
      <td>2012-07-10</td>
      <td>1</td>
      <td>11.61</td>
      <td>Toms Spezialitäten</td>
      <td>Luisenstr. 48</td>
      <td>Münster</td>
      <td>Western Europe</td>
      <td>44087</td>
      <td>Germany</td>
      <td>10249/51</td>
      <td>10249</td>
      <td>51</td>
      <td>42.4</td>
      <td>40</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Adding a column to look at order total (called order_total).


```python
df_ood['order_total'] = df_ood['UnitPrice'] * df_ood['Quantity']
```


```python
df_ood.groupby(df_ood['CustomerId']).sum().head(50)
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
      <th>Id</th>
      <th>EmployeeId</th>
      <th>ShipVia</th>
      <th>Freight</th>
      <th>OrderId</th>
      <th>ProductId</th>
      <th>UnitPrice</th>
      <th>Quantity</th>
      <th>Discount</th>
      <th>order_total</th>
    </tr>
    <tr>
      <th>CustomerId</th>
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
      <td>ALFKI</td>
      <td>129621</td>
      <td>40</td>
      <td>17</td>
      <td>419.60</td>
      <td>129621</td>
      <td>554</td>
      <td>320.85</td>
      <td>174</td>
      <td>1.05</td>
      <td>4596.20</td>
    </tr>
    <tr>
      <td>ANATR</td>
      <td>106954</td>
      <td>42</td>
      <td>24</td>
      <td>306.59</td>
      <td>106954</td>
      <td>402</td>
      <td>215.05</td>
      <td>63</td>
      <td>0.00</td>
      <td>1402.95</td>
    </tr>
    <tr>
      <td>ANTO</td>
      <td>180350</td>
      <td>71</td>
      <td>33</td>
      <td>667.29</td>
      <td>180350</td>
      <td>650</td>
      <td>369.23</td>
      <td>359</td>
      <td>1.00</td>
      <td>7515.35</td>
    </tr>
    <tr>
      <td>AROUT</td>
      <td>320404</td>
      <td>126</td>
      <td>67</td>
      <td>1447.14</td>
      <td>320404</td>
      <td>1371</td>
      <td>575.30</td>
      <td>650</td>
      <td>0.70</td>
      <td>13806.50</td>
    </tr>
    <tr>
      <td>BERGS</td>
      <td>552534</td>
      <td>217</td>
      <td>105</td>
      <td>4835.18</td>
      <td>552534</td>
      <td>2029</td>
      <td>1425.65</td>
      <td>1001</td>
      <td>3.00</td>
      <td>26968.15</td>
    </tr>
    <tr>
      <td>BLAUS</td>
      <td>150911</td>
      <td>97</td>
      <td>34</td>
      <td>351.41</td>
      <td>150911</td>
      <td>565</td>
      <td>347.05</td>
      <td>140</td>
      <td>0.00</td>
      <td>3239.80</td>
    </tr>
    <tr>
      <td>BLONP</td>
      <td>272374</td>
      <td>122</td>
      <td>49</td>
      <td>1980.00</td>
      <td>272374</td>
      <td>1141</td>
      <td>848.75</td>
      <td>666</td>
      <td>0.75</td>
      <td>19088.00</td>
    </tr>
    <tr>
      <td>BOLID</td>
      <td>63550</td>
      <td>29</td>
      <td>11</td>
      <td>444.10</td>
      <td>63550</td>
      <td>234</td>
      <td>209.19</td>
      <td>190</td>
      <td>0.70</td>
      <td>5297.80</td>
    </tr>
    <tr>
      <td>BONAP</td>
      <td>470646</td>
      <td>196</td>
      <td>84</td>
      <td>3952.65</td>
      <td>470646</td>
      <td>1599</td>
      <td>1061.45</td>
      <td>980</td>
      <td>3.20</td>
      <td>23850.95</td>
    </tr>
    <tr>
      <td>BOTTM</td>
      <td>375682</td>
      <td>135</td>
      <td>84</td>
      <td>2171.97</td>
      <td>375682</td>
      <td>1377</td>
      <td>883.60</td>
      <td>956</td>
      <td>2.95</td>
      <td>22607.70</td>
    </tr>
    <tr>
      <td>BSBEV</td>
      <td>233781</td>
      <td>100</td>
      <td>60</td>
      <td>563.15</td>
      <td>233781</td>
      <td>844</td>
      <td>499.20</td>
      <td>293</td>
      <td>0.00</td>
      <td>6089.90</td>
    </tr>
    <tr>
      <td>CACTU</td>
      <td>118846</td>
      <td>71</td>
      <td>24</td>
      <td>158.80</td>
      <td>118846</td>
      <td>528</td>
      <td>197.50</td>
      <td>115</td>
      <td>0.00</td>
      <td>1814.80</td>
    </tr>
    <tr>
      <td>CENTC</td>
      <td>20518</td>
      <td>8</td>
      <td>6</td>
      <td>6.50</td>
      <td>20518</td>
      <td>58</td>
      <td>28.80</td>
      <td>11</td>
      <td>0.00</td>
      <td>100.80</td>
    </tr>
    <tr>
      <td>CHOPS</td>
      <td>234913</td>
      <td>95</td>
      <td>44</td>
      <td>940.44</td>
      <td>234913</td>
      <td>1015</td>
      <td>609.20</td>
      <td>465</td>
      <td>1.30</td>
      <td>12886.30</td>
    </tr>
    <tr>
      <td>COMMI</td>
      <td>105639</td>
      <td>49</td>
      <td>12</td>
      <td>468.84</td>
      <td>105639</td>
      <td>424</td>
      <td>259.15</td>
      <td>133</td>
      <td>0.00</td>
      <td>3810.75</td>
    </tr>
    <tr>
      <td>CONSH</td>
      <td>73925</td>
      <td>42</td>
      <td>12</td>
      <td>116.45</td>
      <td>73925</td>
      <td>146</td>
      <td>190.15</td>
      <td>87</td>
      <td>0.00</td>
      <td>1719.10</td>
    </tr>
    <tr>
      <td>DRACD</td>
      <td>107066</td>
      <td>41</td>
      <td>24</td>
      <td>595.84</td>
      <td>107066</td>
      <td>398</td>
      <td>191.08</td>
      <td>160</td>
      <td>0.00</td>
      <td>3763.21</td>
    </tr>
    <tr>
      <td>DUMO</td>
      <td>95802</td>
      <td>46</td>
      <td>16</td>
      <td>157.61</td>
      <td>95802</td>
      <td>287</td>
      <td>168.65</td>
      <td>80</td>
      <td>0.00</td>
      <td>1615.90</td>
    </tr>
    <tr>
      <td>EASTC</td>
      <td>226763</td>
      <td>105</td>
      <td>38</td>
      <td>2361.77</td>
      <td>226763</td>
      <td>809</td>
      <td>575.72</td>
      <td>569</td>
      <td>0.50</td>
      <td>15033.66</td>
    </tr>
    <tr>
      <td>ERNSH</td>
      <td>1087562</td>
      <td>463</td>
      <td>196</td>
      <td>24536.92</td>
      <td>1087562</td>
      <td>3947</td>
      <td>2666.67</td>
      <td>4543</td>
      <td>6.95</td>
      <td>113236.68</td>
    </tr>
    <tr>
      <td>FAMIA</td>
      <td>199742</td>
      <td>96</td>
      <td>51</td>
      <td>663.39</td>
      <td>199742</td>
      <td>826</td>
      <td>245.14</td>
      <td>357</td>
      <td>1.20</td>
      <td>4438.90</td>
    </tr>
    <tr>
      <td>FOLIG</td>
      <td>170165</td>
      <td>65</td>
      <td>36</td>
      <td>2500.45</td>
      <td>170165</td>
      <td>661</td>
      <td>443.35</td>
      <td>354</td>
      <td>0.00</td>
      <td>11666.90</td>
    </tr>
    <tr>
      <td>FOLKO</td>
      <td>482403</td>
      <td>226</td>
      <td>76</td>
      <td>5310.94</td>
      <td>482403</td>
      <td>2121</td>
      <td>1060.84</td>
      <td>1234</td>
      <td>3.85</td>
      <td>32555.55</td>
    </tr>
    <tr>
      <td>FRANK</td>
      <td>509067</td>
      <td>199</td>
      <td>92</td>
      <td>4931.31</td>
      <td>509067</td>
      <td>2160</td>
      <td>982.98</td>
      <td>1525</td>
      <td>3.15</td>
      <td>28722.71</td>
    </tr>
    <tr>
      <td>FRANR</td>
      <td>64704</td>
      <td>11</td>
      <td>11</td>
      <td>251.36</td>
      <td>64704</td>
      <td>299</td>
      <td>282.59</td>
      <td>69</td>
      <td>0.00</td>
      <td>3172.16</td>
    </tr>
    <tr>
      <td>FRANS</td>
      <td>108327</td>
      <td>26</td>
      <td>12</td>
      <td>145.88</td>
      <td>108327</td>
      <td>457</td>
      <td>244.00</td>
      <td>54</td>
      <td>0.00</td>
      <td>1545.70</td>
    </tr>
    <tr>
      <td>FURIB</td>
      <td>210342</td>
      <td>76</td>
      <td>52</td>
      <td>893.89</td>
      <td>210342</td>
      <td>964</td>
      <td>427.90</td>
      <td>349</td>
      <td>1.95</td>
      <td>7151.55</td>
    </tr>
    <tr>
      <td>GALED</td>
      <td>84895</td>
      <td>37</td>
      <td>14</td>
      <td>68.17</td>
      <td>84895</td>
      <td>420</td>
      <td>156.70</td>
      <td>42</td>
      <td>0.00</td>
      <td>836.70</td>
    </tr>
    <tr>
      <td>GODOS</td>
      <td>280146</td>
      <td>115</td>
      <td>54</td>
      <td>1701.56</td>
      <td>280146</td>
      <td>1141</td>
      <td>712.09</td>
      <td>395</td>
      <td>1.05</td>
      <td>11830.10</td>
    </tr>
    <tr>
      <td>GOURL</td>
      <td>203948</td>
      <td>72</td>
      <td>42</td>
      <td>882.95</td>
      <td>203948</td>
      <td>685</td>
      <td>486.68</td>
      <td>315</td>
      <td>1.30</td>
      <td>8702.23</td>
    </tr>
    <tr>
      <td>GREAL</td>
      <td>235946</td>
      <td>86</td>
      <td>46</td>
      <td>2455.43</td>
      <td>235946</td>
      <td>901</td>
      <td>1091.54</td>
      <td>345</td>
      <td>1.55</td>
      <td>19711.13</td>
    </tr>
    <tr>
      <td>GROSR</td>
      <td>42106</td>
      <td>18</td>
      <td>12</td>
      <td>135.60</td>
      <td>42106</td>
      <td>186</td>
      <td>165.55</td>
      <td>34</td>
      <td>0.00</td>
      <td>1488.70</td>
    </tr>
    <tr>
      <td>HANAR</td>
      <td>342869</td>
      <td>108</td>
      <td>55</td>
      <td>1553.85</td>
      <td>342869</td>
      <td>1364</td>
      <td>1396.10</td>
      <td>839</td>
      <td>2.15</td>
      <td>34101.15</td>
    </tr>
    <tr>
      <td>HILAA</td>
      <td>480253</td>
      <td>233</td>
      <td>82</td>
      <td>3417.38</td>
      <td>480253</td>
      <td>2042</td>
      <td>930.62</td>
      <td>1096</td>
      <td>1.50</td>
      <td>23611.58</td>
    </tr>
    <tr>
      <td>HUNGC</td>
      <td>94228</td>
      <td>30</td>
      <td>15</td>
      <td>302.87</td>
      <td>94228</td>
      <td>340</td>
      <td>205.35</td>
      <td>122</td>
      <td>0.00</td>
      <td>3063.20</td>
    </tr>
    <tr>
      <td>HUNGO</td>
      <td>582184</td>
      <td>278</td>
      <td>115</td>
      <td>7214.49</td>
      <td>582184</td>
      <td>2282</td>
      <td>1719.86</td>
      <td>1684</td>
      <td>6.25</td>
      <td>57317.39</td>
    </tr>
    <tr>
      <td>ISLAT</td>
      <td>244716</td>
      <td>113</td>
      <td>44</td>
      <td>1141.40</td>
      <td>244716</td>
      <td>1088</td>
      <td>498.00</td>
      <td>295</td>
      <td>0.00</td>
      <td>6146.30</td>
    </tr>
    <tr>
      <td>KOENE</td>
      <td>415011</td>
      <td>173</td>
      <td>88</td>
      <td>3033.25</td>
      <td>415011</td>
      <td>1366</td>
      <td>1229.24</td>
      <td>903</td>
      <td>1.55</td>
      <td>31745.75</td>
    </tr>
    <tr>
      <td>LACOR</td>
      <td>120218</td>
      <td>44</td>
      <td>16</td>
      <td>262.45</td>
      <td>120218</td>
      <td>444</td>
      <td>285.03</td>
      <td>83</td>
      <td>0.00</td>
      <td>1992.05</td>
    </tr>
    <tr>
      <td>LAMAI</td>
      <td>328038</td>
      <td>141</td>
      <td>62</td>
      <td>1524.21</td>
      <td>328038</td>
      <td>1321</td>
      <td>655.34</td>
      <td>442</td>
      <td>3.90</td>
      <td>10272.35</td>
    </tr>
    <tr>
      <td>LAUGB</td>
      <td>85155</td>
      <td>19</td>
      <td>24</td>
      <td>28.82</td>
      <td>85155</td>
      <td>325</td>
      <td>71.80</td>
      <td>62</td>
      <td>0.00</td>
      <td>522.50</td>
    </tr>
    <tr>
      <td>LAZYK</td>
      <td>21027</td>
      <td>9</td>
      <td>5</td>
      <td>19.40</td>
      <td>21027</td>
      <td>51</td>
      <td>35.70</td>
      <td>20</td>
      <td>0.00</td>
      <td>357.00</td>
    </tr>
    <tr>
      <td>LEHMS</td>
      <td>413219</td>
      <td>194</td>
      <td>59</td>
      <td>2938.11</td>
      <td>413219</td>
      <td>1549</td>
      <td>1028.19</td>
      <td>794</td>
      <td>3.75</td>
      <td>21282.02</td>
    </tr>
    <tr>
      <td>LETSS</td>
      <td>107437</td>
      <td>50</td>
      <td>20</td>
      <td>546.63</td>
      <td>107437</td>
      <td>472</td>
      <td>229.64</td>
      <td>181</td>
      <td>1.10</td>
      <td>3490.02</td>
    </tr>
    <tr>
      <td>LILAS</td>
      <td>360613</td>
      <td>140</td>
      <td>69</td>
      <td>2214.70</td>
      <td>360613</td>
      <td>1372</td>
      <td>707.14</td>
      <td>836</td>
      <td>3.30</td>
      <td>17825.06</td>
    </tr>
    <tr>
      <td>LINOD</td>
      <td>377604</td>
      <td>135</td>
      <td>62</td>
      <td>2104.76</td>
      <td>377604</td>
      <td>1213</td>
      <td>631.60</td>
      <td>970</td>
      <td>3.10</td>
      <td>17889.55</td>
    </tr>
    <tr>
      <td>LONEP</td>
      <td>149480</td>
      <td>50</td>
      <td>25</td>
      <td>181.25</td>
      <td>149480</td>
      <td>643</td>
      <td>437.70</td>
      <td>134</td>
      <td>0.00</td>
      <td>4258.60</td>
    </tr>
    <tr>
      <td>MAGAA</td>
      <td>222771</td>
      <td>89</td>
      <td>46</td>
      <td>1208.14</td>
      <td>222771</td>
      <td>747</td>
      <td>389.30</td>
      <td>433</td>
      <td>1.05</td>
      <td>7603.85</td>
    </tr>
    <tr>
      <td>MAISD</td>
      <td>183769</td>
      <td>97</td>
      <td>33</td>
      <td>1085.52</td>
      <td>183769</td>
      <td>762</td>
      <td>496.38</td>
      <td>320</td>
      <td>1.00</td>
      <td>10430.58</td>
    </tr>
    <tr>
      <td>MEREP</td>
      <td>336332</td>
      <td>123</td>
      <td>72</td>
      <td>4121.11</td>
      <td>336332</td>
      <td>1273</td>
      <td>952.10</td>
      <td>966</td>
      <td>1.85</td>
      <td>32203.90</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

#### Getting customers with no discounts cust_no_dis_df


```python
cur.execute("""SELECT CustomerId, SUM(Discount), SUM(Quantity)
               FROM 'Order' o
               JOIN OrderDetail od
               ON o.Id = od.OrderId
               GROUP BY o.CustomerId
               HAVING SUM(Discount) = 0;
               """)
cust_no_dis_df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
cust_no_dis_df.columns = [i[0] for i in cur.description]
cust_no_dis_df.head(3)
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
      <th>CustomerId</th>
      <th>SUM(Discount)</th>
      <th>SUM(Quantity)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ANATR</td>
      <td>0.0</td>
      <td>63</td>
    </tr>
    <tr>
      <td>1</td>
      <td>BLAUS</td>
      <td>0.0</td>
      <td>140</td>
    </tr>
    <tr>
      <td>2</td>
      <td>BSBEV</td>
      <td>0.0</td>
      <td>293</td>
    </tr>
  </tbody>
</table>
</div>



###### Hypothesis
##### H_A cust with zero discounts buy less
##### H_N cust with zero discount buy same amount

#### note about data:  assumption that data is normal because sample size is more than large enough.

##### Getting customer data with discounts:  cust_with_dis_df


```python
cur.execute("""SELECT CustomerId, SUM(Discount), SUM(Quantity)
               FROM 'Order' o
               JOIN OrderDetail od
               ON o.Id = od.OrderId
               GROUP BY o.CustomerId
               HAVING SUM(Discount) > 0;
               """)
cust_with_dis_df = pd.DataFrame(cur.fetchall()) #Take results and create dataframe
cust_with_dis_df.columns = [i[0] for i in cur.description]
cust_with_dis_df.head(3)
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
      <th>CustomerId</th>
      <th>SUM(Discount)</th>
      <th>SUM(Quantity)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ALFKI</td>
      <td>1.05</td>
      <td>174</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ANTO</td>
      <td>1.00</td>
      <td>359</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AROUT</td>
      <td>0.70</td>
      <td>650</td>
    </tr>
  </tbody>
</table>
</div>



##### Visualization 


```python
plt.bar(x='No Discount QTY', height = cust_no_dis_df['SUM(Quantity)'].mean(), yerr=stats.sem(cust_no_dis_df['SUM(Quantity)']));
plt.bar(x='Discount QTY ', height = cust_with_dis_df['SUM(Quantity)'].mean(), yerr=stats.sem(cust_with_dis_df['SUM(Quantity)']));

```


![png](output_130_0.png)


#### Testing Null Hypothesis with 2 sample t test (code below):


```python
# Better chart from above using new function 
# def plot_2_grps(grp1, name1, grp2, name2, ylabel='grp vales'):
```


```python
plot_2_grps(cust_no_dis_df['SUM(Quantity)'], "No Discount", 
           cust_with_dis_df['SUM(Quantity)'], "Discounted", "Orders")
```




    (<Figure size 432x288 with 1 Axes>,
     <matplotlib.axes._subplots.AxesSubplot at 0x1c21712cc0>)




![png](output_133_1.png)



```python

```


```python

```


```python
stats.ttest_ind(cust_no_dis_df['SUM(Quantity)'],cust_with_dis_df['SUM(Quantity)'])
```




    Ttest_indResult(statistic=-4.586149855913255, pvalue=1.5045188654504239e-05)



#### Result:  Reject Null Hypothesis based on P Value so customers with zero discount buy less.

##### Recommendation:  continue to offer discounts for customers to buy more quantities of products as well as targeting customers who have not used discounts so they buy more based on what we've discounted to them.  


```python

```

## Conclusion 
We analyzed the data to gain a complete understanding of Tradewind Partners SQL data for discount practices over the last fiscal year.  For the most part, discount strategies are working well:

YOY sales increased by more than 110%
Orders have increased
Product quantities have increased

Recommendations to consider for pricing policies in the following year:

Continue to use many of the same strategies that are deployed last year because they have worked demonstrated by the above data.
Be more conscience with the discounts percentages offered to your clients.  Anything less than 5% really have no importance to them to increase orders.
In addition, there’s little need to offer discounts more than 15%.  Moreover, save margins because clients are not as motivated to purchase more with discounts greater than 15%.
The vast majority of customers are very price sensitive based on the huge amount of volume of orders with discounts. This knowledge is power:  offer discounts to motivate buyers to buy in the timeframe that meets your inventory needs, such as overstock items and to deplete inventory of perishable goods.




```python

```
