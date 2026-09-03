# Exception Handling

Exceptions are errors that occur during program execution.

Python provides `try`, `except`, `else`, and `finally` to handle errors gracefully.

---

## Basic try-except

```python
try:
    result = 10 / 0
except:
    print("An error occurred")
```

---

## Catch Specific Exceptions

It is better to catch specific errors.

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
```

---

## Multiple Exceptions

```python
try:
    value = int("abc")
except ZeroDivisionError:
    print("Division error")
except ValueError:
    print("Invalid value")
```

---

## else Block

Runs only if no exception occurs.

```python
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Error")
else:
    print("Success:", result)
```

### Output

```text
Success: 5.0
```

---

## finally Block

Always runs, whether there is an error or not.

```python
try:
    file = open("test.txt", "r")
    content = file.read()
except FileNotFoundError:
    print("File not found")
finally:
    print("Execution finished")
```

---

## Raising Exceptions

You can manually raise errors using `raise`.

```python
age = -1

if age < 0:
    raise ValueError("Age cannot be negative")
```

---

## Custom Exception Handling Pattern

```python 
def divide(a, b):
    if b == 0:
        raise ValueError("b cannot be zero")
    return a / b

try:
    print(divide(10, 0))
except ValueError as e:
    print("Error:", e)
```

---

## Common Built-in Exceptions

| Exception         | Description        |
| ----------------- | ------------------ |
| ValueError        | Invalid value      |
| TypeError         | Wrong type         |
| ZeroDivisionError | Division by zero   |
| FileNotFoundError | Missing file       |
| IndexError        | Invalid list index |

---

## Summary

* Use `try` to wrap risky code
* Use `except` to handle errors
* Use `else` for success flow
* Use `finally` for cleanup
* Use `raise` to trigger exceptions manually
