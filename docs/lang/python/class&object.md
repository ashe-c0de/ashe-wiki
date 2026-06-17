# Class & Object

Python is an object-oriented programming (OOP) language. OOP helps you structure code using **classes** and **objects**.

---

## Class

A class is a blueprint for creating objects.

```python id="a1b2c3"
class Person:
    pass
```

---

## Object

An object is an instance of a class.

```python id="d4e5f6"
class Person:
    pass

p1 = Person()
print(p1)
```

---

## Constructor (`__init__`)

The constructor is called when an object is created.

```python id="g7h8i9"
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p1 = Person("Alice", 20)

print(p1.name)
print(p1.age)
```

---

## Instance Variables

Variables defined using `self` belong to each object.

```python id="j1k2l3"
class Person:
    def __init__(self, name):
        self.name = name

p1 = Person("Alice")
p2 = Person("Bob")

print(p1.name)
print(p2.name)
```

---

## Instance Methods

Methods inside a class operate on object data.

```python id="m4n5o6"
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello, I am {self.name}")

p1 = Person("Alice")
p1.greet()
```

---

## Self Keyword

`self` refers to the current object.

```python id="p7q8r9"
class Person:
    def show(self):
        print(self)

p1 = Person()
p1.show()
```

---

## Multiple Objects

Each object has its own data.

```python id="s1t2u3"
class Person:
    def __init__(self, name):
        self.name = name

p1 = Person("Alice")
p2 = Person("Bob")

print(p1.name)
print(p2.name)
```

---

## Class Variables vs Instance Variables

### Class Variable (shared)

```python id="v4w5x6"
class Person:
    species = "Human"

print(Person.species)
```

### Instance Variable (unique)

```python id="y7z8a9"
class Person:
    def __init__(self, name):
        self.name = name
```

---

## Simple Example

```python id="b1c2d3"
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} is barking")

d1 = Dog("Buddy")
d1.bark()
```

---

## Summary

* A **class** is a blueprint
* An **object** is an instance of a class
* `__init__` is the constructor
* `self` refers to the current object
* Instance variables belong to objects
* Class variables are shared across all objects
