# Collections

Collections are data structures used to store multiple values in a single variable.

Python provides four main built-in collection types:

* List
* Tuple
* Dictionary
* Set

---

## List

A list is ordered and changeable (mutable). It allows duplicate values.

```python
fruits = ["apple", "banana", "orange"]

print(fruits)
print(fruits[0])
```

### Modify List

```python
fruits = ["apple", "banana", "orange"]

fruits.append("grape")
fruits[1] = "mango"

print(fruits)
```

### Output

```text
['apple', 'mango', 'orange', 'grape']
```

---

## Tuple

A tuple is ordered but **immutable** (cannot be changed after creation).

```python
point = (10, 20)

print(point)
print(point[0])
```

### Output

```text
(10, 20)
10
```

### Key Point

* Use tuple when data should not be modified

---

## Dictionary

A dictionary stores data in key-value pairs.

```python
user = {
    "name": "Alice",
    "age": 20
}

print(user)
print(user["name"])
```

### Add or Update Value

```python
user["age"] = 21
user["city"] = "Singapore"

print(user)
```

### Output

```text
{'name': 'Alice', 'age': 21, 'city': 'Singapore'}
```

---

## Set

A set is unordered and does not allow duplicate values.

```python
numbers = {1, 2, 3, 3, 4}

print(numbers)
```

### Output

```text
{1, 2, 3, 4}
```

### Common Operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # Union
print(a & b)  # Intersection
print(a - b)  # Difference
```

---

## Comparison of Collections

| Type       | Ordered           | Mutable | Duplicates          |
| ---------- | ----------------- | ------- | ------------------- |
| List       | Yes               | Yes     | Yes                 |
| Tuple      | Yes               | No      | Yes                 |
| Dictionary | Yes (Python 3.7+) | Yes     | Keys No, Values Yes |
| Set        | No                | Yes     | No                  |

---

## Null Check

```
items = []

if items is None:
    print("no items were provided")
elif not items:
    print("items were provided, but the list is empty")
```

---

## Summary

* Use **list** for ordered, changeable data
* Use **tuple** for fixed data
* Use **dictionary** for key-value mapping
* Use **set** for unique values
