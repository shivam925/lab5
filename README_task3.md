# Project Setup
## Create a new Flutter project:

```bash

flutter create live_location_tracking_app
cd live_location_tracking_app
```
Add required dependencies in `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter: ^2.1.1
  location: ^4.3.0
```
Run `flutter pub get` to fetch the dependencies.

## Google Maps API Key
Get an API key from the Google Cloud Console.

Add the API key to your AndroidManifest.xml and Info.plist files.

## Code Implementation
1. `main.dart`
```dart

import 'package:flutter/material.dart';
import 'package:live_location_tracking_app/screens/map_screen.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Live Location Tracking',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MapScreen(),
    );
  }
}
```

## 2. `map_screen.dart`

```dart

import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';
import 'package:location/location.dart';

class MapScreen extends StatefulWidget {
  @override
  _MapScreenState createState() => _MapScreenState();
}

class _MapScreenState extends State<MapScreen> {
  GoogleMapController _controller;
  Location _location = Location();
  LatLng _currentLocation;

  @override
  void initState() {
    super.initState();
    _getLocation();
  }

  void _getLocation() async {
    var locationData = await _location.getLocation();
    setState(() {
      _currentLocation = LatLng(locationData.latitude, locationData.longitude);
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Live Location Tracking'),
      ),
      body: _currentLocation != null
          ? GoogleMap(
              initialCameraPosition: CameraPosition(
                target: _currentLocation,
                zoom: 15.0,
              ),
              onMapCreated: (GoogleMapController controller) {
                _controller = controller;
              },
              myLocationEnabled: true,
              myLocationButtonEnabled: true,
              markers: {
                Marker(
                  markerId: MarkerId('currentLocation'),
                  position: _currentLocation,
                  infoWindow: InfoWindow(
                    title: 'Current Location',
                  ),
                ),
              },
            )
          : Center(
              child: CircularProgressIndicator(),
            ),
    );
  }
}
```
