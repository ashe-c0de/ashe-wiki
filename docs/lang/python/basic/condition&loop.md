# Condition & Loop

Conditions allow a program to make decisions.

Loops allow a program to repeat actions.

## if Statement

Execute code only when a condition is true.

```python
age = 18

if age >= 18:
    print("Adult")
```

### Output

```text
Adult
```

## if-else Statement

Choose between two branches.

```python
age = 16

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

### Output

```text
Minor
```

## if-elif-else Statement

Handle multiple conditions.

```python
score = 85

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
else:
    print("D")
```

### Output

```text
B
```

## Comparison Operators

```python
a = 10
b = 20

print(a == b)  # Equal
print(a != b)  # Not equal
print(a < b)   # Less than
print(a <= b)  # Less than or equal
print(a > b)   # Greater than
print(a >= b)  # Greater than or equal
```

## Logical Operators

```python
age = 20
has_ticket = True

if age >= 18 and has_ticket:
    print("Allowed")
```

### Common Operators

| Operator | Meaning                        |
| -------- | ------------------------------ |
| and      | Both conditions are true       |
| or       | At least one condition is true |
| not      | Reverse a condition            |

## for Loop

Iterate over a sequence.

```python
for i in range(5):
    print(i)
```

### Output

```text
0
1
2
3
4
```

## while Loop

Repeat while a condition is true.

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

### Output

```text
0
1
2
3
4
```

## break

Exit a loop immediately.

```python
for i in range(10):
    if i == 5:
        break

    print(i)
```

### Output

```text
0
1
2
3
4
```

## continue

Skip the current iteration.

```python
for i in range(5):
    if i == 2:
        continue

    print(i)
```

### Output

```text
0
1
3
4
```

## Summary

* Use `if`, `elif`, and `else` for decision making
* Use comparison operators to compare values
* Use logical operators to combine conditions
* Use `for` when the number of iterations is known
* Use `while` when repeating until a condition changes
* Use `break` to stop a loop
* Use `continue` to skip an iteration
