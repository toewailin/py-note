### **1. Integers (`int`)**
Integers represent whole numbers without a decimal point. They can be positive, negative, or zero. Python handles arbitrarily large integers (limited by memory), so there's no fixed size for integers.

#### Common Methods and Operations:
- **Arithmetic Operations**: `+`, `-`, `*`, `/`, `//`, `%`, `**` for power, and `abs()`.
- **Conversion**: `int()` - Convert other data types (e.g., string, float) into an integer.
- **Absolute value**: `abs()` - Returns the absolute value of the integer.

#### Example:

```python
# Arithmetic operations
x = 10
y = 3
print(x + y)  # 13
print(x - y)  # 7
print(x * y)  # 30
print(x / y)  # 3.333...
print(x // y) # 3 (floor division)
print(x % y)  # 1 (modulo)
print(x ** y) # 1000 (x raised to power y)
print(abs(-10))  # 10 (absolute value)

# Conversion
z = "123"
int_z = int(z)  # Convert string to integer
print(int_z)  # 123
```

---

### **2. Strings (`str`)**
Strings represent a sequence of characters and are used to store and manipulate text. Strings in Python are immutable, meaning once created, their content cannot be modified.

#### Common Methods and Operations:
- **String formatting**: `.format()`, f-strings (`f""`)
- **String manipulations**: `.lower()`, `.upper()`, `.title()`, `.replace()`, `.split()`, `.strip()`, `.find()`, `.count()`.
- **String comparison**: `==`, `!=`, `>`, `<`, `>=`, `<=`.

#### Example:

```python
# String manipulation
text = "hello world"
print(text.upper())  # "HELLO WORLD"
print(text.lower())  # "hello world"
print(text.title())  # "Hello World"
print(text.replace("world", "there"))  # "hello there"

# Split and strip
text2 = "  Hello, World!  "
print(text2.strip())  # "Hello, World!"
print(text2.split(", "))  # ['Hello', 'World!']

# String formatting
name = "Alice"
greeting = f"Hello, {name}!"
print(greeting)  # "Hello, Alice!"
```

---

### **3. Floating Point Numbers (`float`)**
Floating-point numbers represent real numbers and are used for decimals. They are approximate values and have a limited precision due to how they are stored in memory.

#### Common Methods and Operations:
- **Arithmetic operations**: `+`, `-`, `*`, `/`, `//`, `%`, `**`.
- **Conversion**: `float()` - Convert other data types (e.g., string, integer) into a floating-point number.
- **Round**: `round()` - Rounds the number to a specified number of decimal places.
- **Absolute value**: `abs()` - Returns the absolute value of a floating-point number.

#### Example:

```python
# Arithmetic operations
a = 10.5
b = 3.2
print(a + b)  # 13.7
print(a - b)  # 7.3
print(a * b)  # 33.6
print(a / b)  # 3.28125
print(a // b) # 3.0 (floor division)
print(a ** 2) # 110.25

# Conversion
str_num = "12.34"
float_num = float(str_num)
print(float_num)  # 12.34

# Rounding
pi = 3.14159
print(round(pi, 2))  # 3.14
```

---

### **4. Boolean (`bool`)**
Boolean values are used for logic and decision-making. They can have two values: `True` or `False`. In Python, `True` is treated as `1` and `False` is treated as `0`.

#### Common Methods and Operations:
- **Boolean logic**: `and`, `or`, `not`, `==`, `!=`, `>`, `<`, `>=`, `<=`.

#### Example:

```python
x = True
y = False

# Boolean logic
print(x and y)  # False
print(x or y)   # True
print(not x)    # False

# Comparison
print(x == y)   # False
print(x != y)   # True
```

---

### **5. List (`list`)**
Lists are ordered collections of items that can be of any data type. Lists are mutable, meaning you can add, remove, or change elements.

#### Common Methods and Operations:
- **Adding elements**: `.append()`, `.extend()`, `.insert()`.
- **Removing elements**: `.remove()`, `.pop()`, `.clear()`.
- **Searching**: `.index()`, `.count()`.
- **Sorting and Reversing**: `.sort()`, `.reverse()`.
- **Slicing**: `list[start:end]`.

#### Example:

```python
# Create and modify lists
fruits = ["apple", "banana", "cherry"]
fruits.append("orange")  # Add an item
print(fruits)  # ['apple', 'banana', 'cherry', 'orange']

fruits.remove("banana")  # Remove item by value
print(fruits)  # ['apple', 'cherry', 'orange']

# Slicing and other operations
print(fruits[1:3])  # ['cherry', 'orange']
fruits.sort()
print(fruits)  # ['apple', 'cherry', 'orange']

# Count occurrence
print(fruits.count("apple"))  # 1
```

---

### **6. Tuple (`tuple`)**
Tuples are ordered, immutable collections of items. Once created, the contents of a tuple cannot be changed. They can be used for data integrity purposes or as keys in dictionaries.

#### Common Methods and Operations:
- **Count**: `.count()` - Counts the occurrence of a value in the tuple.
- **Indexing**: `.index()` - Returns the index of the first occurrence of a value.

#### Example:

```python
# Create and manipulate tuples
coordinates = (10, 20, 30)
print(coordinates[0])  # 10
print(coordinates[1:3])  # (20, 30)

# Tuple methods
print(coordinates.count(10))  # 1
print(coordinates.index(30))  # 2
```

---

### **7. Dictionary (`dict`)**
Dictionaries are unordered collections of key-value pairs. Keys must be unique and immutable, while values can be any data type.

#### Common Methods and Operations:
- **Adding/Updating items**: `.update()`, direct assignment (`dict[key] = value`).
- **Removing items**: `.pop()`, `.popitem()`, `.clear()`.
- **Getting items**: `.get()`, `.keys()`, `.values()`, `.items()`.

#### Example:

```python
# Create and modify dictionaries
person = {"name": "Alice", "age": 30}
person["location"] = "New York"  # Add a new key-value pair
print(person)  # {"name": "Alice", "age": 30, "location": "New York"}

# Get item by key
print(person.get("name"))  # "Alice"

# Remove an item
person.pop("age")
print(person)  # {"name": "Alice", "location": "New York"}
```

---

### **8. Set (`set`)**
A set is an unordered collection of unique items. It does not allow duplicates, and it is mutable, meaning you can add or remove items. Sets are particularly useful when you need to check membership or perform mathematical set operations like union and intersection.

#### Common Methods and Operations:
- **Add items**: `.add()` - Adds a single item to the set.
- **Remove items**: `.remove()` - Removes an item from the set; raises a KeyError if the item doesn't exist.
- **Pop item**: `.pop()` - Removes and returns an arbitrary item from the set.
- **Union**: `.union()` or `|` - Combines two sets and returns a new set.
- **Intersection**: `.intersection()` or `&` - Returns the set of common elements between two sets.
- **Difference**: `.difference()` or `-` - Returns elements in the first set but not in the second.

#### Example:

```python
fruits = {"apple", "banana", "cherry"}
fruits.add("orange")  # Add "orange" to the set
print(fruits)  # {"apple", "banana", "cherry", "orange"}

# Remove an element
fruits.remove("banana")
print(fruits)  # {"apple", "cherry", "orange"}

# Union of sets
other_fruits = {"grape", "melon"}
combined_fruits = fruits.union(other_fruits)
print(combined_fruits)  # {"apple", "cherry", "orange", "grape", "melon"}

# Intersection of sets
common_fruits = fruits.intersection({"apple", "melon", "cherry"})
print(common_fruits)  # {"apple", "cherry"}
```

---

### **9. Frozen Set (`frozenset`)**
A frozen set is similar to a set but is **immutable**. Once created, you cannot modify its contents (i.e., add or remove items). This makes frozen sets hashable and usable as dictionary keys.

#### Common Methods and Operations:
- **Set operations**: `.union()`, `.intersection()`, `.difference()`, etc. These methods can be used with frozensets as well.
- **No modification methods**: Unlike sets, frozensets don’t have methods to add or remove elements.

#### Example:

```python
frozen_fruits = frozenset({"apple", "banana", "cherry"})
print(frozen_fruits)  # frozenset({'apple', 'banana', 'cherry'})

# Frozen set operations
other_fruits = frozenset({"cherry", "date", "fig"})
common = frozen_fruits.intersection(other_fruits)
print(common)  # frozenset({'cherry'})
```

---

### **10. Range (`range`)**
The `range()` function is used to generate a sequence of numbers, often used in `for` loops. A `range` object represents an immutable sequence of numbers, which can be used to iterate over a loop.

#### Common Methods and Operations:
- **List Conversion**: `list(range())` - Convert a `range` object to a list.
- **Access elements**: You can access elements from a `range` like a sequence.

#### Example:

```python
numbers = range(1, 10, 2)  # Creates a range from 1 to 9 with a step of 2
print(list(numbers))  # [1, 3, 5, 7, 9]
```

---

### **11. Byte Types (`bytes`, `bytearray`, `memoryview`)**
Byte-related types are useful for handling binary data. The `bytes` type is immutable, while `bytearray` is mutable. `memoryview` allows you to access memory directly.

#### Common Methods and Operations:
- **Byte to String**: `.decode()` - Decodes a byte object to a string.
- **String to Byte**: `.encode()` - Encodes a string to bytes.
- **Mutable byte array**: Use `bytearray` to modify byte objects.
- **View Memory**: `memoryview()` - Allows for memory sharing without copying.

#### Example:

```python
# bytes
data = bytes([50, 100, 76])
print(data.decode())  # "2dL" - converting byte to string

# bytearray (mutable)
mutable_data = bytearray([50, 100, 76])
mutable_data[0] = 65  # Modify first byte
print(mutable_data)  # bytearray(b'A, 100, 76')

# memoryview
mv = memoryview(b"Hello")
print(mv[0])  # 72 (ASCII value of 'H')
```

---

### **12. Complex Numbers (`complex`)**
Complex numbers in Python are represented by a real and imaginary part. The imaginary part is denoted by a `j` at the end (e.g., `3 + 5j`).

#### Common Methods and Operations:
- **Real and Imaginary Parts**: `.real` and `.imag` - Returns the real and imaginary parts of the complex number.
- **Absolute Value**: `abs()` - Returns the magnitude of the complex number.

#### Example:

```python
z = 3 + 5j
print(z.real)  # 3.0
print(z.imag)  # 5.0
print(abs(z))  # 5.830951894845301 (Magnitude of the complex number)
```

---

### **13. Callable Objects**
Callable objects are any objects in Python that can be called as a function (e.g., functions, methods, classes, or instances of a class that implement the `__call__` method).

#### Common Methods and Operations:
- **Call a function or method**: Use the `()` to invoke it.

#### Example:

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))  # "Hello, Alice!"
```

---

### **14. Memory Management and Garbage Collection**

Python handles memory management automatically using **reference counting** and a **garbage collector**. Objects in Python are automatically managed, and you don’t need to manually allocate or deallocate memory.

#### Common Methods and Operations:
- **`del`**: Deletes a variable or an object.
- **`gc` module**: For controlling the garbage collection process.
- **`id()`**: Returns the identity (memory address) of an object.

#### Example:

```python
import gc

# Creating a simple object
a = "Hello"
b = a
print(id(a), id(b))  # Both variables point to the same object in memory

# Deleting object
del a  # Removes reference to the object
print(gc.collect())  # Manually invoke garbage collection
```

---

## **Summary of Data Types and Key Methods**

| Data Type    | Key Methods/Operations                           | Description |
|--------------|---------------------------------------------------|-------------|
| **int**      | `abs()`, `int()`, `str()`, arithmetic operations | Whole numbers (integer arithmetic, conversions) |
| **str**      | `.lower()`, `.upper()`, `.replace()`, `.split()` | Sequence of characters (string manipulation) |
| **float**    | `round()`, `abs()`, `str()`                      | Decimal numbers (floating-point operations) |
| **bool**     | `and`, `or`, `not`, `bool()`                     | Boolean values (`True`/`False`) |
| **list**     | `.append()`, `.pop()`, `.sort()`, `.reverse()`    | Ordered, mutable collection |
| **tuple**    | `.count()`, `.index()`                           | Ordered, immutable collection |
| **dict**     | `.get()`, `.keys()`, `.values()`, `.items()`      | Key-value pairs (dictionary) |
| **set**      | `.add()`, `.remove()`, `.union()`, `.intersection()` | Unordered, unique elements |
| **frozenset**| `.union()`, `.intersection()`                    | Immutable set |
| **range**    | `list(range())`                                  | Sequence of numbers |
| **bytes**    | `.decode()`, `.encode()`                         | Immutable byte sequences |
| **bytearray**| `.append()`, `.decode()`, `.encode()`            | Mutable byte sequences |
| **complex**  | `.real`, `.imag`, `abs()`                        | Complex numbers (real and imaginary parts) |

---
