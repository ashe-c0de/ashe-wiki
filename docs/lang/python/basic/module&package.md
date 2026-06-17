# Module & Package

Modules and packages help you organize Python code into reusable components.

---

## Module

A module is a single Python file (`.py`) that contains functions, variables, or classes.

---

### Create a Module

**utils.py**

```python
def greet(name):
    print(f"Hello {name}")

def add(a, b):
    return a + b
```

---

### Import a Module

**main.py**

```python
import utils

utils.greet("Alice")
print(utils.add(2, 3))
```

### Output

```text
Hello Alice
5
```

---

### Import Specific Functions

```python
from utils import greet, add

greet("Bob")
print(add(10, 20))
```

---

### Import with Alias

```python
import utils as u

u.greet("Charlie")
```

---

## Built-in Modules

Python comes with many built-in modules.

### Example: math

```python
import math

print(math.sqrt(16))
print(math.pi)
```

---

### Example: random

```python
import random

print(random.randint(1, 10))
```

---

## Package

A package is a folder containing multiple modules.

It must include an `__init__.py` file (can be empty).

---

### Example Structure

```
mypackage/
    __init__.py
    math_utils.py
    string_utils.py
```

---

### math_utils.py

```python
def multiply(a, b):
    return a * b
```

---

### Import from Package

```python
from mypackage import math_utils

print(math_utils.multiply(3, 4))
```

---

### Import Specific Function from Package

```python
from mypackage.math_utils import multiply

print(multiply(5, 6))
```

---

## Why Use Modules & Packages

* Organize code into smaller parts
* Improve code reuse
* Make projects easier to maintain
* Avoid name conflicts

---

## Summary

* A **module** is a single `.py` file
* A **package** is a folder containing modules
* Use `import` to reuse code
* Use packages to structure larger projects
