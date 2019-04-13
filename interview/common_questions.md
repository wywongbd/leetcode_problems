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

#### Reverse a linked list
```python
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None
    
class LL(object):
    def __init__(self):
        self.head = None
        self.tail = None
        
    def append(self, value):
        if self.head is None:
            self.head = Node(value)
            self.tail = self.head
        else:
            self.tail.next = Node(value)
            self.tail = self.tail.next
    
    def reverse(self):
        if self.head:
            # update tail variable
            self.tail = self.head
            self._reverse(None, self.head)
    
    def _reverse(self, pre, node):
        if node is None:
            # reached end of linked list
            self.head = pre
            return
        else:
            next_node = node.next
            node.next = pre
            self._reverse(node, next_node)
            
    def show(self):
        ls = []
        node = self.head
        while node:
            ls.append(node.val)
            node = node.next
        print(ls)
```

#### Union Find data structure
With **union by rank**, we can ensure O(log N) for **find()**.
```python
class DisjointSet(object):
    def __init__(self):
        self.parent = {}
        self.rank = {}
    
    def find(self, item):
        if item not in self.parent:
            return None
        
        else:
            while item != self.parent[item]:
                self.parent[item] = self.parent[self.parent[item]]
                item = self.parent[item]
            return item
        
    def union(self, item1, item2):
        if (item1 not in self.parent) or (item2 not in self.parent):
            return
        
        root1, root2 = self.find(item1), self.find(item2)
        
        if root1 == root2:
            return
        
        if self.rank[root1] < self.rank[root2]:
            self.parent[root1] = root2
            
        elif self.rank[root1] > self.rank[root1]:
            self.parent[root2] = root1
            
        else:
            self.parent[root1] = root2
            self.rank[root2] += 1
    
    def makeset(self, item):
        if item not in self.parent:
            self.parent[item] = item
            self.rank[item] = 1
    
    def connected(self, item1, item2):
        if (item1 in self.parent) and (item2 in self.parent):
            return self.find(item1) == self.find(item2)
        else:
            return False
```