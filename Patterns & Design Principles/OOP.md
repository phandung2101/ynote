# Abstraction
## Props
- display only essential details to user
- trivial or non essential details are not display to user
- can achieved by *interface* and *abstract classes*
## Example
A man driving a car only know that when press accelarator will increase speed of car and use brake to decrease speed of the car or stop the car. But inside `Car` object `accelerators` method can handle very complex task but user do not need understand implementation
```java
// Abstract class representing a Vehicle (hiding implementation details)
abstract class Vehicle {
    // Abstract methods (what it can do)
    abstract void accelerate();
    abstract void brake();
    
    // Concrete method (common to all vehicles)
    void startEngine() {
        System.out.println("Engine started!");
    }
} 

// Concrete implementation (hidden details)
class Car extends Vehicle {
    @Override
    void accelerate() {
        System.out.println("Car: Pressing gas pedal...");
        // Hidden complex logic: fuel injection, gear shifting, etc.
    }
    
    @Override
    void brake() {
        System.out.println("Car: Applying brakes...");
        // Hidden logic: hydraulic pressure, brake pads, etc.
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myCar = new Car();
        myCar.startEngine();  
        myCar.accelerate();   
        myCar.brake();        
    }
}
```
> Abstraction is a way we work with program everyday. Always use abstraction for high maintainance code.
# Encapsulation
## Props
- wrapping up of data under a single unit
- similar to **data-hiding**, the data is hide from another classes
- encapsulation can be achived by using access modifer in variables and methods (most encapsulation using **private**)
## Example
Using *getter* and *setter* to control how data change and retrieval
```java
// Encapsulation using private modifier

class Employee {
    // Private fields (encapsulated data)
    private int id;
    private String name;

    // Setter methods 
    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    // Getter methods
    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee();
        
        // Using setters
        emp.setId(101);
        emp.setName("Geek");

        // Using getters
        System.out.println("Employee ID: " + emp.getId());
        System.out.println("Employee Name: " + emp.getName());
    }
}
```
> In clean code, using private to hide data from another class and its childs from dirrect access to value. Like parent have some logic that don't want any change can override and change.
# Inheritance
## Props
- it is mechanism which one class allowed to inherit the features (data and method) from another class. Achieve by using **extend** keyword as "**is-a**" relationship.
- superclass, a class whose features are inherited.
- subclass, a class that extend superclass (also known as derived or extended or child class)
- Inheritance supports the concept of **reusability**, i.e. when we want to create a new class and there is already class that includes some of code that we want, we can derive our new class from the existing class.
## Example
```java
// Superclass (Parent)
class Animal {
    void eat() {
        System.out.println("Animal is eating...");
    }

    void sleep() {
        System.out.println("Animal is sleeping...");
    }
}

// Subclass (Child) - Inherits from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();

        // Inherited methods (from Animal)
        myDog.eat();    
        myDog.sleep();  

        // Child class method
        myDog.bark();   
    }
}
```
# Polymorphism
## Props
- **differentiate between entities of the same name efficiently**.
- Method overloading is known as **compile-time polymorphism** because methods with the same name but different parameters (number, type, or order) are resolved by the Java compiler at compile-time. The compiler uses static binding to determine the exact method to call based on the method signature and embeds this reference in the bytecode. At runtime, the JVM uses the resolved reference (finalized during the linking phase, specifically resolution) to locate the method’s bytecode in the method area and execute it. (Find more detail at [[How JVM works#Linking]])
- method overriding is **runtime polymorphism** because method have same name could lead to different result -> decide which one is used at the runtime. This decide which one to be use by **dynamic method dispatch** process.
## Sample
### Method overriding
```java
class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // Upcasting
        animal.sound(); // Output: Bark
    }
}
```
### Method overloading
```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(1, 2));       // Gọi add(int, int)
        System.out.println(calc.add(1.5, 2.5));   // Gọi add(double, double)
        System.out.println(calc.add(1, 2, 3));    // Gọi add(int, int, int)
    }
}
```
