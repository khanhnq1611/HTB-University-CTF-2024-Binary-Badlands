# Conflict Cruncher - Very Easy Level
This is an easy problem in python, just update the merged dict, nothing special here!


```python
# Input two dictionaries as strings
dict1_str = input()  
dict2_str = input()  

# Parse the dictionaries using eval
dict1 = eval(dict1_str)
dict2 = eval(dict2_str)

# Merge the dictionaries using the Frontier Protocol
merged_dict = dict1.copy()
merged_dict.update(dict2)

# Print the merged dictionary
print(merged_dict)
#HTB{n0w_1m_0ff1c4lly_4_c0nfl1ct_crunch3r_y4y!_392a6d262fddc984eccb29aa00a79b8b}
```
