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
        max_product, min_product = min_product, max_product  # Swap when encountering a negative number

    max_product = max(modified_signals[i], max_product * modified_signals[i])
    min_product = min(modified_signals[i], min_product * modified_signals[i])

    result = max(result, max_product)  # Update the global maximum

# Output the result
print(result)
# FLag HTB{m1ssi0n_c0mpl3t3d_m4x1mum_5t4b1l1ty_4ch13v3d!_01124437b14528e3159420a6721aa524}
