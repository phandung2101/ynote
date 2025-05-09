# Intention
**Prototype** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.
# Problems
How you can copy an object => just iterate all field and copy them to new object. But sometime you cannot access all fields of an object like private field. 
# Solution
Prototype is delegates the cloning process to the actual objects that are being cloned.
```java
abstract class Shape {  
    private int X;  
    private int Y;  
  
    private String color;  
  
    public Shape(Shape source) {  
        this.X = source.X;  
        this.Y = source.Y;  
        this.color = source.color;  
    }  
}  
  
class Rectangle extends Shape {  
    private int width;  
    private int height;  
  
    public Rectangle(Rectangle source) {  
        super(source);  
        this.width = source.width;  
        this.height = source.height;  
    }  
}  
  
class Circle extends Shape {  
    private int radius;  
  
    public Circle(Circle source) {  
        super(source);  
        this.radius = source.radius;  
    }  
}
```
Above code how Prototype work

# Summary
| When use                          | Why                      |
| --------------------------------- | ------------------------ |
| Create complex object & expensive | Clone is quicker         |
| Need multiple clone instance      | Easy to clone and modify |
| Create at runtime                 | Easy to use              |

