# Record Pattern Matching

Record pattern matching is a feature that allows you to destructure and match records in Dart.

## Key Points:
- Records are immutable data structures introduced in Dart 3.0.
- Pattern matching simplifies working with records.

### Example:
```dart
void main() {
  var record = (name: 'Alice', age: 30);

  switch (record) {
    case (name: 'Alice', age: int age):
      print('Alice is $age years old');
      break;
    default:
      print('Unknown record');
  }
}

/// anther example
(String, int) person = ('Ahmed', 25);

var (name, age) = person;
print('Name: $name, Age: $age');

```