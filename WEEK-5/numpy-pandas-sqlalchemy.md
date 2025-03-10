
* * *

**NumPy, Pandas, and SQLAlchemy: A Comprehensive Guide**
========================================================

**Overview**
------------

This document covers the fundamentals of using **NumPy**, **Pandas**, and **SQLAlchemy** in Python. It includes:

*   **NumPy** for numerical computations.
*   **Pandas** for data analysis and manipulation.
*   **SQLAlchemy** for database operations.

* * *

**1\. NumPy Operations**
========================

**1.1 Introduction to NumPy**
-----------------------------

[NumPy](https://numpy.org/) is a Python library used for numerical computing. It provides support for large, multi-dimensional arrays and matrices, along with mathematical functions.

### **Installing NumPy**

Before using NumPy, ensure it's installed:

`pip install numpy` 

### **Importing NumPy**

`import numpy as np` 

* * *

**1.2 Creating Arrays**
-----------------------

NumPy arrays are similar to Python lists but more efficient.

### **Creating a 1D Array**

`arr = np.array([1, 2, 3, 4, 5])
print(arr)` 

### **Creating a 2D Array**

`arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d)` 

*   `np.array([values])` creates a **one-dimensional** NumPy array.
*   `np.array([[values]])` creates a **two-dimensional** NumPy array.

* * *

**1.3 Special Arrays**
----------------------

NumPy provides functions to create arrays with specific values.

`arr_random = np.random.rand(3, 4)  # Random values between 0 and 1
zeros_array = np.zeros((3, 4))  # 3x4 matrix filled with zeros
ones_array = np.ones((3, 4))  # 3x4 matrix filled with ones
full_array = np.full((3, 4), 10)  # 3x4 matrix filled with 10` 

*   `np.random.rand(rows, cols)`: Creates a **random** array.
*   `np.zeros((rows, cols))`: Creates an array filled with **zeros**.
*   `np.ones((rows, cols))`: Creates an array filled with **ones**.
*   `np.full((rows, cols), value)`: Creates an array filled with a **specified value**.

* * *

**1.4 Generating Sequences**
----------------------------

NumPy provides methods to generate sequences of numbers.

`arr_range = np.arange(10, 20, 0.5)
print(arr_range)` 

*   `np.arange(start, stop, step)`: Generates a sequence from **start** to **stop**, with a specified **step size**.

* * *

**1.5 Matrix Operations**
-------------------------

NumPy supports mathematical operations on arrays and matrices.

`matrix_1 = np.array([[1, 2, 3], [4, 5, 6]])
matrix_2 = np.array([[1, 2, 3], [4, 5, 6]])

matrix_addition = matrix_1 + matrix_2
matrix_subtraction = matrix_1 - matrix_2
matrix_multiplication = matrix_1 * matrix_2
matrix_division = matrix_1 / matrix_2
matrix_power = matrix_1 ** matrix_2
matrix_transpose = matrix_1.T` 

*   **Element-wise** operations: Addition, subtraction, multiplication, division.
*   `matrix_1.T`: **Transpose** of a matrix.

* * *

**1.6 Statistical Functions**
-----------------------------

`arr_mean = np.mean(arr)  # Mean (average)
arr_median = np.median(arr)  # Median
arr_std = np.std(arr)  # Standard deviation
arr_var = np.var(arr)  # Variance` 

*   `np.mean(arr)`: Computes the **mean**.
*   `np.median(arr)`: Computes the **median**.
*   `np.std(arr)`: Computes the **standard deviation**.
*   `np.var(arr)`: Computes the **variance**.

* * *

**2\. Pandas Operations**
=========================

**2.1 Introduction to Pandas**
------------------------------

Pandas is a Python library used for data manipulation and analysis.

### **Installing Pandas**

`pip install pandas` 

### **Importing Pandas**

`import pandas as pd` 

* * *

**2.2 Creating a Pandas Series**
--------------------------------

A **Series** is a one-dimensional labeled array.

`series = pd.Series([1, 2, 3, 4, 5])
series_2 = pd.Series([1, 2, 3, 4, 5], index=['a', 'b', 'c', 'd', 'e'])` 

*   **Default index**: `pd.Series([values])`
*   **Custom index**: `pd.Series([values], index=[labels])`

* * *

**2.3 Creating a DataFrame**
----------------------------

A **DataFrame** is a 2D table-like data structure.

`data = {
    'Name': ['John', 'Jane', 'Jim', 'Jill'],
    'Age': [20, 21, 22, 23],
    'City': ['New York', 'Los Angeles', 'Chicago', 'Houston']
}
df = pd.DataFrame(data)
print(df)` 

* * *

**2.4 Reading a CSV File**
--------------------------

`df_csv = pd.read_csv('data.csv')
print(df_csv)` 

*   `pd.read_csv(filename)`: Reads a CSV file into a **DataFrame**.

* * *

**3\. SQLAlchemy Operations**
=============================

**3.1 Introduction to SQLAlchemy**
----------------------------------

[SQLAlchemy](https://www.sqlalchemy.org/) is a Python library for interacting with databases.

### **Installing SQLAlchemy**

`pip install sqlalchemy` 

### **Importing SQLAlchemy**

`from sqlalchemy import create_engine, text, MetaData, Table, Column, Integer, String, ForeignKey, insert, select` 

* * *

**3.2 Creating a SQLite Database**
----------------------------------

`import os
print(os.getcwd())  # Get current working directory

engine = create_engine('sqlite:///basic_module/src/Datastructures/data.db', echo=True)` 

*   `os.getcwd()`: Retrieves the current directory.
*   `create_engine('sqlite:///path')`: Connects SQLite to the project.

* * *

**3.3 Database Permission Issues**
----------------------------------

If permission issues arise, run:

`chmod 755 /basic_module/src/Datastructures/data.db` 

* * *

**3.4 Executing SQLite Commands**
---------------------------------

To open the database in the terminal:

`sqlite3 /basic_module/src/Datastructures/data.db` 

List tables:

`.tables` 

* * *

**3.5 Defining a Database Schema**
----------------------------------

`metadata = MetaData()

users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String),
    Column('age', Integer),
    Column('city', String),
)

posts = Table('posts', metadata,
    Column('id', Integer, primary_key=True),
    Column('title', String),
    Column('content', String),
    Column('user_id', Integer, ForeignKey('users.id')),
)

metadata.create_all(engine)` 

*   **`MetaData()`**: Stores table definitions.
*   **`Table('name', metadata, columns)`**: Defines a table with columns.
*   **`metadata.create_all(engine)`**: Creates the tables in the database.

* * *

**Conclusion**
--------------

*   NumPy: **Efficient numerical computations**.
*   Pandas: **Powerful data analysis tools**.
*   SQLAlchemy: **Seamless database integration**.
