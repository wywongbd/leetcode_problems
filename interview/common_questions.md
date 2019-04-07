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

#### Implementing a hash table using chaining
```python
# Implement a hash table
# Collision handling: chaining
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None
        self.pre = None
        
class DoublyLL(object):
    def __init__(self):
        self.head = None
        self.tail = None
        self.N = 0
    
    def size(self):
        return self.N
    
    def append(self, node):
        if self.head:
            self.tail.next = node
            node.pre = self.tail
            self.tail = node
            self.N += 1
        else:
            self.head = node
            self.tail = node
            self.N += 1
    
    def delete(self, node):
        left_node = node.pre
        right_node = node.next
        self.N -= 1
        
        if left_node is None:
            self.head = right_node
        else:
            left_node.next = right_node
            
        if right_node is None:
            self.tail = left_node
        else:
            right_node.pre = left_node

class HashTable(object):
    def __init__(self, N=100):
        self.N = N
        self.buckets = [DoublyLL() for i in range(self.N)]
    
    def hash(self, key):
        return int(key) % self.N 
        
    def add(self, key, value):
        tup = (key, value)
        self.buckets[self.hash(key)].append(Node(tup))
        
    def find(self, key):
        node = self.buckets[self.hash(key)].head
        
        while node:
            _key, _val = node.val
            if key == _key:
                break
            node = node.next
            
        if node:
            return node.val[1]
        else:
            return None
    
    def update(self, key, value):
        node = self.buckets[self.hash(key)].head
        
        while node:
            _key, _ = node.val
            if key == _key:
                node.val = (key, value)
            node = node.next
        
    def delete(self, key):
        bucket = self.buckets[self.hash(key)]
        node = bucket.head
        
        while node:
            _key, _val = node.val
            if key == _key:
                bucket.delete(node)
            node = node.next
            
table = HashTable(100)
table.add(3, "three")
table.add(4, "four")
table.add(5, "five")
table.add(303, "three-hundred-and-three")
table.add(603, "six-hundred-and-three")
```