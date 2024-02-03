# Step 1: Set up Flutter Project
Create a new Flutter project using the following command:

```bash

flutter create osm_flutter_app
Open the project in your preferred code editor (e.g., Visual Studio Code).
```

## Step 2: Add Dependencies
Add the `flutter_map` and `latlong` dependencies to your `pubspec.yaml` file:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_map: ^0.15.0
  latlong: ^0.7.1
```
Run flutter pub get to install the dependencies.

## Step 3: Create the Map Page
Create a new Dart file for the map page (e.g.,` map_page.dart`), and implement a simple OSM map with markers.

```dart

// map_page.dart
import 'package:flutter/material.dart';
import 'package:flutter_map/flutter_map.dart';
import 'package:latlong/latlong.dart';

class MapPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('OSM Flutter App'),
      ),
      body: FlutterMap(
        options: MapOptions(
          center: LatLng(-20.348404, 57.552152), // Center of Mauritius
          zoom: 10.0,
        ),
        layers: [
          TileLayerOptions(
            urlTemplate: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
            subdomains: ['a', 'b', 'c'],
          ),
          MarkerLayerOptions(
            markers: _buildMarkers(),
          ),
        ],
      ),
    );
  }

  List<Marker> _buildMarkers() {
    return [
      Marker(
        width: 30.0,
        height: 30.0,
        point: LatLng(-20.1635, 57.5051), // Hotel 1
        builder: (ctx) => MarkerWidget("Hotel 1"),
      ),
      // Repeat for other hotels...
    ];
  }
}

class MarkerWidget extends StatelessWidget {
  final String hotelName;

  MarkerWidget(this.hotelName);

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        // Handle marker tap, show info window, etc.
        Scaffold.of(context).showSnackBar(
          SnackBar(
            content: Text('Clicked on $hotelName'),
          ),
        );
      },
      child: Icon(
        Icons.hotel,
        color: Colors.red,
      ),
    );
  }
}
```

## Step 4: Use the Map Page
Modify the `main.dart` file to use the `MapPage`:

```dart
// main.dart
import 'package:flutter/material.dart';
import 'map_page.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'OSM Flutter App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MapPage(),
    );
  }
}
```

## Step 5: Run the App
Run the app on an emulator or physical device:

```bash

flutter run
```
