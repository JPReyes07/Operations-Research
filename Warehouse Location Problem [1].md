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
Let $y_{i}$ = 

$$\begin{cases}
  \displaystyle 1; & \text{set up warehouse at city $i$} \\
  \displaystyle 0; & \text{else}
\end{cases}$$


## Python Implementation
```python
from pulp import *
import pandas as pd
```
