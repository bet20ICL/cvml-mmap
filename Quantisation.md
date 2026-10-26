
We have a set of numbers

We want to store them with less precision

Define a small set of levels

Approximate each number with the closest level

How to decide what the levels are? 

Naive way:
- define a fixed range: the min and max of the set
- define a budget: a fixed number of levels
- evenly distribute levels across the range
- for $n$ levels need approx. $log_2(n)$ bits
- store a short integer per number
- store a scale factor + offset per set
- recover original number by $a x + b$

Better:
- what if numbers are not evenly distributed? 