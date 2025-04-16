# Pattern Matching and Switching

Pattern matching and switching are powerful features in programming that allow you to match values against patterns and execute code based on the match.

## Key Points:
- Dart introduced pattern matching in version 3.0.
- It simplifies complex conditional logic.

### Example:
```dart
void main() {
  var value = 42;

  switch (value) {
    case int n when n > 40:
      print('Greater than 40');
      break;
    case int n when n < 40:
      print('Less than 40');
      break;
    default:
      print('Exactly 40');
  }
}
```