# Data Types

Python is dynamically typed, which means you do not need to declare variable types explicitly.

## Integer

Integers represent whole numbers.

```python
age = 18

print(age)
print(type(age))
```

### Output

```text
18
<class 'int'>
```

## Float

Floats represent decimal numbers.

```python
price = 19.99

print(price)
print(type(price))
```

### Output

```text
19.99
<class 'float'>
```

## String

Strings represent text.

```python
name = "Alice"

print(name)
print(type(name))
```

### Output

```text
Alice
<class 'str'>
```

### String Formatting

```python
name = "Alice"
age = 20

print(f"{name} is {age} years old")
```

### Output

```text
Alice is 20 years old
```

## Boolean

Booleans represent truth values.

```python
is_active = True

print(is_active)
print(type(is_active))
```

### Output

```text
True
<class 'bool'>
```

## None

`None` represents the absence of a value.

```python
result = None

print(result)
print(type(result))
```

### Output

```text
None
<class 'NoneType'>
```

## Type Conversion

Convert values between different types.

```python
age = "18"

print(int(age))
print(float(age))
```

### Output

```text
18
18.0
```

## Check a Type

Use `type()` to inspect a value's type.

```python
name = "Alice"

print(type(name))
```

### Output

```text
<class 'str'>
```

## Common Built-in Types

| Type     | Description    | Example |
| -------- | -------------- | ------- |
| int      | Integer        | 18      |
| float    | Decimal number | 19.99   |
| str      | Text           | "Hello" |
| bool     | Boolean value  | True    |
| NoneType | Empty value    | None    |

## Summary

* `int` stores whole numbers
* `float` stores decimal numbers
* `str` stores text
* `bool` stores `True` or `False`
* `None` represents no value
* Use `type()` to inspect a type
* Use functions such as `int()`, `float()`, and `str()` for type conversion
