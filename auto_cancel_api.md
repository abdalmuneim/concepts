// This file explains the concept of Auto Cancel for API in Flutter.

// Auto Cancel for API ensures that ongoing API calls are canceled when they are no longer needed.
// This is useful for avoiding memory leaks and unnecessary network usage.

// Example implementation with http:
```
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:async';

class AutoCancelApiExample extends StatefulWidget {
  const AutoCancelApiExample({super.key});

  @override
  _AutoCancelApiExampleState createState() => _AutoCancelApiExampleState();
}

class _AutoCancelApiExampleState extends State<AutoCancelApiExample> {
  Timer? _debounce;

  void _fetchData(String query) {
    _debounce?.cancel();
    _debounce = Timer(Duration(milliseconds: 500), () async {
      final response =
          await http.get(Uri.parse('https://api.example.com/search?q=$query'));
      if (response.statusCode == 200) {
        print('Data fetched: ${response.body}');
      } else {
        print('Error fetching data');
      }
    });
  }

  @override
  void dispose() {
    _debounce?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Auto Cancel API Example')),
      body: TextField(
        onChanged: _fetchData,
        decoration: InputDecoration(labelText: 'Search'),
      ),
    );
  }
}
```
// Example implementation with dio:
```
final cancelToken = CancelToken();
dio.get(url, cancelToken: cancelToken);

// Later
cancelToken.cancel("Request cancelled");
```