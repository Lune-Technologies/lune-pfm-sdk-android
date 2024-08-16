# Manually - using the AAR file

You can also add the `LuneSDK` to your project manually, using the raw `aar` file.

To do so:

1. **Download** the Lune SDK (AAR) file and place it in the `libs` older of your Android project. If the `libs` folder does not exist, create it at the root level of your project.

2. **Add Dependencies**: Open your app-level `build.gradle` and add:

```yaml
// Lune SDK
implementation(files("libs/lunesdk-release.aar"))

implementation("androidx.compose.material:material:1.5.4")
implementation("com.squareup.retrofit2:retrofit:2.9.0")
implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.2")
implementation("com.jakewharton.retrofit:retrofit2-kotlinx-serialization-converter:0.8.0")
implementation("com.squareup.okhttp3:okhttp:4.9.3")
implementation("com.squareup.okhttp3:logging-interceptor:4.9.3")
implementation("androidx.lifecycle:lifecycle-runtime-compose:2.6.2")
implementation("androidx.navigation:navigation-compose:2.6.0")
implementation("androidx.compose.material3:material3:1.2.0-alpha02")
implementation("io.coil-kt:coil-compose:2.3.0")
implementation("com.google.accompanist:accompanist-flowlayout:0.20.0")
implementation("androidx.activity:activity-ktx:1.7.2")
implementation("com.google.android.material:material:1.9.0")
```

3. **Sync** your project with the Gradle files.
