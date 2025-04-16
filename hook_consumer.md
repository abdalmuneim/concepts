# Hook Consumer / Hup Consumer

Hook Consumer is a concept in Flutter that combines hooks and state management.

## Key Points:
- Hooks simplify state management.
- Consumers listen to state changes and rebuild widgets.

### Example:
```dart
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';

final counterProvider = StateProvider<int>((ref) => 0);

class HookConsumerExample extends HookWidget {
  @override
  Widget build(BuildContext context) {
    final counter = useProvider(counterProvider);

    return Scaffold(
      appBar: AppBar(title: Text('Hook Consumer Example')),
      body: Center(
        child: Text('Count: ${counter.state}'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => counter.state++,
        child: Icon(Icons.add),
      ),
    );
  }
}
```