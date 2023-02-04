# Warehouse Location Problem [1]
#### *W. L. Winston, “Integer Programming: Problems,” in Operations Research: Applications and Algorithms, 4th ed., Belmont , California: Duxbury Press, 2004, p. 503.*

## Problem
<div align='justify';>
A company is considering opening warehouses in four cities: New York, Los Angeles, Chicago, and Atlanta. Each warehouse can ship 100 units per week. The weekly fixed cost of keeping each warehouse open is $400 for New York, $500 for Los Angeles, $300 for Chicago, and $150 for Atlanta. Region 1 of the country requires 80 units per week, region 2 requires 70 units per week, and region 3 requires 40 units per week. The costs (including production and shipping costs) of sending one unit from a plant to a region are shown in the table. <br /><br />
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
```python
from pulp import *
import pandas as pd
```
