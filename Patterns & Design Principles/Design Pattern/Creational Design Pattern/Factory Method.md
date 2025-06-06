# Intention
**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
# Scenario
Example:
```java
/*package whatever //do not write package name here */

import java.io.*;

// Library classes
abstract class Vehicle {
    public abstract void printVehicle();
}

class TwoWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am two wheeler");
    }
}

class FourWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am four wheeler");
    }
}

// Client (or user) class
class Client {
    private Vehicle pVehicle;

    public Client(int type) {
        if (type == 1) {
            pVehicle = new TwoWheeler();
        } else if (type == 2) {
            pVehicle = new FourWheeler();
        } else {
            pVehicle = null;
        }
    }

    public void cleanup() {
        if (pVehicle != null) {
            pVehicle = null;
        }
    }

    public Vehicle getVehicle() {
        return pVehicle;
    }
}

// Driver program
public class GFG {
    public static void main(String[] args) {
        Client pClient = new Client(1);
        Vehicle pVehicle = pClient.getVehicle();
        if (pVehicle != null) {
            pVehicle.printVehicle();
        }
        pClient.cleanup();
    }
}
```
Issue with above code:
- The `Client` create object directly from input. This strong dependency make when upgrade or maintain can be difficult 
- The `Client` class not only create object but also handle its lifecycle. Mixed responsibility => break **single responsibility principle**
- When add new type we need modify object => break **open/close principle** that should be open for extension but not for modification
# Solution
```java
// Library classes
abstract class Vehicle {
    public abstract void printVehicle();
}

class TwoWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am two wheeler");
    }
}

class FourWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am four wheeler");
    }
}

// Factory Interface
interface VehicleFactory {
    Vehicle createVehicle();
}

// Concrete Factory for TwoWheeler
class TwoWheelerFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new TwoWheeler();
    }
}

// Concrete Factory for FourWheeler
class FourWheelerFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new FourWheeler();
    }
}

// Client class
class Client {
    private Vehicle pVehicle;

    public Client(VehicleFactory factory) {
        pVehicle = factory.createVehicle();
    }

    public Vehicle getVehicle() {
        return pVehicle;
    }
}

// Driver program
public class GFG {
    public static void main(String[] args) {
        VehicleFactory twoWheelerFactory = new TwoWheelerFactory();
        Client twoWheelerClient = new Client(twoWheelerFactory);
        Vehicle twoWheeler = twoWheelerClient.getVehicle();
        twoWheeler.printVehicle();

        VehicleFactory fourWheelerFactory = new FourWheelerFactory();
        Client fourWheelerClient = new Client(fourWheelerFactory);
        Vehicle fourWheeler = fourWheelerClient.getVehicle();
        fourWheeler.printVehicle();
    }
}
```
# Let break it down:
- can see that now when new Vehicle added to code. Just create new one and you don't need to modify `Client` anymore ** .
- but nothing is **best tool*** . as you can see the new code will growth and could be lead too very complex code and hard to understanding at begin 

# Just for know how it could be
```java
class OneWheeler extends Vehicle {
    public void printVehicle() {
        System.out.println("I am one wheeler");
    }
}
class OneWheelerFactory implements VehicleFactory {
    public Vehicle createVehicle() {
        return new OneWheeler();
    }
}
// Driver program
public class GFG {
    public static void main(String[] args) {
        VehicleFactory twoWheelerFactory = new TwoWheelerFactory();
        Client twoWheelerClient = new Client(twoWheelerFactory);
        Vehicle twoWheeler = twoWheelerClient.getVehicle();
        twoWheeler.printVehicle();

        VehicleFactory fourWheelerFactory = new FourWheelerFactory();
        Client fourWheelerClient = new Client(fourWheelerFactory);
        Vehicle fourWheeler = fourWheelerClient.getVehicle();
        fourWheeler.printVehicle();

		// now we can just use new Vehicle
		VehicleFactory oneWheelerFactory = new OneWheelerFactory();
        Client oneWheelerClient = new Client(oneWheelerFactory);
        Vehicle oneWheeler = oneWheelerClient.getVehicle();
        oneWheeler.printVehicle();
    }
}
```
Feelin like more code than this right. But when you modify Client class. We don't know it actually have side effect or not. So above code we be better that we know only new `Vehicle` added not `Client` code modified (because we know that `Client` will run with every `Vehicle`)
```java
public Client(int type) {
	if (type == 1) {
		pVehicle = new TwoWheeler();
	} else if (type == 2) {
		pVehicle = new FourWheeler();
	} else if (type == 3) {
		pVehicle = new OneWheeler();
	} else {
		pVehicle = null;
	}
}

```