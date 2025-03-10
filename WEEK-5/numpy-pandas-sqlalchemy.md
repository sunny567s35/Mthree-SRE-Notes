
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
*   

* * *

**Fixing Python Linting and SQLite Version Resolution Issues**
==============================================================

**1\. Linting Not Working for Python**
--------------------------------------

If Python linting (e.g., **pylint**, **flake8**, or **mypy**) is not working, follow these steps to resolve it.

### **1.1 Check If the Linter Is Installed**

Ensure that the required linter is installed in your current environment.

`pip list | grep pylint  # Check if pylint is installed
pip list | grep flake8  # Check if flake8 is installed` 

If not installed, install it using:

`pip install pylint flake8` 

### **1.2 Check If the Linter Works Manually**

Try running the linter manually on a Python file:

`pylint your_script.py
flake8 your_script.py` 

If it works manually but not in your editor, the issue might be with your editor settings.

### **1.3 Configure Linter in VS Code (If Using VS Code)**

If you are using **VS Code**, ensure the Python linter is enabled.

#### **Enable Linting**

1.  Open **VS Code**.
2.  Press `Ctrl + Shift + P` and type `"Python: Select Linter"`.
3.  Choose the linter you want (`pylint`, `flake8`, or `mypy`).
4.  Alternatively, add the following to **settings.json**:
    
    `{
        "python.linting.enabled": true,
        "python.linting.pylintEnabled": true,
        "python.linting.flake8Enabled": true
    }` 
    

#### **Ensure VS Code Uses the Correct Python Interpreter**

1.  Press `Ctrl + Shift + P` and type `"Python: Select Interpreter"`.
2.  Select the correct Python interpreter (virtual environment if needed).

### **1.4 Check for Linter Conflicts**

If multiple linters are installed, ensure they are not conflicting. Try disabling one linter and testing again.

Example: If using `flake8`, disable `pylint`:

`{
    "python.linting.pylintEnabled": false,
    "python.linting.flake8Enabled": true
}` 

* * *

**2\. SQLite Version Resolution Issue**
---------------------------------------

When **two versions of SQLite** are installed—one **globally** and one in a **virtual environment**—but you need to access the **global SQLite version**, follow these steps.

### **2.1 Identify SQLite Versions**

First, check the installed SQLite versions:

#### **Check Global SQLite Version**

`sqlite3 --version` 

#### **Check SQLite Version in Virtual Environment**

Activate your virtual environment and check:

`source venv/bin/activate  # macOS/Linux
venv\Scripts\activate  # Windows

python -c "import sqlite3; print(sqlite3.sqlite_version)"` 

* * *

### **2.2 Force Python to Use the Global SQLite Version**

By default, Python will use the SQLite version installed within its environment. To force it to use the **global SQLite**, you need to set the **LD\_LIBRARY\_PATH** (Linux/macOS) or modify the **DLL path** (Windows).

#### **On Linux/macOS:**

Set the **LD\_LIBRARY\_PATH** to the global SQLite version:

`export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH` 

To make this change permanent, add the line to your `~/.bashrc` or `~/.zshrc`:

`echo 'export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc  # Apply changes` 

#### **On Windows:**

Modify the system **PATH** environment variable:

1.  Open **Command Prompt** as Administrator.
2.  Find the path of the global SQLite DLL:
    
    `where sqlite3` 
    
3.  Add this path to **Environment Variables**:
    *   Go to **Control Panel** > **System** > **Advanced System Settings** > **Environment Variables**.
    *   Find `Path` under **System Variables** and add the SQLite global directory.

Alternatively, manually point Python to the global SQLite version:

`import os
os.environ['PATH'] = "C:\\path\\to\\global\\sqlite;" + os.environ['PATH']
import sqlite3
print(sqlite3.sqlite_version)` 

* * *

### **2.3 Use the Global SQLite Version Inside Virtual Environments**

If you must use the global SQLite version inside your virtual environment, you can do the following:

#### **Method 1: Uninstall SQLite from Virtual Environment**

`pip uninstall pysqlite3-binary` 

#### **Method 2: Create a Virtual Environment Without SQLite**

`python -m venv --without-pip venv` 

This prevents SQLite from being installed inside the virtual environment.

* * *

### **2.4 Verify the Change**

After applying the changes, restart your terminal and check again:

`python -c "import sqlite3; print(sqlite3.sqlite_version)"` 

* * *

**Conclusion**
--------------

*   **For linting issues**, ensure the correct linter is installed, manually test it, and configure the editor settings.
*   **For SQLite version conflicts**, modify environment variables or force Python to use the global SQLite installation.
