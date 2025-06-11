# Intent
**Adapter** is a structural design pattern that allows objects with incompatible interfaces to collaborate.
# Problems
When legacy system need to use with newer system. We have to choices:
1. Add additional code inside new system => but what if old system is a mess. This could lead to bad code understanding
2. Using adapter

# Solution
Using adapter with target interface that require adapter to do a same thing (same method or just extend them). They must have 
```java
// target interface  
static class Printer {  
    void print() {  
        System.out.println("printing");  
    }  
}  
  
// adaptee  
static class LegacyPrinter {  
    void printDocument() {  
        System.out.println("Legacy print document");  
    }  
}  
  
// adapter  
static class PrinterAdapter extends Printer {  
    LegacyPrinter legacyPrinter = new LegacyPrinter();  
  
    @Override  
    void print() {  
        legacyPrinter.printDocument();  
    }  
}  
  
public static void main(String[] args) {  
    var adapter = new PrinterAdapter();  
    clientProcessPrint(adapter);  
}  
  
// client code  
static void clientProcessPrint(Printer printer) {  
    printer.print();  
}
```

Question in here is why we need to do this.
- Because when working in big system we high-level modules should not depend on low-level modules. Both should depend on abstraction
- Adapter in here just like a middle-ware to work like a Printer and we can easily change to another adapter and maybe another Printer and all of code will work the same
