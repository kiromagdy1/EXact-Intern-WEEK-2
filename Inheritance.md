# IS-A Relationship (Inheritance)

- **Definition**: The "IS-A" relationship is defined through **inheritance** using the `extends` keyword. It represents a hierarchical classification where a subclass is a specific type of a superclass.
- **Example**: An `Apple` **is a** `Fruit`. So, `Apple` inherits from `Fruit`.
- **Key Characteristic**: It promotes code reusability and **polymorphism** (a subclass object can be used wherever a superclass object is expected).

## Code Example (IS-A)

```java
class Fruit { }
class Apple extends Fruit { }
HAS-A Relationship (Composition)
Definition: The "HAS-A" relationship is implemented through composition, where a class contains a reference to an object of another class as an instance variable.

Example: A Car has an Engine. So, Car has a field of type Engine.

Key Characteristic: This is often favored over inheritance for code reuse because it provides greater flexibility by not breaking encapsulation or binding classes too tightly.

Code Example (HAS-A)
java
class Engine { }
class Car {
    private Engine engine; // Car HAS-A Engine
}
Explanation of Parent p = new Child();
In Java, the syntax Parent parent = new Child(); (where Child is a subclass of Parent) is a fundamental concept of polymorphism.

When this statement executes:

A new Child object is created in memory.

The reference variable parent (which is of type Parent) is used to point to that Child object.

Key Behaviors
Method Calls (Dynamic Method Dispatch)
When you call a method on parent, Java decides which version to run based on the actual object type (Child), not the reference type (Parent). If Child has overridden a method, the child's version executes.

java
class Parent {
    void display() { System.out.println("Parent"); }
}
class Child extends Parent {
    @Override
    void display() { System.out.println("Child"); }
}

public static void main(String[] args) {
    Parent p = new Child();
    p.display(); // Outputs: Child
}
Member Variables (No Polymorphism)
Variables are not polymorphic. If both Parent and Child have a variable with the same name, accessing it through the Parent reference will return the value from the Parent class.

Access Limitation
The parent reference can only directly call methods declared in the Parent class. Any new methods defined only in Child cannot be called without an explicit cast.