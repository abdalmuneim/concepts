// This file explains the concepts of Future, Stream, and Isolate in Flutter.

## Future
- represents a value that will be available at some point in the future.
- Runs one-time async operations like HTTP requests.

## Stream
- is a sequence of asynchronous events.
- Provides continuous data, like WebSocket or database changes.

## Isolate
- is a way to run Dart code in a separate thread.
- Dart’s way to do multithreading — use for CPU-heavy work like parsing JSON.

// Example of Future:
```
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Data fetched';
}
```

// Example of Stream:
```
Stream<int> numberStream() async* {
  for (int i = 0; i < 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}
```

// Example of Isolate:
```
import 'dart:isolate';

void isolateFunction(SendPort sendPort) {
  sendPort.send('Message from isolate');
}
```

```
void main() async {
  // Future example
  print(await fetchData());

  // Stream example
  await for (var number in numberStream()) {
    print(number);
  }

  // Isolate example
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(isolateFunction, receivePort.sendPort);
  print(await receivePort.first);
}
```