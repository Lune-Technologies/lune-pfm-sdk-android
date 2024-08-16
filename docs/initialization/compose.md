# Jetpack Compose

Follow the steps below to initialize LuneSDK in your `Jetpack Compose` Project.

1. **Initialize SDK:** In your `MainActivity`, initialize the `LuneSDKManager` in the `onCreate()` method:

```kotlin
// MainActivity.kt
class MainActivity : ComponentActivity() {

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

        // Set up the UI using Jetpack Compose
        setContent {
            LuneBankTheme {
                Surface(modifier = Modifier.fillMaxSize()) {
                    RootComposable(luneSDK)
                }
            }
        }
    }
}


```

1. **Using SDK in Composables:** Pass the `LuneSDKManager` instance to any composable in your app:

```kotlin
@Composable
fun TransactionView(luneSDK: LuneSDKManager) {
    // Use the SDK
    luneSDK.TransactionListComponent()
}
```

Tip:

While you could create multiple instances of `LuneSDKManager`, we recommend creating just one instance per app.

You could share that single instance with other views by passing it around.
