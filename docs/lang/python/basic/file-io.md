# File I/O

File I/O (Input/Output) allows Python programs to read from and write to files.

---

## Open a File

Use the `open()` function.

```python id="a1b2c3"
file = open("example.txt", "r")
content = file.read()
file.close()

print(content)
```

### File Modes

| Mode | Description       |
| ---- | ----------------- |
| `r`  | Read (default)    |
| `w`  | Write (overwrite) |
| `a`  | Append            |
| `x`  | Create new file   |

---

## Read a File

### Read Entire File

```python id="d4e5f6"
with open("example.txt", "r") as file:
    content = file.read()
    print(content)
```

---

### Read Line by Line

```python id="g7h8i9"
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())
```

---

## Write to a File

⚠️ `w` mode will overwrite existing content.

```python id="j1k2l3"
with open("output.txt", "w") as file:
    file.write("Hello Python\n")
    file.write("File I/O example\n")
```

---

## Append to a File

```python id="m4n5o6"
with open("output.txt", "a") as file:
    file.write("This is appended text\n")
```

---

## Check File Existence

```python id="p7q8r9"
import os

if os.path.exists("output.txt"):
    print("File exists")
else:
    print("File not found")
```

---

## Read File into a List

```python id="s1t2u3"
with open("example.txt", "r") as file:
    lines = file.readlines()

print(lines)
```

---

## Common Pattern: with Statement

Using `with` automatically closes the file.

```python id="v4w5x6"
with open("example.txt", "r") as file:
    content = file.read()
```

---

## Summary

* Use `open()` to work with files
* Use `r`, `w`, `a` modes for reading/writing
* Prefer `with` statement for safe file handling
* Use `read()`, `readlines()`, or iteration to read files
* Always close files if not using `with`
