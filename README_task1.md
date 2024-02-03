# Adding Google Maps to Flutter App

In this tutorial, we'll go through the steps to integrate Google Maps into a Flutter app. Google Maps can be a valuable addition to your application, providing users with location-based services and interactive maps.

## Prerequisites

Before you begin, make sure you have the following:

- Flutter installed on your development machine.
- A Google Maps API Key. You can obtain one by following the instructions [here](https://developers.google.com/maps/documentation/javascript/get-api-key).

## Steps

### 1. Create a new Flutter Project

If you don't have a Flutter project yet, create one using the following command:

```bash
flutter create my_google_maps_app
cd my_google_maps_app
```

### 2. Add Google Maps Plugin
Add the Google Maps Flutter plugin to your pubspec.yaml file:

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter: ^2.0.9
```

Run `flutter pub get` to install the dependency.

### 3. Set up Android and iOS
Android
Open the `android/app/src/main/AndroidManifest.xml` file and add the following permissions:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
Make sure you have the required Google Play services version by adding the following to the `<application>` element:

```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_GOOGLE_MAPS_API_KEY" />
iOS
```

Open the ios/Runner/Info.plist file and add the following:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>YOUR_DESCRIPTION_HERE</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>YOUR_DESCRIPTION_HERE</string>
```
Replace YOUR_GOOGLE_MAPS_API_KEY with your actual API key and provide appropriate location usage descriptions.

### 4. Implement Google Maps
Now, let's create a simple Flutter widget that displays Google Maps.

```dart

import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MapScreen(),
    );
  }
}

class MapScreen extends StatefulWidget {
  @override
  _MapScreenState createState() => _MapScreenState();
}

class _MapScreenState extends State<MapScreen> {
  GoogleMapController mapController;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Google Maps Flutter'),
      ),
      body: GoogleMap(
        onMapCreated: (controller) {
          setState(() {
            mapController = controller;
          });
        },
        initialCameraPosition: CameraPosition(
          target: LatLng(37.7749, -122.4194), // Initial map position (San Francisco, CA)
          zoom: 12.0,
        ),
      ),
    );
  }
}
```

### 5. Run your App
Run your app using:

```bash

flutter run
```
