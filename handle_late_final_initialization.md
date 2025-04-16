# How to Handle Late Final Initialization

Late final variables in Dart are initialized lazily and can only be set once.

## Key Points:
- Use `late` for variables that are initialized after declaration.
- Ensure initialization before usage to avoid runtime errors.
- Use constructors or null checks if unsure.

### Example:
```dart
late final String name;

void main() {
  name = 'Alice';
  print(name);
}
```