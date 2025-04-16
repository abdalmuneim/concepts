# State Trees

State trees represent the hierarchical structure of state in an application.

## Key Points:
- State trees help manage complex state dependencies.
- Flutter uses widgets as the building blocks of the state tree.

### Example:
```dart
class AppState {
  final int counter;
  final bool isLoading;

  AppState({required this.counter, required this.isLoading});
}
```