# 🧭 [Compass and Navigation ball apps](https://play.google.com/store/apps/dev?id=6476747734095778835)
## for Android and Wear OS

### Features

- Instant startup.
- Very battery-friendly.
- Extremely tiny, only about 100 kB.
- Works correctly when upside down!
- Shows the horizon with compass directions.
- It shows accurate directions and attitude in every possible device orientation, upside down or whatever. Imagine traveling through space.
- Made with only vector graphics and lots of heavy mathematics. No textures nor 3D libs.
- Has night mode.
- Zoomable without loss of quality.
- LIVE WALLPAPER is included.
- Wear OS app for your watch is included.
- No ads, no permissions.


### How you can control those apps externally

Our compass and navball apps can be used as instruments, to visualize external data. To do this:

- Add the following permission to your Android manifest:

  - Standard compass:
```xml
<uses-permission
    android:name="com.sublimis.compass.std.permission.CONTROL" />
```
  - AHRS compass:
```xml
<uses-permission
    android:name="com.sublimis.compass.ahrs.permission.CONTROL" />
```
  - KSP navball:
```xml
<uses-permission
    android:name="com.sublimis.navball.ksp.permission.CONTROL" />
```
  - NASA Apollo FDAI navball:
```xml
<uses-permission
    android:name="com.sublimis.navball.apollo.permission.CONTROL" />
```

- Start a wanted component activity:
  - Standard compass: `"com.sublimis.compass.ActivityMain"`
  - AHRS compass: `"com.sublimis.compass.ahrs.ActivityMain"`
  - KSP navball: `"com.sublimis.navball.ksp.ActivityMain"`
  - Apollo FDAI navball: `"com.sublimis.navball.apollo.ActivityMain"`

- Send movement data like this:
```java
// See {@link https://developer.android.com/reference/android/hardware/SensorEvent#sensor.type_rotation_vector:}
final float[] quaternion = new float[]{x_sin, y_sin, z_sin, cos, accuracy};
final long timestamp = SystemClock.elapsedRealtimeNanos();

final Intent intent = new Intent("com.sublimis.compass.ACTION.MOVE");
intent.putExtra("com.sublimis.compass.ACTION.MOVE.QUAT", (float[]) quaternion);
intent.putExtra("com.sublimis.compass.ACTION.TIMESTAMP", (long) timestamp);

sendBroadcast(intent, "com.sublimis.compass.permission.CONTROL");
```

- Send meta data and looks choices like this:
```java
final Intent intent = new Intent("com.sublimis.compass.ACTION.META");
intent.putExtra("com.sublimis.compass.APP.NAME", "Example App Name");
intent.putExtra("com.sublimis.compass.APP.VERSION.CODE", (int) 1);
intent.putExtra("com.sublimis.compass.ACTION.META.SCALE", (float) 1.0);
intent.putExtra("com.sublimis.compass.ACTION.META.NIGHT_THEME", (int) 0);
intent.putExtra("com.sublimis.compass.ACTION.META.SWAP_HORIZONTAL", (int) 0);
intent.putExtra("com.sublimis.compass.ACTION.TIMESTAMP",(long) timestamp);

sendBroadcast(intent, "com.sublimis.compass.permission.CONTROL");
```

The app needs at least one broadcast per 10 seconds to remain in the "instrument mode".
The app will revert to "standard mode" (visualizing internal sensor data) if there's no broadcast for more than 10 seconds.
