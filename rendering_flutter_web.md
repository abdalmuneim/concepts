# Rendering Flutter Web

Rendering in Flutter Web involves converting widgets into HTML, CSS, and JavaScript.

## Key Points:
- Flutter Web uses a rendering engine to draw widgets.
- It supports both CanvasKit and HTML renderers.

### Example:
```dart
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Flutter Web Rendering')),
        body: Center(child: Text('Hello, Flutter Web!')),
      ),
    );
  }
}
```