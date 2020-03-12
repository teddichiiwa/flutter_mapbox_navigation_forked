# flutter_mapbox_navigation

Add Turn By Turn Navigation to Your Flutter Application Using MapBox. Never leave your app when you need to navigate your users to a location.

## Features

* A full-fledged turn-by-turn navigation UI for Flutter that’s ready to drop into your application
* [Professionally designed map styles](https://www.mapbox.com/maps/) for daytime and nighttime driving
* Worldwide driving, cycling, and walking directions powered by [open data](https://www.mapbox.com/about/open/) and user feedback
* Traffic avoidance and proactive rerouting based on current conditions in [over 55 countries](https://docs.mapbox.com/help/how-mapbox-works/directions/#traffic-data)
* Natural-sounding turn instructions powered by [Amazon Polly](https://aws.amazon.com/polly/) (no configuration needed)
* [Support for over two dozen languages](https://docs.mapbox.com/ios/navigation/overview/localization-and-internationalization/)

## IOS Configuration

1. Mapbox APIs and vector tiles require a Mapbox account and API access token. In the project editor, select the application target, then go to the Info tab. Under the “Custom iOS Target Properties” section, set `MGLMapboxAccessToken` to your access token. You can obtain an access token from the [Mapbox account page](https://account.mapbox.com/access-tokens/).

1. In order for the SDK to track the user’s location as they move along the route, set `NSLocationWhenInUseUsageDescription` to:
   > Shows your location on the map and helps improve OpenStreetMap.

1. Users expect the SDK to continue to track the user’s location and deliver audible instructions even while a different application is visible or the device is locked. Go to the Capabilities tab. Under the Background Modes section, enable “Audio, AirPlay, and Picture in Picture” and “Location updates”. (Alternatively, add the `audio` and `location` values to the `UIBackgroundModes` array in the Info tab.)


## Android Configuration

1. Mapbox APIs and vector tiles require a Mapbox account and API access token. Add your token in strings.xml file of your android apps res/values/ path. The string key should be "mapbox_access_token". You can obtain an access token from the [Mapbox account page](https://account.mapbox.com/access-tokens/).
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">Navigation map</string>
    <string name="mapbox_access_token" translatable="false">ADD_MAPBOX_ACCESS_TOKEN_HERE</string>
    <string name="user_location_permission_explanation">This app needs location permissions to show its functionality.</string>
    <string name="user_location_permission_not_granted">You didn\'t grant location permissions.</string>
</resources>
```

1. Add the following permission to the app level Android Manifest
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

## Usage

```dart

  MapboxNavigation _directions;

```

```dart

    initState()
    {
      _directions = MapboxNavigation(onRouteProgress: (arrived) async{
      
            _distanceRemaining = await _directions.distanceRemaining;
            _durationRemaining = await _directions.durationRemaining;
      
            setState(() {
              _arrived = arrived;
            });
            if(arrived)
              await _directions.finishNavigation();
      
          });
    }

    final cityhall = Location(name: "City Hall", latitude: 42.886448, longitude: -78.878372);
    final downtown = Location(name: "Downtown Buffalo", latitude: 42.8866177, longitude: -78.8814924);
            
    await _directions.startNavigation(
                                origin: cityhall, 
                                destination: downtown, 
                                mode: NavigationMode.drivingWithTraffic, 
                                simulateRoute: false,
                                language: "French");
  
```

## Screenshots
![Navigation View](screenshots/screenshot1.png?raw=true "iOS View") | ![Android View](screenshots/screenshot2.png?raw=true "Android View")
|:---:|:---:|
| iOS View | Android View |

## To Do
* [DONE] Android Implementation
* [DONE] Add more settings like Navigation Mode (driving, walking, etc)
* [DONE] Stream Events like relevant navigation notifications, metrics, current location, etc. 
* Embeddable Navigation View 
* Provide physical address instead of just coordinates to remove reliance on other geolocation packages