
Let us assign a variable x to value 5.

```python
x = 5
```

When x = 5 is executed, Python creates an object to represent the value 5 and makes x reference this object.

Now, let's assign another variable y to the variable x.

```python
y = x
```

This statement creates y and references the same object as x, not x itself. This is called a Shared Reference, where multiple variables reference the same object.

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/python/py-assign.png)

Now, if we write

```python
x = 'Ashe'
```

Python creates a new object for the value "Ashe" and makes x reference this new object.

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/python/share-ref.png)

The variable y remains unchanged, still referencing the original object 5. Now, If we assign a new value to y:

```python
y = 'Computer'
```

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/python/garbage.png)

- Python creates yet another object for "Computer" and updates y to reference it.

- The original object 5 no longer has any references and becomes eligible for garbage collection.

- Python variables hold references to objects, not the actual objects themselves.

- Reassigning a variable does not affect other variables referencing the same object unless explicitly updated.

We can remove a variable from the namespace using the del keyword. This deletes the variable and frees up the memory it was using.

```python
x = 10
del x
print(x)
```

Output
```bash
Traceback (most recent call last):
  File "<path>\<filename>.py", line 6, in <module>
    print(x)
          ^
NameError: name 'x' is not defined
```
