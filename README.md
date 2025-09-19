### EX3 Implementation of GSP Algorithm In Python
### NAME - PRIYANKA K
### REG NO - 212223230162
### AIM: To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>

### Program:

```python
from collections import defaultdict

def is_subsequence(pattern, sequence):
    i, j = 0, 0
    while i < len(pattern) and j < len(sequence):
        if set(pattern[i]).issubset(set(sequence[j])):
            i += 1
        j += 1
    return i == len(pattern)

def count_support(candidates, dataset, min_support):
    freq = {}
    for cand in candidates:
        count = sum(1 for seq in dataset if is_subsequence(cand, seq))
        if count >= min_support:
            freq[tuple(tuple(x) for x in cand)] = count
    return freq

def generate_candidates(frequent_patterns, k):
    candidates = []
    patterns = list(frequent_patterns.keys())
    for p1 in patterns:
        for p2 in patterns:
            if p1[1:] == p2[:-1]:
                new_pattern = list(p1) + [p2[-1]]
                if len(new_pattern) == k:
                    candidates.append(new_pattern)
    return candidates

def gsp(dataset, min_support):
    items = {item for seq in dataset for itemset in seq for item in itemset}
    candidates = [[[i]] for i in items]
    k = 1
    all_freq = {}
    while candidates:
        freq = count_support(candidates, dataset, min_support)
        if not freq:
            break
        all_freq.update(freq)
        k += 1
        candidates = generate_candidates(freq, k)
    return all_freq

dataset = [
    [["a","b","c"], ["b","e"], ["c","f","g"], ["a","b","e"]],   
    [["a","d"], ["b","c"], ["c"], ["f","g"], ["c","h"]],       
    [["b","c"], ["a","d"], ["e","b","f"], ["c","d","f","g","h"]], 
    [["c"], ["e","c"], ["e","h"]]                             
]

dataset2 = [
    [["b","d"], ["c","b"], ["a","c"]],  
    [["b","f"], ["c","e"], ["b"], ["f","g"]],       
    [["a","h"], ["b","f"], ["a","b","f"]],
    [["b","e"], ["c","e"], ["d"]],   
    [["a"], ["b","d"], ["b","c","b"], ["a","d","e"]]
]

min_support = 3

results = gsp(dataset, min_support)
print("Frequent Sequential Patterns for Dataset 1:")
for pattern, support in results.items():
    print(f"Pattern: {pattern}, Support: {support}")

results = gsp(dataset2, min_support)
print("\nFrequent Sequential Patterns for Dataset 2:")
for pattern, support in results.items():
    print(f"Pattern: {pattern}, Support: {support}")

```
### Output:

<img width="481" height="583" alt="image" src="https://github.com/user-attachments/assets/a61345f4-5477-4746-a543-ff50330f2b88" />


### Visualization:
```python
import matplotlib.pyplot as plt

def visualize_patterns_line(result, title):
    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(12, 6))
        plt.plot([ "".join([f"({','.join(x)})" for x in pattern]) for pattern in patterns ],
                 support, marker='o', linestyle='-', color='blue')
        plt.xlabel('Patterns')
        plt.ylabel('Support Count')
        plt.title(f'Frequent Sequential Patterns - {title}')
        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()
    else:
        print(f"No frequent sequential patterns found for {title}.")

visualize_patterns_line(results, 'Dataset 1')

```
### Output:
<img width="991" height="496" alt="image" src="https://github.com/user-attachments/assets/599b1c67-4183-4a7b-8690-80d8d0cb1cc7" />



### Result:
Hence, Successfully implemented the GSP Algorithm in Python.
