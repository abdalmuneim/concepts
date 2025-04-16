# Abstract Class and Sealed Class

Abstract classes and sealed classes are used to define a blueprint for other classes and restrict inheritance, respectively.

## Key Points:
- Abstract classes cannot be instantiated and can have abstract methods "Defines methods but doesn’t implement them".
- Sealed classes restrict which classes can extend them "You define all allowed subtypes — good for event/state classes".

### Example:
```dart
abstract class Animal {
  void makeSound();
}

class Dog extends Animal {
  @override
  void makeSound() => print('Bark');
}

sealed class Shape permits Circle, Square {}

class Circle extends Shape {}
class Square extends Shape {}
```