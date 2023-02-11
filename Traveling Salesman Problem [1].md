# Warehouse Location Problem [1]
#### *H. A. Taha, “Traveling Salesperson Problem: Problems,” in Operations research: An Introduction, 10th ed., Harlow, England: Pearson Educatin, 2017, pp. 461–462.*

## Problem
<div align='justify';>
A manager has a total of 10 employees working on six projects. Projects are reviewed weekly with each employee. A project may employ more than one employee resulting in  assignment overlaps, as the following table shows: <br /><br />
</div>

|               | Region 1 | Region 2 | Region 3 |
| :-----------: | :------: | :------: | :------: |
| New York      | 20       | 40       | 50       |
| Los Angeles   | 48       | 15       | 26       |
| Chicago       | 26       | 35       | 18       |
| Atlanta       | 24       | 50       | 35       |

<div align='justify';>
We want to meet weekly demands at minimum cost, subject to the preceding information and the following restrictions:
	
1. If the New York warehouse is opened, then the Los Angeles warehouse must be opened.
2. At most two warehouses can be opened.
3. Either the Atlanta or the Los Angeles warehouse must be opened.
	
</div>

## Modeling
### Variable Definitions

Let $x_{ij}$ be the amount of units shipped from city $i$ to region $j$ <br /><br />
Let

$$ y_{i} = \begin{cases}
  \displaystyle 1; & \text{set up warehouse at city $i$} \\
  \displaystyle 0; & \text{else}
\end{cases}$$

such that $i \in {(N, L, C, A)}$ and $j \in {(1, 2, 3)}$

### Objective Function

<div align='justify';>
The objective function is based on two important metrics: (1) the weekly fixed cost in maintaining the warehouse open and (2) the shipping costs to the regions. Given that the function is created from business costs, it is therefore necessary to minimize this model. <br /><br />
</div>

$$ Z = 400y_{N} + 500y_{L} + 300y_{C} + 150y_{A} + \sum\limits_{i} \sum\limits_{j} {c_{ij}x_{ij}} $$ <br /> 

such that $c_{ij}$ refers to the shipping cost from city $i$ to region $j$ for all $i \in {(N, L, C, A)}$ and $j \in {(1, 2, 3)}$ as shown in the table.

### Constraints

**Demand Constraints**: <br />

$$x_{N1}+x_{L1}+x_{C1}+x_{A1} = 80$$ <br />

$$x_{N2}+x_{L2}+x_{C2}+x_{A2} = 70$$ <br />

$$x_{N3}+x_{L3}+x_{C3}+x_{A3} = 40$$ <br />

**Supply Constraints**: <br />

$$x_{N1}+x_{N2}+x_{N3} \le 100y_{N}$$ <br />

$$x_{L1}+x_{L2}+x_{L3} \le 100y_{L}$$ <br />

$$x_{C1}+x_{C2}+x_{C3} \le 100y_{C}$$ <br />

$$x_{A1}+x_{A2}+x_{A3} \le 100y_{A}$$ <br />

**Decision Constraints**: <br />

1. If the New York warehouse is opened, then the Los Angeles warehouse must be opened.

$$y_{N} - y_{L} \le 0$$ 

2. At most two warehouses can be opened.

$$y_{N} + y_{L} + y_{C} + y_{A} \le 2$$ 

3. Either the Atlanta or the Los Angeles warehouse must be opened.

$$y_{L} + y_{A} = 1$$ 


At the core of this algorithm lies the effective use of binary variables to lead the decision-making process on which city should warehouses be built. Here, if $y_{i}$ is 0, then the city will have **no** warehouse, and thus will not produce any units for the regions. Conversely, a value of one permits the production and delivery.
<br /><br />

## Python Implementation

**1. Instantiate the model through the PuLP module**
```python
from pulp import *

model = LpProblem('milp', LpMinimize)
```

**2. Create a dataframe of the necessary data**
```python
import pandas as pd

data_fixcost = {'NewYork': [400],
                'LosAngeles': [500],
                'Chicago': [300],
                'Atlanta': [150]}

data_shipcost = {'Region1': [20, 48, 26, 24],
                 'Region2': [40, 15, 35, 50],
                 'Region3': [50, 26, 18, 35]}

data_demand = {'Region1': [80],
               'Region2': [70],
               'Region3': [40]}

df_shipcost = pd.DataFrame(data_shipcost, index=['NewYork', 'LosAngeles', 'Chicago', 'Atlanta'])
df_fixcost = pd.DataFrame(data_fixcost, index=['FixedCost'])
df_demand = pd.DataFrame(data_demand, index=['Demand'])
```

**3. Create the binary variables**
```python
binvars = {}
for l in df_fixcost.columns:
    binvars[l] = LpVariable(l, 0, 1, LpBinary)
```

**4. Create the continuous variables**
```python
contvars = {}
for l in df_shipcost.index:
    for r in df_shipcost.columns:
        contvars[l, r] = LpVariable('%s_to_%s' % (l, r), 0, None, LpContinuous)
```

**5. Formulate the objective function**
```python
model += sum(df_fixcost[l].item() * binvars[l] for l in df_fixcost.columns) \
         + sum(df_shipcost._get_value(df_shipcost.index.get_loc(l), df_shipcost.columns.get_loc(r), takeable=True) * contvars[(l, r)] for l in df_shipcost.index for r in df_shipcost.columns)
```

**6. Formulate the constraints**
```python
for r in df_shipcost.columns:
    model += sum(contvars[(l, r)] for l in df_shipcost.index) >= df_demand[r].item(), 'Demand Constraint of %s' % (r)
for l in df_shipcost.index:
    model += sum(contvars[(l, r)] for r in df_shipcost.columns) - 100 * binvars[l] <= 0, 'Delivery Amounts of %s' % (l)

model += binvars['NewYork'] - binvars['LosAngeles'] <= 0, 'dc1'
model += sum(binvars[l] for l in df_shipcost.index) <= 2, 'dc2'
model += binvars['LosAngeles'] + binvars['Atlanta'] == 1, 'dc3'
```

**7. Run the module**
```python
model.solve()

for v in model.variables():
    print('%s: %g' % (v.name, v.varValue))
```

## Results and Conclusion

In conclusion, it is recommended for the company to open warehouses in **Chicago** and in **Los Angeles**.

* The Chicago warehouse shall deliver 80 units to Region 1 and 20 units to Region 3.
* The Los Angeles Warehouse shall deliver 70 units to Region 2 and 20 units to Region 3.

This distribution plan will incur a total cost of $4810.

