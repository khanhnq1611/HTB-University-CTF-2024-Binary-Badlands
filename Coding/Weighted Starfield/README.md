The way to solve problem is kind of easy: just find the subarray with the maximun product.

So, It is a easy problem about dynamic programming type. All the things we need to do is find the

maximum product at the index i (local max) and  global max = max( local max, global max)

We also need a local min too, because in the case the element at index i is negative, we need to swap local 

max and local min. 

example a[i] = -10

local min = -3
local max = 5

so we need to swap 2 values and we will get local max is 30, if not swap, local max is become -50, bad idea!



```python
# Input two arrays as strings
signals = input()  # First input array as a string
weights = input()  # Second input array as a string

# Write your solution below and make sure to print the maximum stability score

# Remove brackets and split the strings into integer lists
signals = list(map(int, signals.strip("[]").split(',')))
weights = list(map(int, weights.strip("[]").split(',')))

# Compute the modified_signals array
modified_signals = [signals[i] * weights[i] for i in range(len(signals))]

# Maximum Product Subarray Algorithm
result = modified_signals[0]  # Global max
max_product = modified_signals[0]  # Local max
min_product = modified_signals[0]  # Local min

for i in range(1, len(modified_signals)):
    if modified_signals[i] < 0:
        max_product, min_product = min_product, max_product
        # Swap when encountering a negative number because negative num multiply with min_product may bigger than that with max_product

    max_product = max(modified_signals[i], max_product * modified_signals[i])
    min_product = min(modified_signals[i], min_product * modified_signals[i])

    result = max(result, max_product)  # Update the global maximum

# Output the result
print(result)
# FLag HTB{m1ssi0n_c0mpl3t3d_m4x1mum_5t4b1l1ty_4ch13v3d!_01124437b14528e3159420a6721aa524}
```
