# Destination City
##### *LeetCode | Greedy Coding Problem | Easy Difficulty*


## Problem Statement
<div align='justify';>
You are given the array paths, where paths[i] = [cityA<sub>i</sub>, cityB<sub>i</sub>] means there exists a direct path going from cityAi to cityB<sub>i</sub>. Return the destination city, that is, the city without any path outgoing to another city. It is guaranteed that the graph of paths forms a line without any loop, therefore, there will be exactly one destination city.
</div>

## Expectations
```
Given: [["London", "New York"], ["New York", "Lima"], ["Lima", "Sao Paulo"]]
Output: "Sao Paulo"
```

## Solution Process
* Two sets were first created to differentiate the "starting" and "ending" nodes along each path.
* The **intersection** between the ```end``` and ```start``` sets was then determined, and later **subtracted** from the ```end``` set. 
* The ```return``` statement showed the single destination city as required.

```python
def destCity(path):
    start, end = set(), set()
    for connection in path:
        start.add(connection[0])
        end.add(connection[1])
    dest = end.difference(start.intersection(end))
    return next(iter(dest))
```
