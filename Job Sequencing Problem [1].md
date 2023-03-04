# Job Sequencing Problem [1]
#### *H. A. Taha, “Heuristic Programming: Problems,” in Operations research: An Introduction, 10th ed., Harlow, England: Pearson Educatin, 2017, p. 426*

## Problem
<div align='justify';>
Apply a Tabu Search Algorithm to solve the 5-job sequencing problem using the data below: <br /><br />
</div>

|      Job      | Processing Time (days)| Due Date (days) | Holding Cost ($/day) | Penalty Cost ($/day) |
| :-----: | :------: | :------: | :------: | :------: |
| 1       | 10       | 12       | 3       | 10       |
| 2   	  | 12       | 30       | 1       | 20       |
| 3       | 5        | 9        | 5       | 12       |
| 4       | 7        | 25       | 2       | 8        |
| 5       | 9        | 40       | 4       | 15       |

<div align='justify';>
We want to find the sequence of jobs that will minimize total cost
</div>

## Modeling
### What is the Tabu Search Algorithm?

<div align='justify';>
Unlike linear programming, the Tabu Search Algorithm is a metaheuristic programming which is designed to find <b>good and approximate</b> solutions to complex combinatorial problems. Its defining characteristic lies on its <b>long-term memory</b> which stores explored solutions and avoids revisiting them to prevent certain modifications that lead to suboptimal solutions.
</div>

### Elements

1. **Number of Iterations, k**: Determines how long should the program run. Ideally, more iterations increase the chances of getting a near-optimal solution.
2. **Tenure Period, tau**: The number of iterations for which a solution or is considered forbidden in the search. It determines the balance between exploration and exploitation in the search. Forbidden solutions are tracked through a **tabu list**.
4. **Process of exchanging positions**: Up to the prerogative of the analyst, the swapping process drives which sequence contributes to the least cost. In this activity, the swapping of pairs is set randomly.

## Python Implementation

**1. Import Pandas Library to display a dataframe of the givens
```python
import pandas as pd
import numpy as np
```

**2. Create a dataframe of the necessary data**
```python
data = {'Jobs': [1, 2, 3, 4, 5],
        'Process Times': [10, 12, 5, 7, 9],
        'Due Dates': [12, 30, 9, 25, 40],
        'Holding Costs': [3, 1, 5, 2, 4],
        'Penalty Costs': [10, 20, 12, 8, 15]}
df = pd.DataFrame(data)

job = df.index.to_list()
```

**3. Create a function that overseers the pair swapping*
```python
def generate_switch(current, tabu_list):
    neighbor = current.copy()
    i, j = np.random.choice(5, 2, replace=False)
    neighbor[i], neighbor[j] = neighbor[j], neighbor[i]
    if tuple(neighbor) in tabu_list:
        return current
    return neighbor
```

**4. Solve the total cost under a specific sequence**
```python
def generate_cost(sequence):
    df_sol = df.copy()
    df_sol = df_sol.reindex(sequence)
    df_sol['Completion Dates'] = df_sol['Process Times'].cumsum()

    df_sol['Holding Time'] = df_sol['Due Dates'] - df_sol['Completion Dates']
    df_sol['Holding Time'] = df_sol['Holding Time'].apply(lambda x: 0 if x < 0 else x)

    df_sol['Delay Time'] = df_sol['Completion Dates'] - df_sol['Due Dates']
    df_sol['Delay Time'] = df_sol['Delay Time'].apply(lambda x: 0 if x < 0 else x)

    cost = np.sum(df_sol['Holding Costs'] * df_sol['Holding Time']) + np.sum(df_sol['Penalty Costs'] * df_sol['Delay Time'])
    return cost
```

**5. Implement the Tabu Search**
```python
def tabu(jobs, iterations, tenure):
    best_cost = generate_cost(jobs)
    best_sequence = jobs

    tabu_list = [tuple(best_sequence)]

    for i in range(iterations):
        current = generate_switch(best_sequence, tabu_list)
        current_cost = generate_cost(current)

        if current_cost <= best_cost:
            best_cost = current_cost
            best_sequence = current

        if len(tabu_list) >= tenure:
            tabu_list.pop(0)
        tabu_list.append(tuple(current))

    return best_sequence, best_cost
```

**6. Set the parameters and run the process**
```python
k = 1000
t = 3

df_seq, df_cost = tabu(job, k, t)
df_out = df.reindex(df_seq)

print('The sequence', df_out['Jobs'].to_list(), 'has the lowest cost with $%g' % (df_cost))
```


## Results and Conclusion
```
Jobs  Process Times  Due Dates  Holding Costs  Penalty Costs
  1         10          12            3             10
  2         12          30            1             20
  3          5           9            5             12
  4          7          25            2              8
  5          9          40            4             15 

The sequence [3, 1, 2, 4, 5] has the least cost with $170
```
