# Task Scheduler
##### *LeetCode | Greedy Coding Problem | Medium Difficulty*


## Problem Statement
<div align='justify';>
Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle. <br /><br />

However, there is a non-negative integer ```n``` that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least ```n``` units of time between any two same tasks.<br /><br />

Return the least number of units of times that the CPU will take to finish all the given tasks.

</div>

## Expectations
```
Given: tasks = ["A","A","B","B","B","B"], n = 2
Output: 10 (B -> A -> idle -> B -> A -> idle -> B -> A -> idle -> B)
```

## Solution Process
* Different cases were initially observed by varying the values of ```n``` and number of unique elements. Though there may be multiple answers, having the shortest sequence is of the essence.
* It was deduced that an equation can be used to solve the least number of units of times that the CPU will take to finish all the given tasks.
```
If m = frequency of the most frequent task, x = number of the most frequent tasks, n = cooldown period, and T = total time
   T = (m - 1)*(1 + n) + x
```
* It is also possible to simply take the length of the list given that the entire sequence needs no idle times.
* The ```return``` statement showed the minimum length of the sequence.

```python
def leastInterval(tasks, n):
    if n == 0:
        return len(tasks)
    else:
        frequencies = Counter(tasks)
        m = frequencies.most_common(1)[0][1]
        x = len([t for t, f in frequencies.items() if f == m])
    return max((m - 1) * (1 + n) + x, len(tasks))
```
