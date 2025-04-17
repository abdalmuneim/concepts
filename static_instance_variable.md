// This file explains the concept of Static and Instance Variables in Dart.

## Static
- Variables are shared across all instances of a class.
- Belongs to the class itself.
- **Advantages**:
  - Efficient memory usage for shared data.
  - Useful for constants, counters, or configurations shared across instances.
- **Disadvantages**:
  - Cannot access instance-specific data.
  - Can lead to unexpected behavior if modified globally.
- **Memory**: Allocated in the class's memory space at runtime.
- **Execution**: Initialized when the class is loaded into memory.

## Instance
- Variables are unique to each instance of a class.
- Belongs to an object created from a class.
- **Advantages**:
  - Encapsulates data specific to an object.
  - Promotes object-oriented design principles.
- **Disadvantages**:
  - Higher memory usage for multiple instances.
  - Cannot be shared across instances.
- **Memory**: Allocated in the heap memory when an object is created.
- **Execution**: Initialized when the object is instantiated.

```
class Example {
  static int staticVariable = 0;
  int instanceVariable = 0;

  void increment() {
    staticVariable++;
    instanceVariable++;
  }
}

void main() {
  Example example1 = Example();
  Example example2 = Example();

  example1.increment();
  print('Static Variable: ${Example.staticVariable}'); // Output: 1
  print('Instance Variable (example1): ${example1.instanceVariable}'); // Output: 1

  example2.increment();
  print('Static Variable: ${Example.staticVariable}'); // Output: 2
  print('Instance Variable (example2): ${example2.instanceVariable}'); // Output: 1
}
```