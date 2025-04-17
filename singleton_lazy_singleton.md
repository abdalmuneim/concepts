## Singleton
### Definition
A Singleton is a design pattern that ensures a class has only one instance and provides a global point of access to it.

### Advantages
- Ensures a single instance is shared across the application.
- Reduces memory usage by avoiding multiple object creation.
- Useful for managing shared resources like configuration, logging, or database connections.

### Disadvantages
- Can introduce global state, making testing and debugging harder.
- Violates the Single Responsibility Principle if misused.
- Difficult to implement in multithreaded environments without proper synchronization.

### Memory Location
- Stored in the heap memory, but only one instance exists for the lifetime of the application.

### When to Use
- When a single instance of a class is required throughout the application.
- For managing shared resources or services.

### Example
Here is an example of a Singleton implementation in Dart:

```dart
class Singleton {
  // Private constructor
  Singleton._privateConstructor();

  // Static instance of the class
  static final Singleton _instance = Singleton._privateConstructor();

  // Public factory method to provide access to the instance
  factory Singleton() {
    return _instance;
  }

  // Example method
  void showMessage() {
    print("Singleton instance accessed!");
  }
}

void main() {
  // Accessing the Singleton instance
  var singleton1 = Singleton();
  var singleton2 = Singleton();

  // Both instances are the same
  print(singleton1 == singleton2); // Output: true

  singleton1.showMessage(); // Output: Singleton instance accessed!
}
```

### Lazy Singleton
A lazy Singleton delays the creation of its instance until it is first accessed. This can improve performance and resource usage if the instance is not always needed.

#### Example
Here is an example of a lazy Singleton implementation in Dart:

```dart
class LazySingleton {
  // Private static variable to hold the instance
  static LazySingleton? _instance;

  // Private constructor
  LazySingleton._privateConstructor();

  // Public factory method to provide access to the instance
  factory LazySingleton() {
    if (_instance == null) {
      _instance = LazySingleton._privateConstructor();
    }
    return _instance!;
  }

  // Example method
  void showMessage() {
    print("Lazy Singleton instance accessed!");
  }
}

void main() {
  // Accessing the Lazy Singleton instance
  var lazySingleton1 = LazySingleton();
  var lazySingleton2 = LazySingleton();

  // Both instances are the same
  print(lazySingleton1 == lazySingleton2); // Output: true

  lazySingleton1.showMessage(); // Output: Lazy Singleton instance accessed!
}
```
