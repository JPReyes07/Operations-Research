# Maximum Ice Cream Bars
##### *LeetCode | Greedy Coding Problem | Medium Difficulty*


## Problem Statement
<div align='justify';>
It is a sweltering summer day, and a boy wants to buy some ice cream bars. At the store, there are n ice cream bars. You are given an array costs of length n, where costs[i] is the price of the i<sup>th</sup> ice cream bar in coins. The boy initially has coins coins to spend, and he wants to buy as many ice cream bars as possible. 
<br /><br />
Note: The boy can buy the ice cream bars in any order.
<br /><br />
Return the maximum number of ice cream bars the boy can buy with coins coins.
</div>

## Expectation
```
Given: costs = [1,3,2,4,1], coins = 7
Output: 4 (ice cream bars)
```

## Solution Process
* Two variables were first instantiated as zero. The ```total``` refers to the total coins spent while the  ```counter``` keeps track on the number of ice cream bars bought.
* A ```for``` loop was then implemented over the sorted list to determine the maximum number of ice cream bars while taking into account the condition of having to spend less than the given number of coins. 
* The ```return``` statement returned the value of the ```counter``` variable, or the number of ice cream bars.

```python
def maxIceCream(costs, coins):
    total = 0
    counter = 0
    costs.sort()
    for cost in costs:
        total += cost
        if total <= coins and costs[0] <= coins:
            counter += 1
    return counter
```
