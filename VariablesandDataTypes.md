## Variables and Data Types

In Python, variables are dynamically typed, meaning you don't need to declare their type explicitly. Python has a variety of built-in data types to handle different kinds of data.

### Example:

```python
# Integer: Whole numbers
age = 25  # 'age' is an integer type variable

# String: A sequence of characters
name = "Alice"  # 'name' is a string type variable

# Float: Decimal numbers
height = 5.6  # 'height' is a floating-point number

# Boolean: Represents True or False
is_student = True  # 'is_student' is a boolean type variable

# List: An ordered, mutable (can change) collection of items
fruits = ["apple", "banana", "cherry"]  # 'fruits' is a list
# Lists are mutable, so you can modify them by adding, removing, or changing elements
fruits.append("orange")  # Adding an element to the list
print(fruits)  # ['apple', 'banana', 'cherry', 'orange']

# Tuple: An ordered, immutable (cannot change) collection of items
coordinates = (4, 5)  # 'coordinates' is a tuple
# Tuples are immutable, meaning once they are created, you cannot change their elements
# For example, coordinates[0] = 10  # This will raise an error

# Dictionary: A collection of key-value pairs (unordered)
person = {
    "name": "Alice",  # key: "name", value: "Alice"
    "age": 25  # key: "age", value: 25
}  # 'person' is a dictionary
# You can change the values associated with keys in a dictionary, but you cannot change the keys
person["age"] = 26  # Changing the value associated with the "age" key
print(person)  # {'name': 'Alice', 'age': 26}

# Set: An unordered collection of unique elements (no duplicates allowed)
unique_numbers = {1, 2, 3, 4}  # 'unique_numbers' is a set
# Sets are unordered, meaning the items do not have a specific index and cannot be accessed using indexing
# Sets also do not allow duplicate elements
unique_numbers.add(5)  # Adding a new element to the set
print(unique_numbers)  # {1, 2, 3, 4, 5}
```

### Data Type Characteristics:

1. **Integer**: 
   - Represents whole numbers (positive, negative, or zero).
   - Examples: `1`, `-3`, `100`.

2. **String**: 
   - A sequence of characters enclosed in single (`'`) or double (`"`) quotes.
   - Strings are immutable, which means once a string is created, it cannot be modified.
   - Example: `"Hello, World!"`.
   
3. **Float**: 
   - A decimal number used to represent real values.
   - Example: `3.14`, `0.5`, `-0.123`.

4. **Boolean**: 
   - Represents truth values, either `True` or `False`.
   - Used in conditions and logical operations.
   - Example: `is_active = True`.

5. **List**:
   - A collection of items ordered by index. Lists are mutable, meaning you can modify the contents (add, remove, or change items).
   - Example: `fruits = ["apple", "banana", "cherry"]`.
   - Lists can store elements of different data types.
   
6. **Tuple**:
   - Like a list, but **immutable**. Once a tuple is created, its contents cannot be changed.
   - Example: `coordinates = (4, 5)`.
   - Tuples are often used for fixed collections of data, such as coordinates or RGB values.
   
7. **Dictionary**:
   - A collection of key-value pairs, where each key is unique. Dictionaries are unordered, and the values can be modified.
   - Example: `person = {"name": "Alice", "age": 25}`.
   - You cannot modify dictionary keys after creation, but you can change their corresponding values.
   
8. **Set**:
   - A collection of unique, unordered elements. Sets do not allow duplicate values, and you cannot access elements using an index.
   - Example: `unique_numbers = {1, 2, 3}`.
   - Sets are useful when you need to store unique elements and perform mathematical set operations like union, intersection, and difference.

---

### Type Checking:

To check the data type of a variable, you can use the `type()` function:

```python
print(type(name))  # <class 'str'>
print(type(age))  # <class 'int'>
```
