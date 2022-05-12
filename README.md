# google_maps_in_flutter
[![Build Status](https://github.com/jonashackt/google_maps_in_flutter/workflows/flutter-build/badge.svg)](https://github.com/jonashackt/google_maps_in_flutter/actions)
[![License](http://img.shields.io/:license-mit-blue.svg)](https://github.com/jonashackt/how-to-flutter/blob/master/LICENSE)
[![renovateenabled](https://img.shields.io/badge/renovate-enabled-yellow)](https://renovatebot.com)

Just doing the codelab https://codelabs.developers.google.com/codelabs/google-maps-in-flutter#3

For me my flutter SDK complains about the code in https://codelabs.developers.google.com/codelabs/google-maps-in-flutter#3

`type 'MyApp' is not a subtype of type 'StatelessWidget' in type cast`:

```
======== Exception caught by widgets library =======================================================
The following _CastError was thrown building MyApp(dirty):
type 'MyApp' is not a subtype of type 'StatelessWidget' in type cast

When the exception was thrown, this was the stack: 
#0      StatelessElement.widget (package:flutter/src/widgets/framework.dart:4824:46)
#1      StatelessElement.build (package:flutter/src/widgets/framework.dart:4827:21)
#2      ComponentElement.performRebuild (package:flutter/src/widgets/framework.dart:4754:15)
#3      Element.rebuild (package:flutter/src/widgets/framework.dart:4477:5)
#4      BuildOwner.buildScope (package:flutter/src/widgets/framework.dart:2659:19)
#5      WidgetsBinding.drawFrame (package:flutter/src/widgets/binding.dart:882:21)
#6      RendererBinding._handlePersistentFrameCallback (package:flutter/src/rendering/binding.dart:363:5)
#7      SchedulerBinding._invokeFrameCallback (package:flutter/src/scheduler/binding.dart:1144:15)
#8      SchedulerBinding.handleDrawFrame (package:flutter/src/scheduler/binding.dart:1081:9)
#9      SchedulerBinding.scheduleWarmUpFrame.<anonymous closure> (package:flutter/src/scheduler/binding.dart:862:7)
(elided 4 frames from class _RawReceivePortImpl, class _Timer, and dart:async-patch)
====================================================================================================
```

so I rebuild the code to:

```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Maps Sample App',
      theme: ThemeData(
          appBarTheme: const AppBarTheme(backgroundColor: Colors.green)),
      home: const MapsView(),
    );
  }
}

class MapsView extends StatefulWidget {
  const MapsView({Key? key}) : super(key: key);

  @override
  State<MapsView> createState() => _MapsViewState();
}

class _MapsViewState extends State<MapsView> {
  late GoogleMapController mapController;

  final LatLng _center = const LatLng(45.521563, -122.677433);

  void _onMapCreated(GoogleMapController controller) {
    mapController = controller;
  }

  @override
  Widget build(BuildContext context) {
    return GoogleMap(
      onMapCreated: _onMapCreated,
      initialCameraPosition: CameraPosition(
        target: _center,
        zoom: 11.0,
      ),
    );
  }
}

```