**What is operator overloading? Provide an example.**

**Answer**: Operator overloading allows operators to behave differently based on the types of operands or arguments they operate on. In Python, it is achieved by defining special methods or magic methods that correspond to different operators. For example, the + operator can be overloaded to perform addition on numbers as well as concatenate strings.

```
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        else:
            raise TypeError("Unsupported operand type")

v1 = Vector(2, 3)
v2 = Vector(4, 5)
v3 = v1 + v2  # Calls the __add__ method
print(v3.x, v3.y)  # Output: 6, 8
```

**10. What are access specifiers? What is their significance in OOPs?**

Access specifiers are special types of keywords that are used to specify or control the accessibility of entities like classes, methods, and so on. ****Private****, ****Public****, and ****Protected**** are examples of access specifiers or access modifiers.  
The key components of OOPs, encapsulation and data hiding, are largely achieved because of these access specifiers.

  **What is the difference between overloading and overriding?

A compile-time polymorphism feature called ****overloading**** allows an entity to have numerous implementations of the same name. Method overloading and operator overloading are two examples.

****Overriding**** is a form of runtime polymorphism where an entity with the same name but a different implementation is executed. It is implemented with the help of virtual functions.

***21. How much memory does a class occupy?**

Classes do not use memory. They merely serve as a template from which items are made. Now, objects actually initialize the class members and methods when they are created, using memory in the process.


### **Why Use OOP?**

Object-Oriented Programming (OOP) helps structure code using objects, making it modular, reusable, maintainable, and scalable. It enhances security with encapsulation, reduces code duplication with inheritance, and provides flexibility using polymorphism.

#### **Real-Life Example**

A **Car** class represents real-world cars. Each car object has attributes like speed and fuel level and behaviors like `start()` and `stop()`. Different types of cars, such as electric and fuel-based, can inherit the general `Car` class and define their unique behaviors.

##### **Code Example**

```python
class Car:
    def start(self):
        print("Car started")

class ElectricCar(Car):
    def start(self):
        print("Electric Car started silently")  # Overriding

car1 = Car()
car2 = ElectricCar()
car1.start()  # Car started
car2.start()  # Electric Car started silently
```

---

### **Four Pillars of OOP**

1. **Encapsulation**  
    Encapsulation binds data (attributes) and methods (functions) together and restricts direct access to the internal state. This protects data integrity.
    
    **Example:** A bank account class hides its balance and allows deposits and withdrawals through methods only.
    
    ```python
    class BankAccount:
        def __init__(self, balance):
            self.__balance = balance  # Private variable
    
        def deposit(self, amount):
            self.__balance += amount
    
        def get_balance(self):
            return self.__balance
    
    account = BankAccount(1000)
    account.deposit(500)
    print(account.get_balance())  # 1500
    ```
    
2. **Abstraction**  
    Abstraction hides unnecessary implementation details and exposes only essential functionalities.
    
    **Example:** A TV remote allows users to change channels but hides its internal circuit operations.
    
    ```python
    from abc import ABC, abstractmethod
    
    class Remote(ABC):
        @abstractmethod
        def press_power_button(self):
            pass
    
    class TVRemote(Remote):
        def press_power_button(self):
            print("TV turned ON/OFF")
    
    remote = TVRemote()
    remote.press_power_button()
    ```
    
3. **Inheritance**  
    Inheritance allows a class to acquire properties and behaviors of another class, promoting reusability.
    
    **Example:** A `Dog` class inherits from an `Animal` class.
    
    ```python
    class Animal:
        def eat(self):
            print("Eating")
    
    class Dog(Animal):
        def bark(self):
            print("Barking")
    
    dog = Dog()
    dog.eat()  # Eating
    dog.bark()  # Barking
    ```
    
4. **Polymorphism**  
    Polymorphism allows a single interface to be used for different data types or behaviors, such as method overriding.
    
    **Example:** Different animals make different sounds but use the same method name `make_sound()`.
    
    ```python
    class Animal:
        def make_sound(self):
            print("Animal makes sound")
    
    class Dog(Animal):
        def make_sound(self):
            print("Dog barks")
    
    animal = Animal()
    dog = Dog()
    animal.make_sound()  # Animal makes sound
    dog.make_sound()  # Dog barks
    ```
    

---

### **Method Overloading and Overriding**

**Method Overloading** occurs when multiple methods have the same name but different parameter lists within the same class. Python does not support traditional method overloading but can achieve it using default arguments.

**Example:**

```python
class Calculator:
    def add(self, a, b, c=0):  # Simulating overloading using default values
        return a + b + c

calc = Calculator()
print(calc.add(2, 3))      # 5
print(calc.add(2, 3, 4))   # 9
```

**Method Overriding** occurs when a subclass provides a specific implementation of a method already defined in its parent class.

**Example:**

```python
class Parent:
    def show(self):
        print("Parent class")

class Child(Parent):
    def show(self):
        print("Child class")

child = Child()
child.show()  # Child class
```

---

### **Static vs. Dynamic Binding**

**Static Binding** occurs at compile-time, meaning the function to be called is determined before execution. Python uses static binding in normal method calls.

**Example:**

```python
class A:
    def show(self):
        print("Static binding")

obj = A()
obj.show()  # Static binding
```

**Dynamic Binding** occurs at runtime when the function to be executed is determined based on the object's type. This happens with method overriding using polymorphism.

**Example:**

```python
class B:
    def show(self):
        print("Dynamic binding")

class C(B):
    def show(self):
        print("Overridden function")

obj = C()
obj.show()  # Overridden function
```

---

### **Diamond Problem in Multiple Inheritance**

The **Diamond Problem** occurs when a class inherits from two classes that share a common base class, causing ambiguity in method resolution.

**Example:**

```python
class A:
    def show(self):
        print("A's method")

class B(A):
    pass

class C(A):
    pass

class D(B, C):  # Diamond Problem
    pass

d = D()
d.show()  # A's method (Resolved using Method Resolution Order - MRO)
```

Python resolves this problem using the **MRO (Method Resolution Order)**, ensuring each class is visited only once.

---

### **Constructor and Destructor Call Order**

When an object is created in an inheritance hierarchy, constructors are called from the base class to the derived class. Destructors are called in reverse order.

**Example:**

```python
class Base:
    def __init__(self):
        print("Base Constructor")

    def __del__(self):
        print("Base Destructor")

class Derived(Base):
    def __init__(self):
        super().__init__()
        print("Derived Constructor")

    def __del__(self):
        print("Derived Destructor")
        super().__del__()

obj = Derived()
del obj  # Explicitly deleting to see the destructor call order
```

**Output:**

```
Base Constructor
Derived Constructor
Derived Destructor
Base Destructor
```

---

### **Pointers in Python**

Unlike C++, Python does not have traditional pointers, but objects are referenced through memory addresses.

**Example:**

```python
x = 10
y = x  # Both reference the same memory location
print(id(x), id(y))  # Same address

# Changing one does not affect the other because integers are immutable
y = 20
print(id(y))  # Different address now
```

Python provides **weak references** using the `weakref` module to prevent objects from being garbage collected.

```python
import weakref

class MyClass:
    pass

obj = MyClass()
weak_obj = weakref.ref(obj)
print(weak_obj())  # Accessing the object
del obj
print(weak_obj())  # Now returns None
```

---

