# Modeling UofT Finances

The purpose of this notebook is to model my expected costs and earnings as a first-year student in UofT. I consider multiple scenarios, across multiple residences (to help in choosing one).


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Base Costs

To start, I'm going to model my base cost estimates. The categories are:
- Fixed Costs
  - Tuition and Fees (two payments: Fall in September, Winter in January)
  - Residence (Also two payments, Sep and Nov)
- Variable Costs
  - Food
  - Transportation (pay-per ride, TTC Student Pass, TTC Adult Pass as proxies)
  - Phone Bill (I have to do more detailed research, but see a $45 Freedom plan for students)
  - Health & Personal Care (toiletries and such)
  - Entertainment (going out with friends and such)
  - Miscellaneous (a buffer)


```python
fixed_costs = ['Tuition & Fees', 'Residence']
variable_costs = ['Books & Supplies', 'Food', 'Transportation', 'Phone Bill', 'Health & Personal Care', 'Entertainment', 'Miscellaneous']
var_base = {'Books & Supplies': 40,'Food': 300, 'Transportation': 130, 'Phone Bill': 45, 'Health & Personal Care': 120, 'Entertainment': 150, 'Miscellaneous': 70}
```


```python
base_costs = pd.DataFrame(0.0, index=pd.date_range('2026-09-01', periods=8, freq='MS'), columns=fixed_costs+variable_costs)
base_costs.index.name = 'Date'

# Setting the fixed costs
base_costs.loc['2026-09-01', 'Tuition & Fees'] = 3850 # First tuition payment
base_costs.loc['2027-01-01', 'Tuition & Fees'] = 3850 # Second tuition payment
base_costs.loc['2026-09-01', 'Residence'] = 7497 # First residence payment, Woodsworth
base_costs.loc['2027-01-01', 'Residence'] = 5118 # Second residence payment

for cost in variable_costs:
    base_costs[cost] = var_base[cost]

base_costs
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
      <th>Tuition &amp; Fees</th>
      <th>Residence</th>
      <th>Books &amp; Supplies</th>
      <th>Food</th>
      <th>Transportation</th>
      <th>Phone Bill</th>
      <th>Health &amp; Personal Care</th>
      <th>Entertainment</th>
      <th>Miscellaneous</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2026-09-01</th>
      <td>3850.0</td>
      <td>7497.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>3850.0</td>
      <td>5118.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>40</td>
      <td>300</td>
      <td>130</td>
      <td>45</td>
      <td>120</td>
      <td>150</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>



## Optimistic and Pessimistic Costs

Now I'm going to input my optimistic and pessimistic forecasts for the same categories.


```python
opt_costs = base_costs.copy()
pess_costs = base_costs.copy()

var_opt = {'Books & Supplies': 10,'Food': 200, 'Transportation': 100, 'Phone Bill': 45, 'Health & Personal Care': 80, 'Entertainment': 60, 'Miscellaneous': 40}

var_pess = {'Books & Supplies': 120,'Food': 450, 'Transportation': 165, 'Phone Bill': 45, 'Health & Personal Care': 200, 'Entertainment': 300, 'Miscellaneous': 150}

for cost in variable_costs:
    opt_costs[cost] = var_opt[cost]
    pess_costs[cost] = var_pess[cost]
```


```python
pess_costs
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
      <th>Tuition &amp; Fees</th>
      <th>Residence</th>
      <th>Books &amp; Supplies</th>
      <th>Food</th>
      <th>Transportation</th>
      <th>Phone Bill</th>
      <th>Health &amp; Personal Care</th>
      <th>Entertainment</th>
      <th>Miscellaneous</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2026-09-01</th>
      <td>3850.0</td>
      <td>7497.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>3850.0</td>
      <td>5118.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>120</td>
      <td>450</td>
      <td>165</td>
      <td>45</td>
      <td>200</td>
      <td>300</td>
      <td>150</td>
    </tr>
  </tbody>
</table>
</div>




```python
opt_costs
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
      <th>Tuition &amp; Fees</th>
      <th>Residence</th>
      <th>Books &amp; Supplies</th>
      <th>Food</th>
      <th>Transportation</th>
      <th>Phone Bill</th>
      <th>Health &amp; Personal Care</th>
      <th>Entertainment</th>
      <th>Miscellaneous</th>
    </tr>
    <tr>
      <th>Date</th>
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
      <th>2026-09-01</th>
      <td>3850.0</td>
      <td>7497.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>3850.0</td>
      <td>5118.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>10</td>
      <td>200</td>
      <td>100</td>
      <td>45</td>
      <td>80</td>
      <td>60</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
</div>



## Revenues

Now here, I'm going to repeat the above method, but instead for my revenues by category. I will do optimistic, base, and pessimistic all in one block to save space.


```python
fixed_revenues = ['OSAP Grants', 'OSAP Loans', 'UTAPS', 'Scholarships']
variable_revenues = ['Work']
base_starting_balance = 17.60 * 35 * 16 # Summer work estimate: ontario min wage * 35hrs/week.

base_rev = pd.DataFrame(0.0, index=pd.date_range('2026-09-01', periods=8, freq='MS'), columns=fixed_revenues+variable_revenues)
base_rev.index.name = 'Date'

base_rev['Work'] = 17.60 * 10 * 4  # Ontario min wage * 10hrs/week; base estimate
base_rev.loc['2026-09-01', 'OSAP Grants'] = 13569*0.25 # First OSAP grant
base_rev.loc['2026-09-01', 'OSAP Loans'] = 13569*0.75 # First OSAP loan
base_rev.loc['2027-01-01', 'OSAP Grants'] = 9046*0.25 # Second OSAP grant
base_rev.loc['2027-01-01', 'OSAP Loans'] = 9046*0.75 # Second OSAP loan
base_rev.loc['2026-10-01', 'UTAPS'] = 3000

base_rev

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
      <th>OSAP Grants</th>
      <th>OSAP Loans</th>
      <th>UTAPS</th>
      <th>Scholarships</th>
      <th>Work</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>3392.25</td>
      <td>10176.75</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>3000.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>2261.50</td>
      <td>6784.50</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>704.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pess_rev = base_rev.copy()
pess_rev['Work'] = 17.60 * 5 * 4
pess_starting_balance = 17.60 * 25 * 12

pess_rev

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
      <th>OSAP Grants</th>
      <th>OSAP Loans</th>
      <th>UTAPS</th>
      <th>Scholarships</th>
      <th>Work</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>3392.25</td>
      <td>10176.75</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>3000.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>2261.50</td>
      <td>6784.50</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>352.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
opt_rev = base_rev.copy()
opt_rev['Work'] = 17.60 * 15 * 4
opt_starting_balance = 17.60 * 40 * 18

opt_rev
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
      <th>OSAP Grants</th>
      <th>OSAP Loans</th>
      <th>UTAPS</th>
      <th>Scholarships</th>
      <th>Work</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>3392.25</td>
      <td>10176.75</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>3000.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>2261.50</td>
      <td>6784.50</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1056.0</td>
    </tr>
  </tbody>
</table>
</div>



## Consolidation

I create three dataframes encompassing the net data from costs and revenues dataframes, by scenario. 


```python
def build_net(rev, cost, starting_balance):
    net = pd.DataFrame(index=rev.index)
    net['Net'] = rev.sum(axis=1) - cost.sum(axis=1)
    net['Running Balance'] = net['Net'].cumsum() + starting_balance
    return net
```


```python
net_df = build_net(base_rev, base_costs, base_starting_balance)
net_df
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
      <th>Net</th>
      <th>Running Balance</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>2071.0</td>
      <td>11927.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>2849.0</td>
      <td>14776.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>-151.0</td>
      <td>14625.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>-151.0</td>
      <td>14474.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>-73.0</td>
      <td>14401.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>-151.0</td>
      <td>14250.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>-151.0</td>
      <td>14099.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>-151.0</td>
      <td>13948.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
net_pess_df = build_net(pess_rev, pess_costs, pess_starting_balance)
net_pess_df
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
      <th>Net</th>
      <th>Running Balance</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>1144.0</td>
      <td>6424.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>1922.0</td>
      <td>8346.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>-1078.0</td>
      <td>7268.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>-1078.0</td>
      <td>6190.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>-1000.0</td>
      <td>5190.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>-1078.0</td>
      <td>4112.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>-1078.0</td>
      <td>3034.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>-1078.0</td>
      <td>1956.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
net_opt_df = build_net(opt_rev, opt_costs, opt_starting_balance)
net_opt_df
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
      <th>Net</th>
      <th>Running Balance</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2026-09-01</th>
      <td>2743.0</td>
      <td>15415.0</td>
    </tr>
    <tr>
      <th>2026-10-01</th>
      <td>3521.0</td>
      <td>18936.0</td>
    </tr>
    <tr>
      <th>2026-11-01</th>
      <td>521.0</td>
      <td>19457.0</td>
    </tr>
    <tr>
      <th>2026-12-01</th>
      <td>521.0</td>
      <td>19978.0</td>
    </tr>
    <tr>
      <th>2027-01-01</th>
      <td>599.0</td>
      <td>20577.0</td>
    </tr>
    <tr>
      <th>2027-02-01</th>
      <td>521.0</td>
      <td>21098.0</td>
    </tr>
    <tr>
      <th>2027-03-01</th>
      <td>521.0</td>
      <td>21619.0</td>
    </tr>
    <tr>
      <th>2027-04-01</th>
      <td>521.0</td>
      <td>22140.0</td>
    </tr>
  </tbody>
</table>
</div>



## Metrics

I use the consolidated dataframes to perform calculations and get metrics, the end goal of this project.


```python
def get_metrics(net_df):
    return {
        'final_balance': net_df['Running Balance'].iloc[-1],
        'min_balance': net_df['Running Balance'].min(),
        'max_deficit': net_df['Net'].min(),
        'worst_month': net_df['Net'].idxmin(),
        'months_negative': (net_df['Running Balance'] < 0).sum()
    }
metrics = pd.DataFrame({
    'base': get_metrics(net_df),
    'optimistic': get_metrics(net_opt_df),
    'pessimistic': get_metrics(net_pess_df)
}).T
metrics
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
      <th>final_balance</th>
      <th>min_balance</th>
      <th>max_deficit</th>
      <th>worst_month</th>
      <th>months_negative</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>base</th>
      <td>13948.0</td>
      <td>11927.0</td>
      <td>-151.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>optimistic</th>
      <td>22140.0</td>
      <td>15415.0</td>
      <td>521.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pessimistic</th>
      <td>1956.0</td>
      <td>1956.0</td>
      <td>-1078.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
metrics['required_buffer'] = metrics['min_balance'].apply(lambda x: max(0, -x)) 
metrics
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
      <th>final_balance</th>
      <th>min_balance</th>
      <th>max_deficit</th>
      <th>worst_month</th>
      <th>months_negative</th>
      <th>required_buffer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>base</th>
      <td>13948.0</td>
      <td>11927.0</td>
      <td>-151.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>optimistic</th>
      <td>22140.0</td>
      <td>15415.0</td>
      <td>521.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pessimistic</th>
      <td>1956.0</td>
      <td>1956.0</td>
      <td>-1078.0</td>
      <td>2026-11-01 00:00:00</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(10, 6)) 

net_df['Net'].plot(ax=ax, label='Base Scenario')
net_pess_df['Net'].plot(ax=ax, label='Pessimistic Scenario')
net_opt_df['Net'].plot(ax=ax, label='Optimistic Scenario')

ax.set_title('Net Monthly Revenues by Scenario')
ax.set_xlabel('Month')
ax.set_ylabel('Net Revenue in CAD$')
ax.legend()

plt.show()
```


    
![png](main_files/main_21_0.png)
    



```python
fig, ax = plt.subplots(figsize=(10, 6)) 

net_df['Running Balance'].plot(ax=ax, label='Base Scenario')
net_pess_df['Running Balance'].plot(ax=ax, label='Pessimistic Scenario')
net_opt_df['Running Balance'].plot(ax=ax, label='Optimistic Scenario')

ax.set_title('Monthly Balance by Scenario')
ax.set_xlabel('Month')
ax.set_ylabel('Balance in CAD$')
ax.legend()

plt.show()
```


    
![png](main_files/main_22_0.png)
    


Under all scenarios, my finances seem to be viable. I retain a final balance across them all, and am never in the red.

## Sensitivity Analysis

Seeing how outcomes vary with changes in summer work and part-time work revenue.


```python
summer_range = range(0, 13000, 500)   # total summer earnings
work_range = range(0, 1056, 100)      # monthly work income

results = []

for summer in summer_range:
    for work in work_range:
        rev = pess_rev.copy()
         
        rev['Work'] = work 
        
        net = build_net(rev, pess_costs, summer)
        min_balance = int(net['Running Balance'].min())
        
        results.append({
            'summer': summer,
            'work': work,
            'min_balance': min_balance,
            'final_balance': net['Running Balance'].iloc[-1],
            'months_negative': (net['Running Balance'] < 0).sum(),
            'max_deficit': net['Net'].min(),
        })

sens_df = pd.DataFrame(results)
sens_df
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
      <th>summer</th>
      <th>work</th>
      <th>min_balance</th>
      <th>final_balance</th>
      <th>months_negative</th>
      <th>max_deficit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>-6140</td>
      <td>-6140.0</td>
      <td>5</td>
      <td>-1430.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>100</td>
      <td>-5340</td>
      <td>-5340.0</td>
      <td>5</td>
      <td>-1330.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>200</td>
      <td>-4540</td>
      <td>-4540.0</td>
      <td>4</td>
      <td>-1230.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>300</td>
      <td>-3740</td>
      <td>-3740.0</td>
      <td>4</td>
      <td>-1130.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>400</td>
      <td>-2940</td>
      <td>-2940.0</td>
      <td>3</td>
      <td>-1030.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>281</th>
      <td>12500</td>
      <td>600</td>
      <td>11160</td>
      <td>11160.0</td>
      <td>0</td>
      <td>-830.0</td>
    </tr>
    <tr>
      <th>282</th>
      <td>12500</td>
      <td>700</td>
      <td>11960</td>
      <td>11960.0</td>
      <td>0</td>
      <td>-730.0</td>
    </tr>
    <tr>
      <th>283</th>
      <td>12500</td>
      <td>800</td>
      <td>12760</td>
      <td>12760.0</td>
      <td>0</td>
      <td>-630.0</td>
    </tr>
    <tr>
      <th>284</th>
      <td>12500</td>
      <td>900</td>
      <td>13560</td>
      <td>13560.0</td>
      <td>0</td>
      <td>-530.0</td>
    </tr>
    <tr>
      <th>285</th>
      <td>12500</td>
      <td>1000</td>
      <td>14292</td>
      <td>14360.0</td>
      <td>0</td>
      <td>-430.0</td>
    </tr>
  </tbody>
</table>
<p>286 rows × 6 columns</p>
</div>




```python
sens_pt = sens_df.pivot_table(values='min_balance', index='summer', columns='work').astype('int64')
plt.figure(figsize=(12, 8))
sns.heatmap(sens_pt, annot=True, fmt='d')
plt.title('Heatmap of Minimum Balance by Summer and Part-time Work Earnings')
plt.show()
```


    
![png](main_files/main_26_0.png)
    



```python
sns.heatmap(sens_df.pivot_table(values='months_negative', index='summer', columns='work'), annot=True)
plt.title('Heatmap of Months Negative by Summer and Part-time Work Earnings')
plt.show()
```


    
![png](main_files/main_27_0.png)
    



```python
# Feasible points (never go negative)
feasible = sens_df[sens_df['months_negative'] == 0]

# For each work level, find minimum summer required
frontier = feasible.groupby('work')['summer'].min().reset_index()

frontier
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
      <th>work</th>
      <th>summer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>6500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>100</td>
      <td>5500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>200</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>300</td>
      <td>4000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>400</td>
      <td>3000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>500</td>
      <td>2500</td>
    </tr>
    <tr>
      <th>6</th>
      <td>600</td>
      <td>1500</td>
    </tr>
    <tr>
      <th>7</th>
      <td>700</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>800</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>900</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.plot(frontier['work'], frontier['summer'])
plt.xlabel('Monthly Work Income')
plt.ylabel('Minimum Summer Savings Required')
plt.title('Income Tradeoff Frontier')
plt.show()
```


    
![png](main_files/main_29_0.png)
    


Given my pessimistic cost and revenue scenarios (planning for the worst): in order to avoid any months in the red, I need to have either 6500 in savings to start, or earn 800 every month of the school year. In the middle, I'd need 4000 to start and 300/month from work, or 3000 to start and 400/month from work.

## Residence Cost Comparison

An addendum: I compare costs by residence for a fuller view of comparisons between residences.


```python
residence_costs = {'Woodsworth': [7497, 5118], 'Chestnut': [7804+3862, 5203+3062], 'Knox': [9245, 6651], 'New College': [14965/2, 14965/2], 'Oak': [12167, 12167], 'Trinity': [11284, 11284], 'University College': [8274, 8274]}
res_results = []

for residence in residence_costs:
    costs = pess_costs.copy()

    costs.loc['2026-09-01', 'Residence'] = residence_costs[residence][0] # First residence payment
    costs.loc['2027-01-01', 'Residence'] = residence_costs[residence][1] # Second residence payment
    
    if residence != 'Woodsworth':
        costs['Food'] = 100 # not 0, because of misc food costs like beverages, snacks, and so on.     
    
    res_net = build_net(pess_rev, costs, pess_starting_balance)
    min_balance = int(res_net['Running Balance'].min())
    
    res_results.append({
        'residence': residence,
        'min_balance': min_balance,
        'final_balance': res_net['Running Balance'].iloc[-1],
        'months_negative': (res_net['Running Balance'] < 0).sum(),
        'max_deficit': res_net['Net'].min(),
    })
residence_comp = pd.DataFrame(res_results)
residence_comp.sort_values('min_balance', ascending=False)
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
      <th>residence</th>
      <th>min_balance</th>
      <th>final_balance</th>
      <th>months_negative</th>
      <th>max_deficit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>New College</td>
      <td>2406</td>
      <td>2406.0</td>
      <td>0</td>
      <td>-3014.5</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Woodsworth</td>
      <td>1956</td>
      <td>1956.0</td>
      <td>0</td>
      <td>-1078.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Knox</td>
      <td>1475</td>
      <td>1475.0</td>
      <td>0</td>
      <td>-2183.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>University College</td>
      <td>823</td>
      <td>823.0</td>
      <td>0</td>
      <td>-3806.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Chestnut</td>
      <td>-2559</td>
      <td>-2560.0</td>
      <td>4</td>
      <td>-3797.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Trinity</td>
      <td>-5196</td>
      <td>-5197.0</td>
      <td>4</td>
      <td>-6816.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Oak</td>
      <td>-6962</td>
      <td>-6963.0</td>
      <td>4</td>
      <td>-7699.0</td>
    </tr>
  </tbody>
</table>
</div>



We can see that, among the residence options available to me, New College is most economical; after that, Woodsworth, Knox, and UC The difference between these options is 1583 in remaining balance at the end of the year, but I will still have a decent amount (823, at a minimum) left over. The first three options especially are nigh interchangable depending on how I value their other offerings. Since, personally, I don't value left-over money that much over a better residence life, it makes sense to rank these four options based primarily on other factors, with limited weighting (though non-zero) for left-over cash. The rest of the options are unviable, as they put me in the red, of course.

## Weighted Comparison

A weighted comparison using other categories too. I had the residences ranked in each category using three LLMS: Claude 4.6, GPT-5.3, and Deepseek. Categories and my weightings:

1. Cost - 0.05 (low-weighted because of marginal differences between the specific residences picked)
2. Room Quality & Amenities - 0.5
3. Community - 0.2
4. Food - 0.2 (prefer kitchen)
5. Location - 0.05


```python
residence_rankings = {'Woodsworth': [2, 1, 3, 1, 3], 'Knox': [3, 4, 4, 3, 4], 'UC': [4, 3, 1, 4, 1], 'New College': [1, 2, 2, 2, 2]} # in order specified above
pd.DataFrame.from_dict(data=residence_rankings, orient='index', columns=['Cost', 'Room & Amenities', 'Community', 'Food', 'Location'])
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
      <th>Cost</th>
      <th>Room &amp; Amenities</th>
      <th>Community</th>
      <th>Food</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Woodsworth</th>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Knox</th>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>UC</th>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>New College</th>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
weightings = [0.05, 0.5, 0.2, 0.2, 0.05]
weighted_rankings = {}

for residence in residence_rankings:
    ranking = sum([a * b for a, b in zip(residence_rankings[residence], weightings)])
    weighted_rankings[residence] = ranking

res_ranks = pd.DataFrame.from_dict(weighted_rankings, orient='index', columns=['Weighted Ranking'])
res_ranks.sort_values('Weighted Ranking')
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
      <th>Weighted Ranking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Woodsworth</th>
      <td>1.55</td>
    </tr>
    <tr>
      <th>New College</th>
      <td>1.95</td>
    </tr>
    <tr>
      <th>UC</th>
      <td>2.75</td>
    </tr>
    <tr>
      <th>Knox</th>
      <td>3.75</td>
    </tr>
  </tbody>
</table>
</div>



We see that according to my weighted preferences across categories and the rankings of these residences on those categories, that Woodsworth is my top choice, followed by NC, UC, and Knox. Rankings were ordinal, not cardinal, so the gap between residences is not properly expressed, but it functions decently generally.
