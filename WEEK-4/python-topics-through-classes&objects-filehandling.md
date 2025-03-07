
* * *



# **Python OOP Concepts - Constructors, Destructors, Shallow Copy & Deep Copy**

## **1. Constructors (`__init__` Method)**
A **constructor** is a special method in Python that is **automatically called** when an object is created. It initializes the object's attributes.

### **Example: Constructor in Python**
```python
class Animal:
    def __init__(self, name):  # Correct constructor format
        self.name = name  # Initializing instance variable

    def speak(self):
        return f"Animal {self.name} is speaking"

# Creating an object of the Animal class
animal = Animal("Lion")
print(animal.speak())  # Output: Animal Lion is speaking`` 
```
### **🔴 Common Errors in Constructors**

1.  **Wrong method name (`_init_` instead of `__init__`)**
    
    `def _init_(self, name):  # ❌ Wrong: It should be __init__` 
    
    ✅ **Fix:**
    
    `def __init__(self, name):  # ✅ Correct` 
    
2.  **Forgetting `self` parameter**
    
    ``def __init__(name):  # ❌ Wrong: `self` is missing`` 
    
    ✅ **Fix:**
    
    `def __init__(self, name):  # ✅ Correct` 
    

* * *

**2\. Destructors (`__del__` Method)**
--------------------------------------

A **destructor** is a special method in Python that **is called automatically** when an object is deleted or goes out of scope.

### **Example: Destructor in Python**

`class Person:
    def __init__(self, name):
        self.name = name
        print(f"Person {self.name} is created.")

    def __del__(self):
        print(f"Person {self.name} is destroyed.")

# Creating an object
p1 = Person("Alice")

# Explicitly deleting the object
del p1  # Calls the destructor immediately` 

### **🔴 Common Errors in Destructors**

1.  **Using `_del_` instead of `__del__`**
    
    `def _del_(self):  # ❌ Wrong` 
    
    ✅ **Fix:**
    
    `def __del__(self):  # ✅ Correct` 
    

* * *

**3\. Shallow Copy vs Deep Copy**
---------------------------------

Python provides the `copy` module for object copying. There are two types:

1.  **Shallow Copy** (`copy.copy()`) → Creates a new object but **does not create copies of nested objects** (only references them).
2.  **Deep Copy** (`copy.deepcopy()`) → Creates a new object and **recursively copies all nested objects**.

* * *

**3.1 Shallow Copy (`copy.copy()`)**
------------------------------------

A **shallow copy** creates a new object but **references the same nested objects**.

### **Example:**

`import copy

class Address:
    def __init__(self, city, country):
        self.city = city
        self.country = country

class Person:
    def __init__(self, name, address):
        self.name = name
        self.address = address

# Creating an object
original_person = Person("John", Address("New York", "USA"))

# Creating a shallow copy
shallow_copy_person = copy.copy(original_person)

# Modifying the original object
original_person.address.city = "Boston"

# Checking the effect on copies
print(original_person.address.city)  # Output: Boston
print(shallow_copy_person.address.city)  # Output: Boston (Same reference)` 

### **🔴 Issue with Shallow Copy**

*   The copied object still **shares** the reference to the nested `Address` object.
*   Changing `original_person.address.city` **also changes** `shallow_copy_person.address.city`.

* * *

**3.2 Deep Copy (`copy.deepcopy()`)**
-------------------------------------

A **deep copy** creates an entirely new object, including copies of all nested objects.

### **Example:**

`import copy

class Address:
    def __init__(self, city, country):
        self.city = city
        self.country = country

class Person:
    def __init__(self, name, address):
        self.name = name
        self.address = address

# Creating an object
original_person = Person("John", Address("New York", "USA"))

# Creating a deep copy
deep_copy_person = copy.deepcopy(original_person)

# Modifying the original object
original_person.address.city = "Boston"

# Checking the effect on copies
print(original_person.address.city)  # Output: Boston
print(deep_copy_person.address.city)  # Output: New York (Completely independent copy)` 

### **✅ Deep Copy Solves the Issue**

*   `deep_copy_person.address.city` **remains "New York"** because it is an independent copy.
*   **No shared references** between the original and copied object.

* * *

**4\. Common Errors in Copying**
--------------------------------

### **🔴 Using `=` Instead of `copy.copy()`**

`p1 = Person("Alice", Address("NY", "USA"))
p2 = p1  # ❌ Wrong: This only copies the reference, not the object` 

✅ **Fix: Use `copy.copy()`**

`p2 = copy.copy(p1)  # ✅ Correct for shallow copy` 

### **🔴 Using `copy.copy()` Instead of `copy.deepcopy()`**

`p2 = copy.copy(p1)  # ❌ Wrong if you want an independent copy` 

✅ **Fix: Use `copy.deepcopy()`**

`p2 = copy.deepcopy(p1)  # ✅ Correct for deep copy` 

* * *

**5\. Fixing Errors in Your Original Code**
-------------------------------------------

### **Errors Identified**

1.  **Incorrect `__init__` Method (`_init_` instead of `__init__`)**
    
    `def _init_(self, name):  # ❌ Wrong` 
    
    ✅ **Fix:**
    
    `def __init__(self, name):  # ✅ Correct` 
    
2.  **Incorrect `__str__` Method (`_str_` instead of `__str__`)**
    
    `def _str_(self):  # ❌ Wrong` 
    
    ✅ **Fix:**
    
    `def __str__(self):  # ✅ Correct` 
    
3.  **Method Overriding Issue in `speak()` (Overwritten by second definition)**
    
    ``def speak(self, age=0):
    def speak(self, name, age, breed):  # ❌ Overwrites previous `speak()` `` 
    
    ✅ **Fix: Use `*args` for method overloading**
    
    `def speak(self, *args):
        return f"Dog {self.name} is barking with details: {args}"` 
    
4.  **Incorrect `if __name__ == "__main__":`**
    
    `if _name_ == "_main_":  # ❌ Wrong` 
    
    ✅ **Fix:**
    
    `if __name__ == "__main__":  # ✅ Correct` 
    

* * *

**Summary**
-----------

✔ **Constructors** initialize objects automatically.  
✔ **Destructors** are called when an object is deleted.  
✔ **Shallow copy** copies references, while **deep copy** creates independent copies.  
✔ Always use `__init__`, `__del__`, and `__str__` correctly.  
✔ Prefer `copy.deepcopy()` for **completely independent objects**.

* * *

* * *


``# **Python Exception Handling (`try-except-finally`)**

## **1. Introduction to Exception Handling**
Exception handling in Python is done using `try-except-finally`.  
- `try`: Contains the code that may raise an exception.
- `except`: Handles the exception if it occurs.
- `finally`: Executes **whether an exception occurs or not** (used for cleanup actions).

---

## **2. Basic `try-except-finally` Example**

try:
    x = 10 / 0  # Division by zero error
except ZeroDivisionError as e:
    print("Error:", e)
finally:
    print("This always executes")`` 

### **🔹 Output**

vbnet

`Error: division by zero
This always executes` 

* * *

**3\. Handling Multiple Exceptions**
------------------------------------

`try:
    num = int(input("Enter a number: "))
    result = 10 / num
except ZeroDivisionError:
    print("You cannot divide by zero!")
except ValueError:
    print("Invalid input! Please enter a number.")
finally:
    print("Execution completed.")` 

### **🔹 Output**

*   If `num = 0` → `"You cannot divide by zero!"`
*   If `num = "abc"` → `"Invalid input! Please enter a number."`
*   The `finally` block runs **every time**.

* * *

**4\. Custom Exception Handling**
---------------------------------

You can **define your own exception class** by inheriting from `Exception`.

### **🔹 Example: Custom Exception**

``class CustomException(Exception):
    def __init__(self, message):  # ✅ Fix: Corrected `__init__`
        self.message = message
        super().__init__(self.message)  # ✅ Fix: Corrected super() call

try:
    raise CustomException("This is a custom exception")  # Raising the exception
except CustomException as e:
    print("Caught Custom Exception:", e)
except Exception as e:
    print("Caught General Exception:", e)
finally:
    print("Finally block executed.")`` 

### **🔹 Output**

vbnet

`Caught Custom Exception: This is a custom exception
Finally block executed.` 

* * *

**5\. Common Python Exceptions**
--------------------------------

Exception Name

Description

Example

`ZeroDivisionError`

Division by zero

`10 / 0`

`ValueError`

Invalid type conversion

`int("abc")`

`TypeError`

Invalid operation on different types

`"hello" + 5`

`IndexError`

Accessing invalid list index

`my_list[10]`

`KeyError`

Accessing invalid dictionary key

`my_dict["missing"]`

`AttributeError`

Calling a non-existent attribute

`"hello".unknown_method()`

`FileNotFoundError`

File does not exist

`open("missing.txt")`

### **🔹 Example: Handling Multiple Exceptions**

`try:
    my_list = [1, 2, 3]
    print(my_list[10])  # IndexError
except IndexError:
    print("Index out of range!")
except Exception as e:
    print("Some other error occurred:", e)` 

* * *

**6\. Using `else` with `try-except`**
--------------------------------------

The `else` block executes **only if no exception occurs**.

### **🔹 Example**

`try:
    num = int(input("Enter a number: "))
    result = 10 / num
except ZeroDivisionError:
    print("Cannot divide by zero!")
else:
    print("Success! The result is:", result)  # Runs if no error
finally:
    print("This always runs.")` 

* * *

**7\. Summary**
---------------

✔ **Use `try` to wrap risky code.**  
✔ **Use `except` to handle exceptions.**  
✔ **Use `finally` for cleanup operations.**  
✔ **Define custom exceptions for specific needs.**  
✔ **Use `else` for successful execution scenarios.**

* * *

### 🚀 **Now you are ready to handle Python exceptions efficiently!** ✅

yaml

 ``--- 
### **Fixes in Your Original Code**
1. **Incorrect `__init__` method name (`_init_` instead of `__init__`)**
    ```python
    def _init_(self, message):  # ❌ Wrong
    ```
    ✅ **Fix:**
    ```python
    def __init__(self, message):  # ✅ Correct
    ```

2. **Incorrect `super()` Call (`_init_` instead of `__init__`)**
    ```python
    super()._init_(self.message)  # ❌ Wrong
    ```
    ✅ **Fix:**

    super().__init__(self.message)  # ✅ Correct
    ```

--- 






 
