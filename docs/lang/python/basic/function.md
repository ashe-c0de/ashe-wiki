# Function

A function is a reusable block of code that performs a specific task.

## Define a Function

```python
def greet():
    print("Hello Python!")
```

## Call a Function

```python
def greet():
    print("Hello Python!")

greet()
```

### Output

```text
Hello Python!
```

## Function with Parameters

Parameters allow you to pass data into a function.

```python
def greet(name):
    print("Hello", name)

greet("Alice")
```

### Output

```text
Hello Alice
```

## Parameter Types

Python supports different kinds of parameters.

### Positional Parameters

Arguments are matched to parameters by their position.

```python
def introduce(name, age):
    print(name, age)

introduce("Alice", 20)
```

### Keyword Parameters

Arguments are matched by parameter name.

```python
def introduce(name, age):
    print(name, age)

introduce(age=20, name="Alice")
```

### Default Parameters

Parameters can have default values.

```python
def greet(name="Guest"):
    print("Hello", name)

greet()
greet("Alice")
```

### Output

```text
Hello Guest
Hello Alice
```

## Type Hints for Parameters

You can specify the expected parameter and return types using type hints.

```python
def add(a: int, b: int) -> int:
    return a + b

result = add(3, 5)
print(result)
```

Type hints improve code readability and help development tools detect errors, but they are not enforced at runtime by Python.

## Summary

* Define functions with `def`
* Pass data using parameters
* Use positional, keyword, and default parameters
* Add type hints to document parameter and return types
