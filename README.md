```python
# =========================
# 🔥 EXP 1: RECURSION VS ITERATION
# =========================
import time

def factorial_recursive(n):
    if n==0 or n==1:
        return 1
    return n*factorial_recursive(n-1)

def factorial_iterative(n):
    res=1
    for i in range(2,n+1):
        res*=i
    return res

def fib_recursive(n):
    if n==0: return 0
    if n==1: return 1
    return fib_recursive(n-1)+fib_recursive(n-2)

def fib_iterative(n):
    a,b=0,1
    for _ in range(n):
        print(a,end=" ")
        a,b=b,a+b
    print()

# =========================
# 🔥 EXP 2: ARRAYS
# =========================
class StaticArray:
    def __init__(self, cap):
        self.cap=cap
        self.size=0
        self.arr=[None]*cap

    def append(self,val):
        if self.size==self.cap:
            raise Exception("Full")
        self.arr[self.size]=val
        self.size+=1

class DynamicArray:
    def __init__(self):
        self.cap=2
        self.size=0
        self.arr=[None]*self.cap

    def resize(self,new_cap):
        new=[None]*new_cap
        for i in range(self.size):
            new[i]=self.arr[i]
        self.arr=new
        self.cap=new_cap

    def append(self,val):
        if self.size==self.cap:
            self.resize(self.cap*2)
        self.arr[self.size]=val
        self.size+=1

# =========================
# 🔥 EXP 3: STACK & QUEUE
# =========================
class Stack:
    def __init__(self,n):
        self.arr=[None]*n
        self.top=-1

    def push(self,x):
        self.top+=1
        self.arr[self.top]=x

    def pop(self):
        x=self.arr[self.top]
        self.top-=1
        return x

class Queue:
    def __init__(self,n):
        self.arr=[None]*n
        self.front=self.rear=-1
        self.n=n

    def enqueue(self,x):
        if self.front==-1:
            self.front=0
        self.rear=(self.rear+1)%self.n
        self.arr[self.rear]=x

    def dequeue(self):
        x=self.arr[self.front]
        if self.front==self.rear:
            self.front=self.rear=-1
        else:
            self.front=(self.front+1)%self.n
        return x

# Infix to Postfix
def precedence(op):
    if op in '+-': return 1
    if op in '*/': return 2
    return 0

def infix_to_postfix(expr):
    stack=[]
    res=""
    for ch in expr:
        if ch.isalnum():
            res+=ch
        elif ch=='(':
            stack.append(ch)
        elif ch==')':
            while stack[-1]!='(':
                res+=stack.pop()
            stack.pop()
        else:
            while stack and precedence(ch)<=precedence(stack[-1]):
                res+=stack.pop()
            stack.append(ch)
    while stack:
        res+=stack.pop()
    return res

# =========================
# 🔥 EXP 4: LINKED LIST
# =========================
class Node:
    def __init__(self,data):
        self.data=data
        self.next=None

class SLL:
    def __init__(self):
        self.head=None

    def insert_end(self,data):
        new=Node(data)
        if not self.head:
            self.head=new
            return
        temp=self.head
        while temp.next:
            temp=temp.next
        temp.next=new

# =========================
# 🔥 EXP 5: BST
# =========================
class TreeNode:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

class BST:
    def __init__(self):
        self.root=None

    def insert(self,data):
        self.root=self._insert(self.root,data)

    def _insert(self,node,data):
        if not node:
            return TreeNode(data)
        if data<node.data:
            node.left=self._insert(node.left,data)
        else:
            node.right=self._insert(node.right,data)
        return node

    def inorder(self,node):
        if node:
            self.inorder(node.left)
            print(node.data,end=" ")
            self.inorder(node.right)

# =========================
# 🔥 EXP 6: HEAP
# =========================
class MinHeap:
    def __init__(self):
        self.heap=[]

    def insert(self,val):
        self.heap.append(val)
        i=len(self.heap)-1
        while i>0 and self.heap[(i-1)//2]>self.heap[i]:
            self.heap[i],self.heap[(i-1)//2]=self.heap[(i-1)//2],self.heap[i]
            i=(i-1)//2

    def extract_min(self):
        if not self.heap:
            return None
        root=self.heap[0]
        self.heap[0]=self.heap[-1]
        self.heap.pop()
        self.heapify(0)
        return root

    def heapify(self,i):
        l=2*i+1
        r=2*i+2
        smallest=i
        if l<len(self.heap) and self.heap[l]<self.heap[smallest]:
            smallest=l
        if r<len(self.heap) and self.heap[r]<self.heap[smallest]:
            smallest=r
        if smallest!=i:
            self.heap[i],self.heap[smallest]=self.heap[smallest],self.heap[i]
            self.heapify(smallest)

# =========================
# 🔥 EXP 7: AVL TREE
# =========================
class AVLNode:
    def __init__(self,key):
        self.key=key
        self.left=None
        self.right=None
        self.height=1

def height(n):
    return n.height if n else 0

def rotate_right(y):
    x=y.left
    t=x.right
    x.right=y
    y.left=t
    y.height=1+max(height(y.left),height(y.right))
    x.height=1+max(height(x.left),height(x.right))
    return x

def rotate_left(x):
    y=x.right
    t=y.left
    y.left=x
    x.right=t
    x.height=1+max(height(x.left),height(x.right))
    y.height=1+max(height(y.left),height(y.right))
    return y

def insert_avl(root,key):
    if not root:
        return AVLNode(key)
    if key<root.key:
        root.left=insert_avl(root.left,key)
    else:
        root.right=insert_avl(root.right,key)

    root.height=1+max(height(root.left),height(root.right))
    balance=height(root.left)-height(root.right)

    if balance>1 and key<root.left.key:
        return rotate_right(root)
    if balance<-1 and key>root.right.key:
        return rotate_left(root)
    if balance>1 and key>root.left.key:
        root.left=rotate_left(root.left)
        return rotate_right(root)
    if balance<-1 and key<root.right.key:
        root.right=rotate_right(root.right)
        return rotate_left(root)
    return root

# =========================
# 🔥 EXP 8: SORTING
# =========================
def bubble_sort(a):
    n=len(a)
    for i in range(n):
        for j in range(0,n-i-1):
            if a[j]>a[j+1]:
                a[j],a[j+1]=a[j+1],a[j]

def insertion_sort(a):
    for i in range(1,len(a)):
        key=a[i]
        j=i-1
        while j>=0 and key<a[j]:
            a[j+1]=a[j]
            j-=1
        a[j+1]=key

def selection_sort(a):
    for i in range(len(a)):
        min_idx=i
        for j in range(i+1,len(a)):
            if a[j]<a[min_idx]:
                min_idx=j
        a[i],a[min_idx]=a[min_idx],a[i]

def merge_sort(a):
    if len(a)>1:
        mid=len(a)//2
        L=a[:mid]
        R=a[mid:]
        merge_sort(L)
        merge_sort(R)
        i=j=k=0
        while i<len(L) and j<len(R):
            if L[i]<R[j]:
                a[k]=L[i]; i+=1
            else:
                a[k]=R[j]; j+=1
            k+=1
        while i<len(L):
            a[k]=L[i]; i+=1; k+=1
        while j<len(R):
            a[k]=R[j]; j+=1; k+=1

# =========================
# 🔥 SAMPLE RUN
# =========================
print("Factorial:", factorial_recursive(5))
print("Fibo iterative:", end=" "); fib_iterative(10)

arr=[5,2,9,1]
merge_sort(arr)
print("Sorted:",arr)
```
