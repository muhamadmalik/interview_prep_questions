## OOP Concepts Index

### The Four Pillars of OOP
* [[#1. Polymorphism? (Real-world examples often requested)|1. Polymorphism]]
* [[#9. Encapsulation?|9. Encapsulation]]
* [[#7. Abstraction?|7. Abstraction]]
* [[#2. Pillars of OOP? (Real-life examples often requested)|2. The Four Pillars (Overview)]]

### Inheritance Concepts
* [[#8. Multilevel vs. Multiple inheritance?|8. Multilevel vs. Multiple Inheritance]]
* [[#3. Diamond problem? How to solve it?|3. The Diamond Problem & MRO]]
* [[#6. Function overloading/overriding?|6. Method Overriding]]
* [[#22. Is a vs has a relationship?|22. Is-A vs. Has-A Relationship]]
* [[#1. What to write in class so that it can't be inherited? (e.g., final in Java, sealed in C#)|How to Create a "Final" Class]]
* [[#15. Constructor vs. Destructor? (Real-world examples)|Constructor Chaining (in `super().__init__`)]]

### Abstraction & Interfaces
* [[#5. Abstract classes and interfaces? (Difference often asked)|5. Abstract Classes vs. Interfaces]]
* [[#16. Virtual Functions?|16. Virtual Functions (Python's Default)]]
* [[#18. Difference between virtual and pure virtual functions?|18. Virtual vs. Pure Virtual Functions (Python Analogy)]]
* [[#2. How to avoid implementation of an abstract function?|2. Avoiding Abstract Method Implementation]]

### Class & Object Fundamentals
* [[#15. Constructor vs. Destructor? (Real-world examples)|15. Constructor & Destructor]]
* [[#14. Shallow and deep copy?|14. Shallow vs. Deep Copy]]
* [[#17. Copy constructor? (Why use it?)|17. Copy Constructor (Python's `copy` module)]]
* [[#20. Static keyword and Static function?|20. Static Methods & Attributes]]
* [[#7. Accessors, Mutators, Add Operator (Python __add__)?|Accessors, Mutators, and Operator Overloading (`__add__`)]]
* [[#8. How we limit creating not more than 10 objects?|Limiting the Number of Class Instances]]

### Relationships Between Classes
* [[#12. Association?|12. Association ("uses-a")]]
* [[#13. Aggregation?|13. Aggregation ("has-a", independent life)]]
* [[#23. Association / Composition / Aggregation?|23. Composition ("has-a", dependent life)]]

### Comparisons & Distinctions
* [[#4. Abstraction vs. Encapsulation? (Code examples often requested)|4. Abstraction vs. Encapsulation]]
* [[#6. Function overloading/overriding?|6. Overloading vs. Overriding]]
* [[#21. Difference between private and protected?|21. Private (`__`) vs. Protected (`_`)]]
* [[#Upcasting and downcasting?|Upcasting vs. Downcasting]]
* [[#Coupling and Cohesion?|Coupling vs. Cohesion]]
* [[#OOP vs functional programming?|OOP vs. Functional Programming]]

### Language-Specific & Advanced Topics
* [[#10. Function overloading?|10. Function Overloading (in Python)]]
* [[#11. Friend function / friend class?|11. Friend Functions (and why Python doesn't have them)]]
* [[#19. Dangling Pointer?|19. Dangling Pointers (and why Python doesn't have them)]]
* [[#3. Null pointer?|3. Null Pointers (Python's `None`)]]
* [[#Virtual table in OOP?|Virtual Tables (and Python's MRO)]]
* [[#4. .obj vs .exe?|4. .obj vs .exe (and Python's `.pyc` files)]]
* [[#5. Features of Java?|5. Features of Java]]

---
#### **1. Polymorphism? (Real-world examples often requested)**
**Polymorphism** (from Greek, meaning "many forms") is an OOP principle that allows objects of different classes to be treated as objects of a common superclass. It means you can call the same method on different objects, and each object will respond in its own specific way.

This is primarily achieved in Python through **Method Overriding**.

**Real-World Example:** Consider a payment processing system. You can have different payment methods, but they all share a common action: `pay`.

```python
import abc

# An abstract base class defining the common interface
class PaymentMethod(abc.ABC):
    @abc.abstractmethod
    def pay(self, amount):
        pass

# Concrete implementation for Credit Card
class CreditCard(PaymentMethod):
    def pay(self, amount):
        print(f"Paying ${amount} using Credit Card.")

# Concrete implementation for PayPal
class PayPal(PaymentMethod):
    def pay(self, amount):
        print(f"Paying ${amount} using PayPal.")

# The processing function doesn't need to know the specific type.
# It just knows it can call .pay() on any PaymentMethod object.
def process_payment(payment_method, amount):
    payment_method.pay(amount)

# Create objects of different classes
cc = CreditCard()
pp = PayPal()

# Call the same function with different objects
process_payment(cc, 100)  # Output: Paying $100 using Credit Card.
process_payment(pp, 150)  # Output: Paying $150 using PayPal.
```

---

#### **2. Pillars of OOP? (Real-life examples often requested)**
The four main pillars of Object-Oriented Programming are:
1.  **Encapsulation:** The bundling of data (attributes) and methods that operate on that data into a single unit (a class). It hides the internal state of an object from the outside.
    *   **Real-life Example:** A medical capsule encloses the medicine (data) inside a shell, protecting it from the outside.
2.  **Abstraction:** Hiding complex implementation details and showing only the essential features of the object. It focuses on *what* an object does instead of *how* it does it.
    *   **Real-life Example:** When you drive a car, you use a simple interface (steering wheel, pedals). You don't need to know the complex mechanics of the engine or transmission.
3.  **Inheritance:** A mechanism where a new class (child) inherits attributes and methods from an existing class (parent). It promotes code reuse and establishes an "is-a" relationship.
    *   **Real-life Example:** You inherit physical traits (like eye color) and genetic material from your parents.
4.  **Polymorphism:** The ability of an object to take on many forms. It allows a single interface (like a method name) to represent different underlying forms (implementations).
    *   **Real-life Example:** The word "cut" can mean different things depending on the context: a doctor can "cut" with a scalpel, a director can "cut" a scene, and an actor can "cut" their lines. The action is the same, but the result is different.

---

#### **3. Diamond problem? How to solve it?**
The **Diamond Problem** is a form of ambiguity that arises in multiple inheritance when a class inherits from two parent classes that both inherit from the same single grandparent class. If the grandparent has a method that is overridden by both parents, it becomes unclear which parent's version of the method the child class should inherit.

**How Python Solves It:** Python solves the diamond problem using a sophisticated algorithm called **C3 linearization**, which computes a **Method Resolution Order (MRO)**. The MRO is a predictable, deterministic, and linearized list of classes in the inheritance hierarchy. When a method is called, Python searches for it in the classes in the MRO list, in order, and uses the first implementation it finds.

**Example:**
```python
class A: # Grandparent
    def my_method(self):
        print("Method from class A")

class B(A): # Parent 1
    def my_method(self):
        print("Method from class B")

class C(A): # Parent 2
    def my_method(self):
        print("Method from class C")

class D(B, C): # Child inheriting from B and C
    pass

d_instance = D()
d_instance.my_method() # -> Python follows the MRO and calls B's method

# You can inspect the MRO directly
print(D.mro())
```
**Output:**
```
Method from class B
[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
```
The MRO for `D` is `[D, B, C, A, object]`. Python looks for `my_method` first in `D` (not found), then in `B` (found), so it calls `B`'s version and stops searching.

---

#### **4. Abstraction vs. Encapsulation? (Code examples often requested)**
While related, these two concepts solve different problems.

| Feature         | **Abstraction**                                                | **Encapsulation**                                                |
| --------------- | -------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Main Goal**   | To hide complexity.                                            | To hide data.                                                    |
| **Focus**       | On the external view of an object (the "what").                | On the internal implementation of an object (the "how").         |
| **Technique**   | Using abstract classes and methods (`abc` module).             | Using access control conventions (`_protected`, `__private`).      |
| **Analogy**     | A TV remote. You know what the buttons do, not how they work.    | The TV's internal casing, which protects the electronics.        |

**Abstraction Example (Hiding Complexity):**
```python
import abc

# We define WHAT a vehicle should do, but not HOW.
class Vehicle(abc.ABC):
    @abc.abstractmethod
    def start_engine(self):
        pass

# The user of this class only needs to know about start_engine().
```

**Encapsulation Example (Hiding Data):**
```python
class BankAccount:
    def __init__(self, initial_balance):
        # The balance is "private", hidden from direct outside access.
        self.__balance = initial_balance

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount

    def get_balance(self):
        # We provide a safe, controlled way to view the balance.
        return self.__balance

account = BankAccount(1000)
# account.__balance = -500 # This is prevented/discouraged.
account.deposit(500)
print(account.get_balance()) # Output: 1500
```

---

#### **5. Abstract classes and interfaces? (Difference often asked)**
*   **Abstract Class:** A class that is intended to be a blueprint for other classes. It cannot be instantiated on its own. It can contain both abstract methods (with no implementation) and concrete methods (with implementation). In Python, you create one by inheriting from `abc.ABC`.
*   **Interface:** An interface is a concept that defines a contract of methods that a class must implement. It contains *only* method signatures, no implementations.
    *   **Python does not have a formal `interface` keyword.** Instead, an abstract class where *all* methods are abstract effectively serves as an interface.

**The Key Difference:** An abstract class can provide default method implementations, while a pure interface cannot.

```python
import abc

# This acts as an "Interface" because it only defines the contract.
class ILogger(abc.ABC):
    @abc.abstractmethod
    def log(self, message):
        pass

# This is an "Abstract Class" because it provides some implementation.
class Shape(abc.ABC):
    def __init__(self, color):
        self.color = color # Concrete part

    @abc.abstractmethod # Abstract part
    def area(self):
        pass

    def describe(self): # Concrete part
        return f"This is a shape with color {self.color}."
```

---

#### **6. Function overloading/overriding?**
*   **Method Overriding:** This occurs when a subclass (child class) provides a specific implementation for a method that is already defined in its superclass (parent class). This is a fundamental part of polymorphism.

    ```python
    class Animal:
        def speak(self):
            return "Some generic sound"

    class Dog(Animal):
        # Overriding the speak method from the parent
        def speak(self):
            return "Woof!"

    d = Dog()
    print(d.speak()) # Output: Woof!
    ```

*   **Method Overloading:** This is the ability to define multiple methods with the same name within the same class, but with different parameters (either different number of parameters or different types).
    *   **Python does not support method overloading in the traditional sense.** If you define two methods with the same name, the last definition simply overwrites the previous one.

    You can **simulate** overloading using default arguments or `*args`.

    ```python
    # Simulating overloading with default arguments
    class Calculator:
        def add(self, a, b, c=0): # 'c' has a default value
            return a + b + c

    calc = Calculator()
    # We can call the same 'add' method with different numbers of arguments
    print(calc.add(2, 3))       # Output: 5
    print(calc.add(2, 3, 5))    # Output: 10
    ```

---

#### **7. Abstraction?**
Abstraction is the principle of hiding the implementation details of an object and exposing only the essential, high-level functionalities. It helps manage complexity by allowing us to work with a simplified model of a real-world entity. In Python, this is formally achieved using Abstract Base Classes (ABCs) from the `abc` module.

**Example:** You want to interact with different types of databases (PostgreSQL, SQLite) but with a single, simple interface.
```python
import abc

# Abstracting the concept of a database connection
class Database(abc.ABC):
    @abc.abstractmethod
    def connect(self):
        pass

    @abc.abstractmethod
    def query(self, sql):
        pass

# Concrete implementation, hiding the specific driver details
class PostgreSQL(Database):
    def connect(self):
        print("Connecting to PostgreSQL...")
        # ... complex connection logic here ...

    def query(self, sql):
        print(f"Running PostgreSQL query: {sql}")

# The code using this abstraction doesn't care if it's PostgreSQL or another DB.
db = PostgreSQL()
db.connect()
db.query("SELECT * FROM users")
```

---

#### **8. Multilevel vs. Multiple inheritance?**
*   **Multilevel Inheritance:** A class inherits from a parent class, which in turn inherits from another grandparent class. This forms a linear chain of inheritance (`A -> B -> C`).

    ```python
    class Grandparent:
        def G_method(self):
            print("Method from Grandparent")

    class Parent(Grandparent): # Inherits from Grandparent
        def P_method(self):
            print("Method from Parent")

    class Child(Parent): # Inherits from Parent
        pass

    c = Child()
    c.P_method()  # Inherited from Parent
    c.G_method() # Inherited from Grandparent
    ```

*   **Multiple Inheritance:** A class inherits from *more than one* parent class at the same time.

    ```python
    class Father:
        def skill_F(self):
            print("Gardening")

    class Mother:
        def skill_M(self):
            print("Cooking")

    class Child(Father, Mother): # Inherits from both Father and Mother
        pass

    c = Child()
    c.skill_F() # Output: Gardening
    c.skill_M() # Output: Cooking
    ```

---

#### **9. Encapsulation?**
Encapsulation is the practice of bundling data (attributes) and the methods that operate on that data into a single unit called a class. A key part of encapsulation is restricting direct access to an object's internal state. This prevents accidental or unauthorized modification of the data.

In Python, this is achieved by convention using naming:
*   **Public (`name`):** Accessible from anywhere.
*   **Protected (`_name`):** A hint that it's for internal or subclass use.
*   **Private (`__name`):** Name is mangled to `_ClassName__name`, making it difficult to access from outside.

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name          # Public attribute
        self.__salary = salary    # Private attribute

    def get_salary(self):
        # A public method to safely access the private data
        return self.__salary

    def set_salary(self, new_salary):
        # A public method to safely modify the private data with validation
        if new_salary > 0:
            self.__salary = new_salary
        else:
            print("Error: Salary must be positive.")

emp = Employee("Bob", 50000)
# print(emp.__salary)  # This will cause an AttributeError
print(f"{emp.name}'s salary is {emp.get_salary()}") # Correct way
emp.set_salary(55000)
```

---

#### **10. Function overloading?**
This question is a duplicate of #6.

To reiterate: Python does **not** support traditional function overloading where you can have multiple functions with the same name but different parameter types/counts. The last function definition with a given name is the only one that exists. The common workaround is to use default arguments or variable arguments (`*args`, `**kwargs`) to create a single flexible function that can handle different call patterns.

---

#### **11. Friend function / friend class?**
The concepts of **friend functions** and **friend classes** originate from C++. A "friend" is a function or class that is granted special access to the `private` and `protected` members of another class, even though it is not a member of that class itself.

**Python does not have this concept.** The Python philosophy is often summarized as "We are all consenting adults here." Access control is based on convention (`_` and `__`) rather than strict enforcement. If a developer needs to, they *can* access a name-mangled `__private` variable, but the naming convention signals that they should not. There is no mechanism to formally grant special access to an outside function or class.

---

#### **12. Association?**
**Association** is the most general relationship between two classes. It simply means that objects of the two classes are connected or "associated" with each other. It's often described as a "uses-a" relationship. The objects can exist independently of each other. Aggregation and Composition are more specific types of association.

**Example:** A `Doctor` and a `Patient` are associated. A doctor "uses" a patient's records, but they are both independent entities.
```python
class Patient:
    def __init__(self, name):
        self.name = name

class Doctor:
    def __init__(self, name):
        self.name = name

    def perform_checkup(self, patient: Patient):
        print(f"Dr. {self.name} is checking up on {patient.name}.")

p = Patient("Alice")
d = Doctor("Smith")
d.perform_checkup(p) # The doctor object "uses" the patient object.
```

---

#### **13. Aggregation?**
**Aggregation** is a specialized form of association. It represents a "has-a" relationship where the child object can exist independently of the parent object. If the parent object is destroyed, the child object is not.

**Example:** A `Department` "has" `Professor`s. If the department is closed down, the professors still exist and can join other departments.

```python
class Professor:
    def __init__(self, name):
        self.name = name

class Department:
    def __init__(self, name, professors: list[Professor]):
        self.name = name
        self.professors = professors # The department "has" a list of professors.

# The professor objects are created independently
prof1 = Professor("Dr. Einstein")
prof2 = Professor("Dr. Curie")

# The department is created, containing references to the professors
science_dept = Department("Physics", [prof1, prof2])

# If we delete the department, the professor objects still exist
del science_dept
print(prof1.name) # Output: Dr. Einstein
```

---

#### **14. Shallow and deep copy?**
This refers to how objects containing other objects (composite objects) are copied. The `copy` module is used for this.

*   **Shallow Copy (`copy.copy()`):** Creates a new top-level object but populates it with *references* to the nested objects from the original. Modifying a nested object in the copy will also affect the original.
*   **Deep Copy (`copy.deepcopy()`):** Creates a new top-level object and then *recursively* creates new copies of all nested objects. The copy is completely independent of the original.

```python
import copy

# A list containing a nested list
original_list = [1, 2, [10, 20]]

# Shallow copy
shallow_copy = copy.copy(original_list)
# Deep copy
deep_copy = copy.deepcopy(original_list)

# Modify the nested list in the shallow copy
shallow_copy[2][0] = 99

print(f"Original after modification:     {original_list}")
print(f"Shallow copy after modification: {shallow_copy}")
print(f"Deep copy (unaffected):          {deep_copy}")
```
**Output:**
```
Original after modification:     [1, 2, [99, 20]]
Shallow copy after modification: [1, 2, [99, 20]]
Deep copy (unaffected):          [1, 2, [10, 20]]
```

---

#### **15. Constructor vs. Destructor? (Real-world examples)**
*   **Constructor:** A special method that is automatically called when an object of a class is created (instantiated). In Python, this is the `__init__` method. Its primary purpose is to initialize the object's attributes.

*   **Destructor:** A special method that is called when an object is about to be destroyed (when its reference count drops to zero and it's garbage collected). In Python, this is the `__del__` method.
    *   **Important:** The use of `__del__` is highly discouraged in Python because the timing of its execution is not guaranteed. For resource management (like closing files or network connections), it's much better to use a **context manager** (the `with` statement).

**Real-World Example (using the preferred context manager approach):**
```python
class FileManager:
    def __init__(self, filename, mode):
        print("Constructor (__init__): Preparing to open file.")
        self.filename = filename
        self.mode = mode
        self.file = None

    # This is part of the context manager protocol (replaces a constructor's role for resource allocation)
    def __enter__(self):
        print("Entering context: Opening file.")
        self.file = open(self.filename, self.mode)
        return self.file

    # This is part of the context manager protocol (replaces the destructor's role for cleanup)
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Exiting context: Closing file.")
        if self.file:
            self.file.close()

# The 'with' statement ensures __enter__ and __exit__ are called automatically.
with FileManager('my_file.txt', 'w') as f:
    f.write('Hello, world!')
    print("Inside context: Working with file.")
# The file is guaranteed to be closed here, even if errors occurred inside the 'with' block.
```

---
You are absolutely right. My apologies for the oversight in the previous response. I had provided a placeholder and intended to complete the full list. Thank you for pointing it out.

Here are the complete answers for all the remaining questions from the detailed OOP list, continuing from where I left off.

***

### **OOP Interview Questions and Answers (Continued)**

This section completes the detailed list of questions from the first image.

---

#### **16. Virtual Functions?**
**Concept:** In languages like C++, a virtual function is a member function in a base class that you expect to be redefined (overridden) in derived classes. When you refer to a derived class object using a pointer or a reference to the base class, you can call the virtual function for that object and have it execute the derived class's version of the function.

**In Python, this behavior is the default.** All methods in Python are inherently "virtual." There is no `virtual` keyword because Python's dynamic nature means it will always look for the method on the object's actual class first (following the Method Resolution Order - MRO). This is a core mechanism that enables polymorphism.

**Example:**
```python
class Animal:
    def speak(self):
        return "Some generic animal sound"

class Dog(Animal):
    # This method implicitly 'overrides' the parent's virtual method
    def speak(self):
        return "Woof!"

def make_animal_speak(animal_object):
    # Python doesn't care about the 'type' of the reference.
    # It calls the method on the actual object.
    print(animal_object.speak())

my_dog = Dog()
generic_animal = Animal()

make_animal_speak(my_dog)         # Output: Woof!
make_animal_speak(generic_animal) # Output: Some generic animal sound
```

---

#### **17. Copy constructor? (Why use it?)**
**Concept:** A copy constructor (a C++ term) is a special constructor used to create a new object as a copy of an existing object.

**Python does not have an explicit copy constructor.** Instead, it provides the `copy` module to achieve the same goal.

*   **Assignment (`=`)** only copies the reference. Both variables will point to the *same* object.
*   **`copy.copy()` (Shallow Copy)** creates a new object, but inserts references to the nested objects of the original.
*   **`copy.deepcopy()` (Deep Copy)** creates a new object and recursively copies all nested objects.

**Why use it?** To create a truly independent duplicate of an object. This prevents unintended side effects where modifying the "copy" also modifies the original.

**Example:**
```python
import copy

class MyObject:
    def __init__(self, items):
        self.items = items

# Original object
obj1 = MyObject([1, 2, 3])

# Assignment (just another reference)
obj2 = obj1

# Shallow copy (the Pythonic way to "copy construct")
obj3 = copy.copy(obj1)

# Modify the list inside the second object
obj2.items.append(99)

print(f"Original object's items: {obj1.items}") # Output: [1, 2, 3, 99] (modified!)
print(f"Copied object's items:  {obj3.items}")  # Output: [1, 2, 3, 99] (also modified!)
# This demonstrates why a deep copy is often needed for nested mutable objects.
```

---

#### **18. Difference between virtual and pure virtual functions?**
**Concept:** This is another C++ distinction that has a parallel in Python.

| Feature               | **Virtual Function (C++)**                  | **Pure Virtual Function (C++)**                                        |
| --------------------- | ------------------------------------------- | ---------------------------------------------------------------------- |
| **Implementation**    | Has an implementation in the base class.    | Has no implementation; declared with `= 0`.                            |
| **Overriding**        | **Can** be overridden by the derived class. | **Must** be overridden by any concrete derived class.                  |
| **Base Class**        | The base class can be instantiated.         | Makes the base class an **Abstract Class**; it cannot be instantiated. |
| **Python Equivalent** | Any standard method in a parent class.      | A method decorated with `@abc.abstractmethod`.                         |

**Python Example:**
```python
import abc

class Shape(abc.ABC):
    def describe(self): # Like a standard 'virtual' function
        return "I am a shape."

    @abc.abstractmethod # Like a 'pure virtual' function
    def area(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side
    
    # Must implement the abstract method
    def area(self):
        return self.side * self.side

s = Square(5)
print(s.describe()) # Output: I am a shape.
print(s.area())     # Output: 25

# shape = Shape() # This would raise a TypeError because it has an abstract method
```

---

#### **19. Dangling Pointer?**
A **dangling pointer** is a pointer that points to a memory location that has been deallocated or freed. Trying to access the data through a dangling pointer leads to undefined behavior, which can cause crashes or subtle bugs. This is a common problem in languages with manual memory management like C/C++.

**Python does not have dangling pointers.** Python uses automatic garbage collection. An object's memory is only deallocated when its reference count drops to zero (i.e., nothing is pointing to it anymore). This system ensures that you can't hold a reference to memory that has already been freed.

---

#### **20. Static keyword and Static function?**
The `static` keyword creates members that belong to the **class** itself, rather than to any specific instance (object) of the class.

In Python, this is achieved with **class attributes** and **static methods**.

*   **Static Variable (Class Attribute):** A variable shared among all instances of a class.
*   **Static Method (`@staticmethod`):** A method that belongs to the class but does not receive the instance (`self`) or the class (`cls`) as its first argument. It's essentially a regular function namespaced within the class, often used for utility functions that relate to the class but don't depend on instance state.

**Example:**
```python
class Car:
    # This is a class attribute (static variable)
    number_of_wheels = 4

    def __init__(self, make):
        self.make = make # This is an instance attribute

    @staticmethod
    def is_motor_vehicle(): # This is a static method
        return True

# Accessing class attribute and static method without creating an instance
print(f"All cars have {Car.number_of_wheels} wheels.")
print(f"Is a car a motor vehicle? {Car.is_motor_vehicle()}")
```

---

#### **21. Difference between private and protected?**
In Python, access control is by **convention**, not strict enforcement.

*   **Protected (`_`):** A single leading underscore is a convention to tell other developers: "This attribute or method is intended for internal use by this class and its subclasses. You can access it, but you probably shouldn't."
*   **Private (`__`):** A double leading underscore triggers **name mangling**. Python renames the attribute internally to `_ClassName__attributeName`. This makes it much harder to access from outside the class and is primarily used to avoid naming conflicts in subclasses, not for security.

**Example:**
```python
class MyClass:
    def __init__(self):
        self.public_var = "I'm public"
        self._protected_var = "I'm protected"
        self.__private_var = "I'm private"

obj = MyClass()
print(obj.public_var)
print(obj._protected_var) # Accessible, but discouraged

# print(obj.__private_var) # This will raise an AttributeError
# To access it, you must use the mangled name:
print(obj._MyClass__private_var)
```

---

#### **22. Is a vs has a relationship?**
*   **Is-A Relationship:** Represents **Inheritance**. The subclass *is a* specialized type of the superclass.
    *   **Example:** A `Dog` **is an** `Animal`.
    *   **Implementation:** `class Dog(Animal):`

*   **Has-A Relationship:** Represents **Composition** or **Aggregation**. An object *has an* instance of another object as one of its attributes.
    *   **Example:** A `Car` **has an** `Engine`.
    *   **Implementation:** `self.engine = Engine()` inside the `Car`'s `__init__` method.

---

#### **23. Association / Composition / Aggregation?**
These are three types of relationships between classes, all falling under the general idea of "has-a" but with different lifecycle dependencies.

*   **Association:** The most general relationship. It means two objects are connected or "use" each other, but have independent lifecycles.
    *   **Example:** A `Doctor` and a `Patient`. They are associated, but one can exist without the other.

*   **Aggregation:** A more specific "has-a" relationship where the child object can exist independently of the parent object. It represents a whole-part relationship where the part can be separated from the whole.
    *   **Example:** A `Department` has `Professor`s. If the department is closed, the professors (objects) still exist.

*   **Composition:** A strong "has-a" relationship where the child object **cannot** exist independently of the parent. The part is an integral component of the whole, and its lifecycle is tied to the parent's.
    *   **Example:** A `House` has `Room`s. If you destroy the house, the rooms are destroyed with it. This is typically modeled by creating the child object inside the parent's constructor.

---
#### **Remaining Questions (Consolidated and Answered)**

*   **Final keyword/methods/classes?**
    *   In Java/C#, `final` prevents a method from being overridden or a class from being inherited. Python has no direct `final` keyword. To prevent inheritance, you can use an advanced metaclass technique (as shown in a previous answer).

*   **Can we access non-static data members in static function?**
    *   No. A static method belongs to the class, not an instance. It does not receive `self` (the instance), so it has no way to access instance-specific data members.

*   **Memory leakage vs dangling pointer?**
    *   **Memory Leak:** Occurs when a program allocates memory but loses all references to it, making it impossible to free that memory. The program "forgets" about the memory, which remains allocated but unused. In Python, this can happen with reference cycles that the garbage collector can't break.
    *   **Dangling Pointer:** A reference to memory that has already been freed.
    *   **Key Difference:** A memory leak is *un-freeable* memory; a dangling pointer is a reference to *already-freed* memory. Python's GC prevents dangling pointers but can still suffer from memory leaks.

*   **Coupling and Cohesion?**
    *   **Coupling:** The degree of dependency between different modules/classes. **Low coupling** is good (modules are independent).
    *   **Cohesion:** The degree to which elements inside a single module/class work together for a single purpose. **High cohesion** is good (class has a clear, focused responsibility).
    *   **Goal:** Design for **Low Coupling** and **High Cohesion**.

*   **OOP vs functional programming?**
    *   **OOP:** Organizes code around **objects** (data and behavior bundled together). It uses concepts like encapsulation, inheritance, and polymorphism. State is typically encapsulated within objects.
    *   **Functional Programming (FP):** Organizes code around **pure functions** (functions that have no side effects and always produce the same output for the same input). It emphasizes immutability and avoids shared state.

*   **What are the minimum requirements for a class to be abstract?**
    *   A class becomes abstract if it inherits from `abc.ABC` and contains at least one method decorated with `@abc.abstractmethod`.

*   **What cohesion does?**
    *   High cohesion leads to code that is more understandable, reusable, and maintainable. A class with high cohesion does one thing well, making its purpose clear and its logic self-contained.

*   **If a child inherits from two classes and parent classes have same name function, how to tackle that?**
    *   This is the **Diamond Problem**. Python solves it using the **Method Resolution Order (MRO)**. Python follows a specific, predictable search path (e.g., Child -> Parent1 -> Parent2 -> Grandparent) and calls the first implementation of the function it finds. You can inspect this path with `ClassName.mro()`.

*   **Func A(int a, float b) vs Func A(float a, int b)? What if called with Func A(1,1)? (Ambiguity Error)**
    *   This question is specific to statically-typed languages like C++ that support function overloading. In C++, calling `A(1, 1)` would be ambiguous because the compiler wouldn't know whether to promote the second `1` to a float or the first `1` to a float. It would result in a compile-time error.
    *   **In Python, this is not an issue.** Python is dynamically typed and does not support traditional function overloading. The last function defined with the name `A` would simply overwrite the previous one.

*   **Purpose of `virtual = 0`?**
    *   In C++, setting a virtual function to `= 0` makes it a **pure virtual function**, which in turn makes the class an **abstract class**. This forces any concrete subclass to provide an implementation for that function.

*   **Why use & in copy constructor parameters list?**
    *   In C++, the parameter for a copy constructor is passed by **reference (`&`)** to avoid an infinite loop. If it were passed by value, the compiler would need to make a copy of the argument, which would require calling the copy constructor, which would require making a copy, and so on.

*   **Virtual table in OOP?**
    *   A virtual table (or vtable) is a mechanism used in languages like C++ to support dynamic dispatch (i.e., to make virtual functions work). It's a lookup table of function pointers, and each class with virtual functions has its own vtable. When you call a virtual function on an object, the program looks up the correct function address in that object's vtable at runtime. **Python uses a different mechanism (dictionary lookups via MRO) to achieve the same result.**

*   **Upcasting and downcasting?**
    *   **Upcasting:** Treating a derived class object as an instance of its base class. This is always safe and happens implicitly in Python. (e.g., `animal = Dog()`).
    *   **Downcasting:** Treating a base class object as an instance of one of its derived classes. This is generally unsafe because the object may not actually be an instance of that specific derived class. In Python, you would typically check the type first using `isinstance()` before proceeding.

*   **Constructor chaining?**
    *   The process of a constructor calling another constructor. In inheritance, it means a child class constructor calls the parent class constructor using `super().__init__(...)` to ensure the parent part of the object is properly initialized.

#### **1. What to write in class so that it can't be inherited? (e.g., final in Java, sealed in C#)**
Python does not have a built-in `final` or `sealed` keyword to prevent class inheritance. This is by design, following Python's philosophy of "consenting adults."

However, it is possible to achieve this effect using an advanced technique called **metaclasses**. A metaclass can intercept the creation of a subclass and raise an error. This is not common practice and is generally considered un-pythonic.

**Example using a metaclass:**
```python
# A metaclass that forbids any class from inheriting from a class that uses it.
class FinalMeta(type):
    def __new__(cls, name, bases, classdict):
        for base in bases:
            if isinstance(base, FinalMeta):
                raise TypeError(f"Class '{base.__name__}' is final and cannot be subclassed.")
        return type.__new__(cls, name, bases, classdict)

# Now, apply this metaclass to our class.
class SealedClass(metaclass=FinalMeta):
    def greet(self):
        return "You can use me, but not extend me."

# This will work fine:
s = SealedClass()

# This will raise a TypeError:
try:
    class SubClass(SealedClass):
        pass
except TypeError as e:
    print(e) # Output: Class 'SealedClass' is final and cannot be subclassed.
```

---

#### **2. How to avoid implementation of an abstract function?**
You cannot avoid implementing an abstract function in a **concrete class**. That is the entire purpose of an abstract method: it defines a contract that any non-abstract subclass *must* fulfill.

If a subclass inherits from an abstract class but fails to provide an implementation for one or more of its abstract methods, then that **subclass itself becomes an abstract class**, and you will not be able to create an instance of it.

**Example:**
```python
import abc

class Base(abc.ABC):
    @abc.abstractmethod
    def do_something(self):
        pass

# This class does NOT implement do_something()
class Derived(Base):
    def greet(self):
        print("Hello")

# Trying to instantiate Derived will fail.
try:
    d = Derived()
except TypeError as e:
    # Output: Can't instantiate abstract class Derived with abstract method do_something
    print(e)
```
So, the only way to "avoid" implementation is to not create a concrete (instantiable) class.

---

#### **3. Null pointer?**
The concept of a "null pointer" comes from languages like C/C++, where it's a pointer that doesn't point to any valid memory address.

Python does not have pointers in the same way, so it does not have null pointers. The Python equivalent is the special singleton object **`None`**.

*   `None` is a built-in constant that represents the absence of a value or a null state.
*   It is an object of its own type, `NoneType`.
*   There is only one instance of `None` in a Python program, so the standard way to check for it is using the identity operator `is`.

```python
my_variable = None

if my_variable is None:
    print("The variable has no value assigned.")
else:
    print(f"The variable has the value: {my_variable}")
```

---

#### **4. .obj vs .exe?**
This question refers to the compilation and linking process in languages like C or C++.

*   **.obj (Object File):** This is the output of the compiler after it processes a single source code file (e.g., a `.cpp` file). It contains machine code (binary code) but is not yet a complete, runnable program. It may contain unresolved references to functions or variables defined in other source files.
*   **.exe (Executable File):** This is the final, runnable program created by a **linker**. The linker takes one or more object files (`.obj`) and combines them, resolves the cross-references between them, and links them with necessary system libraries to produce a standalone executable file that the operating system can run.

**Analogy in Python:**
Python is an interpreted language, so the process is different.
*   When you run `my_script.py`, the Python interpreter first compiles it into **bytecode**. This bytecode is often saved in a `.pyc` file inside a `__pycache__` directory. The `.pyc` file is analogous to an **.obj** file; it's an intermediate format that's not human-readable.
*   The **Python interpreter** itself (e.g., `python.exe`) then executes this bytecode. There is no separate `.exe` file created from your script by default. The interpreter is the executable that runs your code.

---

#### **5. Features of Java?**
This question is about Java, but here are its key features, often contrasted with Python:

*   **Statically Typed:** Variable types must be explicitly declared and are checked at compile time (unlike Python's dynamic typing).
*   **Compiled:** Java code is compiled to bytecode, which is then run on the Java Virtual Machine (JVM).
*   **Platform Independent ("Write Once, Run Anywhere"):** The JVM allows Java bytecode to run on any device that has a JVM, regardless of the underlying operating system.
*   **Object-Oriented:** Java is a purely object-oriented language where almost everything is an object.
*   **Explicit Interfaces:** Uses the `interface` keyword to define contracts.
*   **Supports Traditional Overloading:** Allows multiple methods with the same name but different parameter lists.
*   **Robust Memory Management:** Has automatic garbage collection, similar to Python.
*   **Strong Community and Ecosystem:** Has a vast collection of libraries and frameworks for enterprise-level applications.

---

#### **6. What method makes classes show different behaviour in polymorphism?**
The core technique that enables polymorphic behavior is **Method Overriding**.

When a child class provides its own specific implementation for a method that is already defined in its parent class, it is "overriding" the parent's behavior. At runtime, when that method is called on an object, Python's dynamic dispatch mechanism ensures that the version of the method in the object's *actual* class is executed, not the one in the parent class.

**Example:**
```python
class Bird:
    def fly(self):
        print("This bird is flying.")

class Penguin(Bird):
    # Penguin overrides the fly method to provide its own behavior
    def fly(self):
        print("This penguin cannot fly, but it can swim!")

def make_bird_fly(bird: Bird):
    bird.fly() # The same method call

b = Bird()
p = Penguin()

make_bird_fly(b) # Output: This bird is flying.
make_bird_fly(p) # Output: This penguin cannot fly, but it can swim!
```
The `fly()` method call results in different behavior because the `Penguin` class **overrode** it.

---

#### **7. Accessors, Mutators, Add Operator (Python __add__)?**
These are three distinct but related concepts in class design.

*   **Accessor (Getter):** A method used to get the value of an attribute, especially one that is considered private or protected. The "Pythonic" way to create a getter is with the `@property` decorator.
*   **Mutator (Setter):** A method used to set or change the value of an attribute. This is useful for adding validation logic before the change is made. The "Pythonic" way is with the `@<property_name>.setter` decorator.
*   **Add Operator (`__add__`):** This is an example of **operator overloading**. By implementing the `__add__` special method in your class, you can define what the `+` operator does when applied to objects of your class.

**Example demonstrating all three:**
```python
class Money:
    def __init__(self, amount):
        self._amount = amount # A "protected" attribute

    # Accessor (Getter) for 'amount'
    @property
    def amount(self):
        print("Getting amount...")
        return self._amount

    # Mutator (Setter) for 'amount'
    @amount.setter
    def amount(self, value):
        print("Setting amount...")
        if value < 0:
            raise ValueError("Amount cannot be negative.")
        self._amount = value

    # Overloading the '+' operator
    def __add__(self, other):
        if isinstance(other, Money):
            return Money(self.amount + other.amount)
        return NotImplemented

    def __repr__(self):
        return f"Money(${self.amount})"

wallet1 = Money(50)
wallet2 = Money(100)

# Using the + operator, which calls __add__
total = wallet1 + wallet2
print(f"Total: {total}") # Output: Total: Money($150)

# Using the setter and getter
total.amount = 200 # Calls the setter
print(f"New total: {total.amount}") # Calls the getter
```

---

#### **8. How we limit creating not more than 10 objects?**
You can limit the number of instances a class can have by overriding the `__new__` special method. `__new__` is the first step in object creation (it happens before `__init__`) and is responsible for actually creating the object. By adding a class-level counter, you can check the count and raise an exception if the limit is exceeded.

```python
class LimitedInstances:
    _instance_count = 0
    _MAX_INSTANCES = 10

    def __new__(cls, *args, **kwargs):
        if cls._instance_count >= cls._MAX_INSTANCES:
            raise TypeError(f"Cannot create more than {cls._MAX_INSTANCES} instances of {cls.__name__}")
        
        # Increment the counter BEFORE creating the object
        cls._instance_count += 1
        
        # Call the parent's __new__ to actually create the instance
        print(f"Creating instance number {cls._instance_count}...")
        return super().__new__(cls)

    def __init__(self, id):
        self.id = id
        
    # Optional: Decrement the counter when an object is deleted
    def __del__(self):
        LimitedInstances._instance_count -= 1
        print(f"Instance destroyed. {LimitedInstances._instance_count} instances remaining.")


# Create 10 instances successfully
instances = []
for i in range(10):
    instances.append(LimitedInstances(i))

# Now, try to create an 11th instance
try:
    extra_instance = LimitedInstances(10)
except TypeError as e:
    print(f"\nError: {e}")
```
**Output:**
```
Creating instance number 1...
Creating instance number 2...
...
Creating instance number 10...

Error: Cannot create more than 10 instances of LimitedInstances
```
---

#### **9. How is encapsulation achieved?**
Encapsulation is achieved in Python through the creation of **classes**. A class acts as a container or "capsule" that bundles two things together:
1.  **Data (Attributes):** Variables that hold the state of an object.
2.  **Methods:** Functions that define the behavior of the object and operate on its data.

A crucial part of encapsulation is **data hiding**, which is the practice of restricting direct access to an object's internal data. In Python, this is achieved by a **naming convention** rather than strict keywords:

*   **Public (`name`):** Accessible from anywhere. This is the default.
*   **Protected (`_name`):** A single leading underscore indicates that an attribute is for internal use by the class or its subclasses. This is a convention only; access is not blocked.
*   **Private (`__name`):** A double leading underscore triggers "name mangling," where Python renames the attribute to `_ClassName__name`. This makes it difficult (but not impossible) to access from outside the class and is primarily used to prevent naming conflicts in subclasses.

By marking data as protected or private and providing public methods (like getters and setters) to interact with that data, you achieve encapsulation. This protects the object's internal state from being corrupted by accidental or improper external modifications.