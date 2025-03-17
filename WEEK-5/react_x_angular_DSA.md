### **Virtual DOM vs Real DOM**

#### **1. Real DOM (Document Object Model)**
- The actual representation of the webpage in the browser.
- Updates are slow because the entire DOM needs to be re-rendered when changes occur.
- Directly interacts with HTML, CSS, and JavaScript.
- Causes reflow and repaint, which affects performance.
- Every change leads to manipulation of the UI, making it inefficient for frequent updates.

#### **2. Virtual DOM (VDOM)**
- A lightweight copy of the Real DOM, maintained in memory.
- Used by frameworks like React to improve performance.
- Updates are fast because changes are first made in the Virtual DOM and then efficiently synced with the Real DOM.
- Uses a **diffing algorithm** to detect changes and updates only the necessary parts in the Real DOM.
- Reduces unnecessary re-renders, improving speed and efficiency.

#### **Key Differences:**

| Feature          | Real DOM | Virtual DOM |
|-----------------|----------|-------------|
| **Definition**  | Actual representation of the webpage | In-memory lightweight copy of the DOM |
| **Update Speed** | Slow (re-renders entire DOM) | Fast (updates only changed parts) |
| **Performance** | Less efficient | More efficient |
| **Rendering** | Directly updates the UI | Uses diffing & batch updates |
| **Use Case** | Traditional websites | React & other modern libraries |

### **Stack Implementation in Python**

This code defines a `Stack` class that implements a stack data structure with various special methods to enhance its functionality. Here’s a breakdown of its components:

---
## Difference between Angular and React

### ANGULAR
- Angular is a TypeScript framework developed and maintained by Google.
- It follows the Model-View-Controller and Component-based architecture.

### **Architecture of Angular**
1. **Modules** – Organizes code into logical units.
2. **Components** – UI elements that manage the view.
3. **Services** – Handles business logic and data fetching.
4. **Templates** – HTML files that define the structure of the view.
5. **Directives** – Add dynamic behavior to templates.
6. **Dependency Injection** – Injects services and objects into components.
7. **Router** – Handles navigation between components.

---

### REACT
- React is a JavaScript library developed and maintained by Facebook.
- React mainly focuses on building UI components.
- It uses Component-based architecture and Virtual DOM to improve performance.

### **Architecture of React**
1. **Components** – UI elements that define the view.
2. **Props** – Used to pass data from parent to child components.
3. **State** – Holds data specific to a component.
4. **Virtual DOM** – React creates a virtual copy of the DOM and updates only the differences.
5. **Hooks** – Handle state, lifecycle, and context.
6. **Router** – Handles navigation between components (via React Router).

---

### **Comparison**
- Angular is structured and a solution for building complex apps.
- React is flexible and fast and is mainly used for dynamic user interfaces.
## RECONCILIATION
Reconciliation is the process of updating the user interface (UI) when the state of a web application changes.

### **Reconciliation in React**
- React uses the Virtual DOM to perform reconciliation.

**Steps:**
1. When the state changes → React creates a new Virtual DOM.
2. React compares the new Virtual DOM with the previous one (this is called diffing).
3. React finds out what’s different between the two versions.
4. It updates only the changed parts of the Real DOM (instead of rebuilding the whole tree).

---

### **Reconciliation in Angular**
- Angular does not use a Virtual DOM – it works directly with the Real DOM.
- Angular uses a technique called **Change Detection** to perform reconciliation.

**When state changes:**
1. Angular checks each component to see if the data has changed.
2. If data has changed → Angular updates the component.
3. Angular uses a process called **Zone.js** to track changes and trigger updates.
![Linux Commands](../Images/Screenshots/Screenshot%202025-03-13%20170532.png)

---
---
### **Class: `Stack`**

#### **1. Initialization (`__init__`)**
- Initializes an empty list `items` to store stack elements.

#### **2. Basic Stack Operations**
- `push(item)`: Adds an item to the stack.
- `pop()`: Removes and returns the top item.
- `peek()`: Returns the top item without removing it.
- `size()`: Returns the number of elements in the stack.
- `is_empty()`: Checks if the stack is empty.

#### **3. Overloaded Magic Methods**
- **String Representation**:
  - `__str__()`: Returns a string representation of the stack.
  - `__repr__()`: Returns an official string representation.
- **Length & Membership**:
  - `__len__()`: Returns the number of elements in the stack.
  - `__contains__(item)`: Checks if an item exists in the stack.
- **Indexing Support**:
  - `__getitem__(index)`: Allows access to elements like a list.
  - `__setitem__(index, value)`: Modifies elements using index.
  - `__delitem__(index)`: Deletes an element by index.
- **Iteration & Reversal**:
  - `__iter__()`: Enables iteration over stack elements.
  - `__reversed__()`: Allows reversed iteration.
- **Comparison Operators**:
  - `__eq__()`, `__ne__()`, `__gt__()`, `__ge__()`, `__lt__()`, `__le__()`: Enable stack comparisons.
- **Arithmetic Operators**:
  - `__add__()`, `__iadd__()`: Allow stack concatenation.
  - `__mul__()`, `__imul__()`: Support stack repetition.
  - `__truediv__()`, `__mod__()`: Raise errors since division/modulo are not valid for stacks.
- **Mathematical Functions**:
  - `__neg__()`: Returns a list of negated values.
  - `__abs__()`: Returns absolute values.
  - `__round__(n)`, `__ceil__()`, `__floor__()`: Perform mathematical rounding operations.

---

### **Usage Example**
```python
if __name__ == "__main__":
    stack = Stack()
    stack.push(1)
    stack.push(2)
    stack.push(3)
    
    print("Stack after pushes:", stack)
    
    print("Popped item:", stack.pop())
    print("Stack after pop:", stack)
    
    print("Peek item:", stack.peek())
    print("Stack size:", stack.size())
    print("Is stack empty?", stack.is_empty())
    
    # Testing slicing and list operations
    print("Stack items (as list):", stack.items)
    print("First item:", stack.items[0])
    print("Last item:", stack.items[-1])
    print("Stack items (sliced):", stack.items[1:])  # Example slice
    print("Reversed stack:", list(reversed(stack)))
    print("Ceiled values:", stack.__ceil__())
    print("Rounded values:", stack.__round__(1))  # Round to 1 decimal
```

This script demonstrates how the `Stack` class operates, including pushing, popping, peeking, and using special operations like slicing, reversing, and mathematical functions.
<img width="485" alt="image" src="https://github.com/user-attachments/assets/42a28554-092e-4813-a331-abcf148e49b2" />

### **Problem Statement**
Write a program to transfer elements from one stack to another without using a third stack.

---

### **Implementation**
```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop() if self.items else None

    def is_empty(self):
        return not self.items

    def __str__(self):
        return str(self.items)

def transfer_stack(source, destination):
    if source.is_empty():
        return
    destination.push(source.pop())
    transfer_stack(source, destination)

# Example Usage
source = Stack()
destination = Stack()
for i in range(1, 5):
    source.push(i)

print("Before Transfer:", source, destination)
transfer_stack(source, destination)
print("After Transfer:", source, destination)
```

---

### **Explanation**
#### **1. Stack Class**
- `__init__()`: Initializes an empty stack.
- `push(item)`: Adds an item to the stack.
- `pop()`: Removes and returns the top item (returns `None` if empty).
- `is_empty()`: Checks if the stack is empty.
- `__str__()`: Returns a string representation of the stack.

#### **2. Transfer Function (`transfer_stack`)**
- Uses recursion to move elements from `source` to `destination`.
- Base case: If `source` is empty, return.
- Pops the top element from `source`, pushes it onto `destination`, then calls itself recursively.

#### **3. Example Usage**
- Creates two stacks: `source` and `destination`.
- Pushes numbers `1, 2, 3, 4` onto `source`.
- Prints stacks before and after transfer.

This method efficiently transfers elements without using an additional stack by leveraging recursion.

<img width="479" alt="image" src="https://github.com/user-attachments/assets/b52d4aa6-b213-4bda-b3ff-0439960d95e7" />

## Tree Data Structure and Implementation

## **Concepts of a Tree Data Structure**
A **tree** is a hierarchical data structure that consists of nodes connected by edges. It follows a parent-child relationship, where:

- **Root**: The topmost node of the tree.
- **Node**: Each element in the tree, containing a value and references to child nodes.
- **Branches**: The connections between parent and child nodes.
- **Leaf Node**: A node with no children.

Trees are used in various applications like file systems, databases, and hierarchical structures.

## **Tree Traversal & Search Algorithms**
### **Traversal Methods**
1. **Depth-First Search (DFS):**
   - **Preorder (Root -> Left -> Right)**
   - **Inorder (Left -> Root -> Right)** (For Binary Trees)
   - **Postorder (Left -> Right -> Root)**
2. **Breadth-First Search (BFS):**
   - Level-order traversal (Visits nodes level by level).

### **Search Algorithms**
- **DFS Search**: Uses stack-based recursion or explicit stack.
- **BFS Search**: Uses a queue to explore nodes level by level.

## **TreeNode Class Implementation**
Below is an implementation of a generic tree structure in Python:

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.children = []

    def add_child(self, child):
        self.children.append(child)

    def remove_child(self, child):
        self.children.remove(child)

    def __str__(self):
        return self.value   
    
    def delete(self, value):
        if value in self.children:
            self.children.remove(value)
        else:
            for child in self.children:
                child.delete(value)

    def inorder_traversal(self):
        if self.children:
            for child in self.children:
                child.inorder_traversal()
        else:
            print(self.value)

if __name__ == "__main__":
    tree = TreeNode("A")
    tree.add_child("B")
    tree.add_child("C")
    tree.add_child("D")
    tree.add_child("E")
    print(tree)
    print(tree.children)
```

### **Explanation of the Code**
1. **`TreeNode` Class:**
   - Stores `value` and `children`.
   - Provides methods for adding/removing children and traversing the tree.
2. **Traversal (`inorder_traversal`)**:
   - Iterates through children recursively (not ideal for general trees, better for binary trees).
3. **Execution (`__main__`)**:
   - Creates a tree with root `A` and adds children `B, C, D, E`.
   - Prints root and its children.

---
This implementation lays the foundation for more complex tree structures like **binary search trees (BSTs)**.

<img width="584" alt="image" src="https://github.com/user-attachments/assets/d9757f81-298c-4ba8-af08-54d781f43245" />
### **Flow of Execution**
1. `TreeNode` class performs all tree operations.
2. `__init__` → Initializes a node with a value and child nodes.
3. `add_child` → Adds a child node.
4. `remove_child` → Removes a child node.
5. `__str__` → Returns the value of the node.
6. `__repr__` → Returns a string representation.
7. `__len__` → Returns the number of children.
8. `__contains__` → Checks if a node contains a particular value.
9. `__getitem__` → Allows access to child nodes using indexing.
10. `__setitem__` → Sets child node values.
11. `__delitem__` → Deletes a child node at a particular index.
12. `__iter__` → Iterates over children.
13. `__reversed__` → Reverses iteration over children.
14. Comparison methods → Compare nodes.
