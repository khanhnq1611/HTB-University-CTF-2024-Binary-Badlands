```python
# Input the text as a single string
input_text = input()  # Example: "The quick brown fox jumps over the lazy dog."

# Split the input text into words and normalize to lowercase
words = input_text.lower().split()

# Remove punctuation from words
import string
words = [word.strip(string.punctuation) for word in words]

# Count the frequency of each word
word_counts = {}
for word in words:
    word_counts[word] = word_counts.get(word, 0) + 1

# Find the most common word
most_common_word = max(word_counts, key=word_counts.get)

# Print the most common word
print(most_common_word)
#HTB{pfupp_wh0_m4d3_th353_345y_ch4ll3ng35_ch1ld1sh!_8d7cb1565c85d3bb3dee51704a7b2a4a}

```
