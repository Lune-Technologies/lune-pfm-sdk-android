# Initialization

To initialize the SDK, you simply have to create an instance of `LuneSDKManager` that would be used across your app.

Provided that you will need to initialize LuneSDK with credentials, you may want to do all the prep work within a view-model. That includes things like:

- getting the credentials
- setting up a refresh callback (optional, based on the TTL of your credentials)
- setting up logging (also optional)

You can find specific implementation details for your project setup below:

1. [Jetpack Compose](compose.md)
2. [Activities and Fragments](activities.md)

---

## Refresh Token Callback

The refresh token callback is a critical component for managing user authentication tokens. It handles the situation where the user's access token expires and needs to be refreshed.

**Implementation:**

The callback function is implemented in the `TokenRefresher` class, which you can customize based on your app's needs:

```kotlin
class TokenRefresher: RefreshTokenCallback {
    override fun refreshToken(): String? {
    // Implement logic to refresh the token and obtain a new access token

     return "" // make sure to return the token here
    }
}
```

**Usage:**

- **Activities and Fragments:**

The `TokenRefresher` class is passed as a parameter to the `LuneSDKManager` during initialization. It can also be set up later using

the `setupRefreshCallback()` method.

- **Jetpack Compose:**

The `TokenRefresher` class is used in the same way as in Activities and Fragments, ensuring a unified approach across different integration methods

**Example:**

```plain
// Setting up the callback during initialization
luneSDK = LuneSDKManager(
    baseUrl = "",           // Base URL
    token = "",             // User access token
    customerId = id,        // Customer ID
    refreshTokenCallback = TokenRefresher() // Callback for token refresh
)

// Setting up the callback later
luneSDK.setupRefreshCallback(TokenRefresher())
```

This unified approach to implementing the refresh token callback ensures consistency across different integration methods, simplifying the token management process within your application.

---

## Optional

1. To allow buttons to float above the keyboard, add the following attribute in your `AndroidManifest.xml` file:

```xml
<activity android:windowSoftInputMode="adjustResize">
    <!--this allows buttons to float above the keyboard-->
</activity>


```
