EXP 1: Recursion vs Iteration + Time
import time

# Factorial Recursive
def factorial_recursive(n):
    if n == 0 or n == 1:
        return 1
    return n * factorial_recursive(n-1)

# Factorial Iterative
def factorial_iterative(n):
    res = 1
    for i in range(2, n+1):
        res *= i
    return res

# Fibonacci Recursive
def fib_recursive(n):
    if n == 0: return 0
    if n == 1: return 1
    return fib_recursive(n-1) + fib_recursive(n-2)

# Fibonacci Iterative
def fib_iterative(n):
    a, b = 0, 1
    for _ in range(n):
        print(a, end=" ")
        a, b = b, a + b
    print()

# Time Calculation
start = time.time()
print("Factorial Rec:", factorial_recursive(5))
end = time.time()
print("Time:", end-start)

# Example
n = 5

print("Factorial Recursive:", factorial_recursive(n))
print("Factorial Iterative:", factorial_iterative(n))

print("Fibonacci Iterative:", end=" ")
fib_iterative(10)

print("Fibonacci Recursive (first 5):", end=" ")
for i in range(5):
    print(fib_recursive(i), end=" ")
print()
EXP2Array (Static & Dynamic)
# Static Array
class StaticArray:
    def __init__(self, capacity):
        self.capacity = capacity
        self.size = 0
        self.arr = [None] * capacity

    def append(self, value):
        if self.size == self.capacity:
            raise Exception("Array Full")
        self.arr[self.size] = value
        self.size += 1

    def display(self):
        print(self.arr[:self.size])

# Dynamic Array
class DynamicArray:
    def __init__(self):
        self.capacity = 2
        self.size = 0
        self.arr = [None] * self.capacity

    def resize(self, new_cap):
        new_arr = [None] * new_cap
        for i in range(self.size):
            new_arr[i] = self.arr[i]
        self.arr = new_arr
        self.capacity = new_cap

    def append(self, value):
        if self.size == self.capacity:
            self.resize(self.capacity * 2)
        self.arr[self.size] = value
        self.size += 1

    def display(self):
        print(self.arr[:self.size])

# Static Array Example
sa = StaticArray(5)
for i in range(5):
    sa.append(i)
print("Static Array:")
sa.display()

# Dynamic Array Example
da = DynamicArray()
for i in range(10):
    da.append(i)
print("Dynamic Array:")
da.display()
EXP 3: Stack, Queue + Applications
# Stack
class Stack:
    def __init__(self, n):
        self.arr = [None]*n
        self.top = -1

    def push(self, x):
        self.top += 1
        self.arr[self.top] = x

    def pop(self):
        x = self.arr[self.top]
        self.top -= 1
        return x

# Queue (Circular)
class Queue:
    def __init__(self, n):
        self.arr = [None]*n
        self.front = self.rear = -1
        self.n = n

    def enqueue(self, x):
        if self.front == -1:
            self.front = 0
        self.rear = (self.rear + 1) % self.n
        self.arr[self.rear] = x

    def dequeue(self):
        x = self.arr[self.front]
        if self.front == self.rear:
            self.front = self.rear = -1
        else:
            self.front = (self.front + 1) % self.n
        return x

# Infix to Postfix
def precedence(op):
    if op in '+-': return 1
    if op in '*/': return 2
    return 0

def infix_to_postfix(expr):
    stack = []
    res = ""
    for ch in expr:
        if ch.isalnum():
            res += ch
        elif ch == '(':
            stack.append(ch)
        elif ch == ')':
            while stack[-1] != '(':
                res += stack.pop()
            stack.pop()
        else:
            while stack and precedence(ch) <= precedence(stack[-1]):
                res += stack.pop()
            stack.append(ch)
    while stack:
        res += stack.pop()
    return res

# Stack Example
s = Stack(5)
s.push(10)
s.push(20)
s.push(30)
print("Stack Pop:", s.pop())

# Queue Example
q = Queue(5)
q.enqueue(1)
q.enqueue(2)
q.enqueue(3)
print("Queue Dequeue:", q.dequeue())

# Infix → Postfix Example
expr = "(A+B)*C"
print("Postfix:", infix_to_postfix(expr))
EXP 4: Linked List
# Node Class
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Linked List
class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def insert_end(self, data):
        new = Node(data)
        if not self.head:
            self.head = new
            return
        temp = self.head
        while temp.next:
            temp = temp.next
        temp.next = new

    # ✅ find_item added
    def find_item(self, x):
        temp = self.head
        while temp:
            if temp.data == x:
                return temp
            temp = temp.next
        return None

    # ✅ delete_node added
    def delete_node(self, x):
        temp = self.head

        if temp and temp.data == x:
            self.head = temp.next
            return

        prev = None
        while temp and temp.data != x:
            prev = temp
            temp = temp.next

        if temp:
            prev.next = temp.next

    def display(self):
        temp = self.head
        while temp:
            print(temp.data, end=" -> ")
            temp = temp.next
        print("None")

# 🔥 Example
ll = SinglyLinkedList()
ll.insert_end(10)
ll.insert_end(20)
ll.insert_end(30)

print("Linked List:")
ll.display()

node = ll.find_item(20)
print("Found:", node.data if node else "Not Found")

ll.delete_node(20)
print("After Deletion:")
ll.display()
EXP 5: Binary Tree & BST
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None

    def insert(self, data):
        self.root = self._insert(self.root, data)

    def _insert(self, node, data):
        if not node:
            return Node(data)
        if data < node.data:
            node.left = self._insert(node.left, data)
        else:
            node.right = self._insert(node.right, data)
        return node

    # ✅ search added
    def search(self, key):
        return self._search(self.root, key)

    def _search(self, node, key):
        if not node:
            return False
        if node.data == key:
            return True
        elif key < node.data:
            return self._search(node.left, key)
        else:
            return self._search(node.right, key)

    def inorder(self, node):
        if node:
            self.inorder(node.left)
            print(node.data, end=" ")
            self.inorder(node.right)

# 🔥 Example
bst = BST()
vals = [50,30,70,20,40,60,80]

for v in vals:
    bst.insert(v)

print("BST Inorder:")
bst.inorder(bst.root)

print("\nSearch 40:", bst.search(40))
print("Search 90:", bst.search(90))
EXP 6: Min Heap & Max Heap
# Min Heap
class MinHeap:
    def __init__(self):
        self.heap = []

    def insert(self, val):
        self.heap.append(val)
        i = len(self.heap) - 1
        while i > 0 and self.heap[(i-1)//2] > self.heap[i]:
            self.heap[i], self.heap[(i-1)//2] = self.heap[(i-1)//2], self.heap[i]
            i = (i-1)//2

    def extract_min(self):
        if not self.heap:
            return None
        root = self.heap[0]
        self.heap[0] = self.heap.pop()
        self.heapify(0)
        return root

    def heapify(self, i):
        l, r = 2*i+1, 2*i+2
        smallest = i
        if l < len(self.heap) and self.heap[l] < self.heap[smallest]:
            smallest = l
        if r < len(self.heap) and self.heap[r] < self.heap[smallest]:
            smallest = r
        if smallest != i:
            self.heap[i], self.heap[smallest] = self.heap[smallest], self.heap[i]
            self.heapify(smallest)

h = MinHeap()

nums = [5,3,8,1,2]
for n in nums:
    h.insert(n)

print("Heap:", h.heap)

print("Extract Min:", h.extract_min())
print("After Extraction:", h.heap)
EXP 7: AVL Tree
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

def height(n):
    return n.height if n else 0

def rotate_right(y):
    x = y.left
    t = x.right
    x.right = y
    y.left = t
    y.height = 1 + max(height(y.left), height(y.right))
    x.height = 1 + max(height(x.left), height(x.right))
    return x

def rotate_left(x):
    y = x.right
    t = y.left
    y.left = x
    x.right = t
    x.height = 1 + max(height(x.left), height(x.right))
    y.height = 1 + max(height(y.left), height(y.right))
    return y

def insert(root, key):
    if not root:
        return Node(key)
    if key < root.key:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)

    root.height = 1 + max(height(root.left), height(root.right))
    balance = height(root.left) - height(root.right)

    if balance > 1 and key < root.left.key:
        return rotate_right(root)
    if balance < -1 and key > root.right.key:
        return rotate_left(root)

    return root

root = None

vals = [10,20,30,40,50]
for v in vals:
    root = insert(root, v)

print("AVL Tree Inserted (Root):", root.key)
EXP 8: Sorting Algorithms
def bubble_sort(a):
    for i in range(len(a)):
        for j in range(len(a)-i-1):
            if a[j] > a[j+1]:
                a[j], a[j+1] = a[j+1], a[j]

def insertion_sort(a):
    for i in range(1, len(a)):
        key = a[i]
        j = i-1
        while j >= 0 and key < a[j]:
            a[j+1] = a[j]
            j -= 1
        a[j+1] = key

def selection_sort(a):
    for i in range(len(a)):
        min_idx = i
        for j in range(i+1, len(a)):
            if a[j] < a[min_idx]:
                min_idx = j
        a[i], a[min_idx] = a[min_idx], a[i]

def merge_sort(a):
    if len(a) > 1:
        mid = len(a)//2
        L = a[:mid]
        R = a[mid:]

        merge_sort(L)
        merge_sort(R)

        i=j=k=0
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                a[k]=L[i]; i+=1
            else:
                a[k]=R[j]; j+=1
            k+=1

        while i < len(L):
            a[k]=L[i]; i+=1; k+=1
        while j < len(R):
            a[k]=R[j]; j+=1; k+=1

arr = [5,2,9,1,6]

print("Original:", arr)

bubble_sort(arr.copy())
print("Bubble:", arr)

arr = [5,2,9,1,6]
insertion_sort(arr)
print("Insertion:", arr)

arr = [5,2,9,1,6]
selection_sort(arr)
print("Selection:", arr)

arr = [5,2,9,1,6]
merge_sort(arr)
print("Merge:", arr)
LAB_MIDTERM
1a) Linked list ADT
#️⃣ Q1: Linked List ADT + Union

🔹 Core Methods
create_list() → initialize head = None
insert_node(x) → insert at end (O(n))
delete_node(x) → search & remove (O(n))
find_item(x) → return node reference (O(n))
display() → traverse & print
🔹 Key Logic (Union L1 ∪ L2)
Traverse L1 → add all to L3
Traverse L2 → add only if not found in L3
Use find_item() to avoid duplicates

👉 Concept: Remove duplicates while merging
👉 Complexity: O(n²) (due to search)
# Node Class
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Linked List ADT
class LinkedList:
    def __init__(self):
        self.head = None

    # create_list()
    def create_list(self, lst):
        for x in lst:
            self.insert_node(x)

    # insert_node(x)
    def insert_node(self, x):
        new = Node(x)
        if not self.head:
            self.head = new
            return
        temp = self.head
        while temp.next:
            temp = temp.next
        temp.next = new

    # delete_node(x)
    def delete_node(self, x):
        temp = self.head
        if temp and temp.data == x:
            self.head = temp.next
            return
        prev = None
        while temp and temp.data != x:
            prev = temp
            temp = temp.next
        if temp:
            prev.next = temp.next

    # find_item(x)
    def find_item(self, x):
        temp = self.head
        while temp:
            if temp.data == x:
                return temp   # #returns reference
            temp = temp.next
        return None

    # display()
    def display(self):
        temp = self.head
        res = []
        while temp:
            res.append(temp.data)
            temp = temp.next
        print(res)
1b union of two linked list
# create L1 and L2
L1 = LinkedList()
L2 = LinkedList()

L1.create_list([45,35,12,60,65,40,30,63,70])
L2.create_list([23,34,12,20,25,65,30,64,70])

# create L3 = union
L3 = LinkedList()

# insert from L1
temp = L1.head
while temp:
    L3.insert_node(temp.data)
    temp = temp.next

# insert from L2 (avoid duplicates)
temp = L2.head
while temp:
    if not L3.find_item(temp.data):   # #check duplicate
        L3.insert_node(temp.data)
    temp = temp.next

print("L3 =", end=" ")
L3.display()
2.sorted doubly linked list
Q2: Doubly Linked List (Sorted + Remove Duplicates)

🔹 insert_sorted(x)
Traverse until correct position
Update prev and next pointers
Maintain sorted order
🔹 remove_duplicates_sorted()
Traverse once
If curr.data == curr.next.data → delete next node
Only pointer updates (no extra space)
🔹 display_forward()
Traverse from head → print

👉 Concept: Sorted insertion + in-place duplicate removal
👉 Complexity: O(n)
class Node:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None

    # insert_sorted(x)
    def insert_sorted(self, x):
        new = Node(x)

        # insert at beginning
        if not self.head or x < self.head.data:
            new.next = self.head
            if self.head:
                self.head.prev = new
            self.head = new
            return

        temp = self.head
        while temp.next and temp.next.data < x:
            temp = temp.next

        new.next = temp.next
        if temp.next:
            temp.next.prev = new
        temp.next = new
        new.prev = temp

    # remove duplicates
    def remove_duplicates_sorted(self):
        temp = self.head
        while temp and temp.next:
            if temp.data == temp.next.data:
                temp.next = temp.next.next
                if temp.next:
                    temp.next.prev = temp
            else:
                temp = temp.next

    # display
    def display_forward(self):
        temp = self.head
        while temp:
            print(temp.data, end=" ")
            temp = temp.next
        print()

# Driver
dll = DoublyLinkedList()
arr = [7,3,9,3,5,2,9,1,5,4]

for x in arr:
    dll.insert_sorted(x)

dll.remove_duplicates_sorted()

print("Final sorted list after removing duplicates:")
dll.display_forward()
3a. Stack using Linked List
Q3: Stack using Linked List + Merge Logic

🔹 Stack Operations
push(x) → insert at head (O(1))
pop() → remove head (O(1))
peek() → return head data
display() → traverse
🔹 Key Logic (S1, S2 → S3)
Separate:
negatives
positives
Negatives → sort descending
Positives → sort ascending
Push into S3 in order

👉 Concept: Order manipulation using stack
👉 Trick: Think like sorting + grouping
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class Stack:
    def __init__(self):
        self.top = None

    def push(self, x):
        new = Node(x)
        new.next = self.top
        self.top = new

    def pop(self):
        if not self.top:
            return None
        x = self.top.data
        self.top = self.top.next
        return x

    def peek(self):
        return self.top.data if self.top else None

    def display(self):
        temp = self.top
        while temp:
            print(temp.data, end=" ")
            temp = temp.next
        print()
3b. Merge Stacks into S3
S1 = [-5,-10,-1,8,15,20]
S2 = [-7,-4,-9,3,17,40]

# separate negatives & positives
neg = []
pos = []

for x in S1+S2:
    if x < 0:
        neg.append(x)
    else:
        pos.append(x)

neg.sort(reverse=True)   # #descending negatives
pos.sort()               # #ascending positives

S3 = neg + pos

print("S3 =", S3)
4a & 4b. BST
Core Methods
insert(x) → maintain BST property
delete(x) → handle:
leaf
1 child
2 children (inorder successor)
inorder() → sorted output
🔹 Key Logic
Delete all nodes < 30
Perform reverse inorder → descending order

👉 Concept: BST property + traversal
👉 Traversal Trick:

inorder → ascending
reverse inorder → descending
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None

    def insert(self, data):
        self.root = self._insert(self.root, data)

    def _insert(self, node, data):
        if not node:
            return Node(data)
        if data < node.data:
            node.left = self._insert(node.left, data)
        else:
            node.right = self._insert(node.right, data)
        return node

    # delete
    def delete(self, node, key):
        if not node:
            return node
        if key < node.data:
            node.left = self.delete(node.left, key)
        elif key > node.data:
            node.right = self.delete(node.right, key)
        else:
            if not node.left:
                return node.right
            elif not node.right:
                return node.left
            temp = self.min_value(node.right)
            node.data = temp.data
            node.right = self.delete(node.right, temp.data)
        return node

    def min_value(self, node):
        while node.left:
            node = node.left
        return node

    # inorder
    def inorder(self, node):
        if node:
            self.inorder(node.left)
            print(node.data, end=" ")
            self.inorder(node.right)

    # reverse inorder (descending)
    def descending(self, node):
        if node:
            self.descending(node.right)
            print(node.data, end=" ")
            self.descending(node.left)

# Driver
values = [50,28,75,16,35,62,90,12,33,40,58,68]
bst = BST()

for v in values:
    bst.insert(v)

# delete nodes < 30
for v in values:
    if v < 30:
        bst.root = bst.delete(bst.root, v)

print("Output:")
bst.descending(bst.root)
