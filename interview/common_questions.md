#### Find non-repeated item (everything else must be repeated once!!)
```python
ls = [1, 2, 1, 3, 5, 3, 5]
number = ls[0]

for i in range(1, len(ls)):
    # bitwise exclusive OR
    number = number ^ ls[i]

print(number)
```

#### First non-repeating character using one traversal of string
```python
from collections import OrderedDict 

duplicates = set()
unique_keys = OrderedDict()
string = "abcdefghzabcdefghabcdegfi"

for i, char in enumerate(string):
    if char in duplicates:
        continue
    elif char in unique_keys:
        unique_keys.pop(char)
        duplicates.add(char)
    else:
        unique_keys[char] = True
        
# last=False means FIFO
print(unique_keys)
print(unique_keys.popitem(last=False))
```
Note that **OrderedDict** is actually **Doubly Linked List + Hash Table**.