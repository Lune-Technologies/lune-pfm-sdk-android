# Activities and Fragments

Follow the steps below to initialize LuneSDK in your `Activities and Fragments` Project.

1. **Add Lune View to Layout:** In your layout file, include the Lune view by specifying its fully qualified class name:

```django
<!-- YourLayoutFile.xml -->
<io.lunedata.lunesdk.library.classes.LuneCompatManager
    android:id="@+id/luneLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    <!-- other attributes -->
/>
```

1. I**nitialize SDK in Code:** In your `Activity` or `Fragment`, create and initialize an instance of `LuneSDKManager` with the required credentials:

```kotlin
// YourActivity.kt
private lateinit var luneSDK: LuneSDKManager

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Initialize the SDK manager
    luneSDK = LuneSDKManager(
        baseUrl = "",           // Base URL
        token = "",             // User access token
        customerId = id,        // Customer ID
        refreshTokenCallback = TokenRefresher() // Callback for token refresh
    )

    // Optionally set up the callback later
    luneSDK.setupRefreshCallback(TokenRefresher())

    // Configure the Lune view
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.manager = luneSDK
    luneView.component = LuneView.ActivityComponent
}
```
