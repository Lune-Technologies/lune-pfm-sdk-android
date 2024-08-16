# Documentation for the Lune PFM Android SDK

  

📦 An SDK to embed Lune enrichment views into your Android mobile apps

  

* * *

  

## Requirements

  

- [ ]     Android Studio Arctic Fox (2020.3.1) or later
- [ ]     Kotlin 1.5.10 or later
- [ ]     Minimum Android API level 21 (Android 5)
- [ ]     Lune SDK (AAR) file

# Installation

  

There are a bunch of options to choose from when it comes to how to install the `LuneSDK`.

Follow the instructions below for any of the options you prefer:

  

1. Maven Central ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11498](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11498))
2. Manually - using the AAR file ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11518](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11518))

# Maven Central

  

To install the `LuneSDK` into your project from Maven Central:

  

  

1. **Add Dependency**: In your app-level `build.gradle`, add:

```yaml
// Lune SDK
implementation("io.lunedata.lunesdk:lunesdk:$luneSdkVersion")


```

Replace `$luneSdkVersion` with the latest version.

  

1. **Sync** your project with Gradle.

# Manually - using the AAR file

  

You can also add the `LuneSDK` to your project manually, using the raw `aar` file.

To do so:

  

1. **Download** the Lune SDK (AAR) file and place it in the `libs` older of your Android project. If the `libs` folder does not exist, create it at the root level of your project.

  

1. **Add Dependencies**: Open your app-level `build.gradle` and add:

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

  

1. **Sync** your project with the Gradle files.

# Initialization

  

To initialize the SDK, you simply have to create an instance of `LuneSDKManager`   that would be used across your app.

  

  

Provided that you will need to initialize LuneSDK with credentials, you may want to do all the prep work within a view-model. That includes things like:

*   getting the credentials
*   setting up a refresh callback (optional, based on the TTL of your credentials)
*   setting up logging (also optional)

  

You can find specific implementation details for your project setup below:

  

1. Jetpack Compose ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11558](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11558))
2. Activities and Fragments ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11578](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11578))

  

* * *

  

### Refresh Token Callback

  

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

  

*   **Activities and Fragments:**

The `TokenRefresher` class is passed as a parameter to the `LuneSDKManager` during initialization. It can also be set up later using

the `setupRefreshCallback()` method.

  

  

*   **Jetpack Compose:**

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

  

  

* * *

  

## Optional

  

1. To allow buttons to float above the keyboard, add the following attribute in your `AndroidManifest.xml` file:

  

```xml
<activity android:windowSoftInputMode="adjustResize">
    <!--this allows buttons to float above the keyboard-->
</activity>


```

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

# Customization

  

The SDK has lots of configurable parameters which can be overridden by setting up a `JSON` file with a schema similar to the one attached below. You could just download the file and modify the values you wish to change.

  

[lune\_config.json](https://t4627279.p.clickup-attachments.com/t4627279/c28bc95e-7de8-4466-b74d-b9de46e16e81/lune_config.json)

  

Once edited, save the JSON file as `lune_config.json` and place it in the `/res/raw` directory of your Android project. The final path should be `/res/raw/lune_config.json`.

# Localization and Strings

  

If your app is already localized, the SDK would be localized as well - no configurations needed. If your app is not localized, however, the SDK respects that and stays in English to preserve consistency and uniformity across your app.

  

  

## String Overrides

  

You can override specific strings by assigning a different value to the same string keys used by the SDK.

The strings used in the SDK can be found in the localization file attached below.

[lunesdk-localizations.zip](https://t4627279.p.clickup-attachments.com/t4627279/bf9a5157-b1a2-4a0a-8526-062114884300/lunesdk-localizations.zip)

  

As you may have noticed, the keys are unique and should not conflict with any other strings in your project.

# Images

  

You can override any of the images in `LuneSDK` by simply giving any other images in your project the exact same name.

  

Each image is named using the format:

```gherkin
lune_asset_<image_name>


```

  

You can find a list of the images you can override here:

[lune-asset-names.txt](https://t4627279.p.clickup-attachments.com/t4627279/5de6dad7-b11a-4798-a11d-d63e072247b2/lune-asset-names.txt)

# Components

  

The Lune SDK components are broadly divided into full-page views and smaller (mix and match) components.

  

  

## Full page views

  

These components are typically large and were designed to be used as stand-alone components on a page. While you could still choose to do so, we strongly discourage adding other widgets as siblings to full page views.

  

#### Examples are:

  

1. CashflowComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6464](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6464))
2. ExpenseComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6524](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6524))
3. TransactionListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6344](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6344))
4. TransactionDetailComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6384](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6384))
5. BrandTrendsComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6484](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6484))
6. CategoryTrendsComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6544](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6544))
7. CategorySpendListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6444](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6444))
8. BrandListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6424](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6424))
9. BudgetComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6564](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6564))

  

  

  

## Smaller (mix and match) Components

  

These components are usually small enough and could easily be arranged as siblings of other widgets on a page.

  

#### Examples are:

  

1. CashflowChartComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11218](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11218))
2. CategorySpendChartComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6504](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6504))
3. CategoryTrendChartComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11238](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11238))
4. BrandTrendChartComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11258](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-11258))
5. BudgetFormComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6404](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6404))
6. BudgetSummaryComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6364](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6364))

# CashflowComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/7b11d95b-7db5-486e-8d2e-07923e72860d/Screenshot%202023-08-30%20at%208.57.55%20AM.png)

  

The `CashflowComponent` shows the user's gross expense and income over a period of time, along with the

difference between them. To use this view, just call the `CashflowComponent` method of

your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// CashflowView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun CashflowView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CashflowComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CashflowComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CashflowComponent
}
```

  

✨ You can now add an optional argument to the `CashflowViewParams(view)` parameter if you need to render a custom view as the footer of the screen.

  

Here is a simple example with a list of cards.

```kotlin
            lunekit = LuneSDKManager(
                baseUrl = ClientApi.baseURl ?: "",
                token?: "",
            )
            


            val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)

            val view = layoutInflater.inflate(R.layout.layout_recycle, null, false)

            val recyclerview = view.findViewById<RecyclerView>(R.id.list_item)
            val viewAll = view.findViewById<TextView>(R.id.tv_viewAll)

            viewAll.setOnClickListener {
                Toast.makeText(this, "View All Click Action", Toast.LENGTH_LONG).show()
            }

            recyclerview.layoutManager = LinearLayoutManager(this, RecyclerView.HORIZONTAL, false)

            val data = ArrayList<String>()
            for (i in 1..20) {
                data.add("Item $i")
            }

            val adapter = CustomAdapter(data)
            recyclerview.adapter = adapter

            luneView.manager = lunekit
            luneView.component = LuneView.CashflowComponent
            luneView.data =  CashflowViewParams(view)
```

  

* * *

  

## Localization Keys and Analytics

/

  

![](https://t4627279.p.clickup-attachments.com/t4627279/9021730e-cbee-4cc9-9bee-85f54c4b0248/CleanShot%202024-08-15%20at%2013.47.47%402x.png)

Analytics Tags

  

1. `cashflow_amount`
2. `outflow_amount`
3. `outflow_tile`
4. `inflow_tile`

  

Localization Keys

  

1. `lune_sdk_str_outflow`
2. `lune_sdk_str_inflow`
3. `lune_sdk_str_you_ve_spent`, `lune_sdk_str_more_than_you_have_earned`, `lune_sdk_str_you_ve_earned`, `lune_sdk_str_more_than_you_have_spent`

# ExpenseComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/dc66c1b0-e4de-477d-be09-fb2de7200208/Screenshot%202023-08-30%20at%209.02.57%20AM.png)

  

The `ExpenseComponent` shows the user's spend across categories in a neat donut chart, along with a

list of recent transactions. Users are able to view data for previous months easily too. To use this view, just call the `ExpenseComponent` method of your `LuneSDKManager` instance as shown in the

example below.

  

```kotlin
// ExpenseView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun ExpenseView(
    luneSDK: LuneSDKManager
) {
    luneSDK.ExpenseComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.ExpenseComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.ExpenseComponent
}
```

  

✨ You can now add an optional argument to the `ExpenseViewParams(view)` parameter if you need to render a custom view between the chart and the transaction list.

  

Here is a simple example with a list of cards.

  

```kotlin
            lunekit = LuneSDKManager(
                baseUrl = ClientApi.baseURl ?: "",
                token?: "",
            )
            


            val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)

            val view = layoutInflater.inflate(R.layout.layout_recycle, null, false)

            val recyclerview = view.findViewById<RecyclerView>(R.id.list_item)
            val viewAll = view.findViewById<TextView>(R.id.tv_viewAll)

            viewAll.setOnClickListener {
                Toast.makeText(this, "View All Click Action", Toast.LENGTH_LONG).show()
            }

            recyclerview.layoutManager = LinearLayoutManager(this, RecyclerView.HORIZONTAL, false)

            val data = ArrayList<String>()
            for (i in 1..20) {
                data.add("Item $i")
            }

            val adapter = CustomAdapter(data)
            recyclerview.adapter = adapter

            luneView.manager = lunekit
            luneView.component = LuneView.ExpenseComponent
            luneView.data =  ExpenseViewParams(view)


```

  

## Localization Keys and Analytics

  

![](https://t4627279.p.clickup-attachments.com/t4627279/2324d417-66ac-484f-9029-e7ccd7e248b7/CleanShot%202024-08-15%20at%2016.44.04%402x.png)

Analytics Tags

  

1. `date_picker_button`
2. `chart_icon`
3. `chart_view_all`
4. `transaction_amount`
5. `transaction_tile`
6. `transactions_view_all`

  

Localization Keys

  

1. `lune_sdk_str_view_all`
2. `lune_sdk_str_view_all`
3. `lune_sdk_str_transactions`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/71ff27b4-41a1-428a-95b1-3a8c8b4f54a6/image.png)

Analytics Tags

  

1. `date_picker_button`
2. `trends_button`
3. `category_tile`
4. `category_amount`
5. `avg_spend_sort_button`
6. `name_sort_button`

  

Localization Keys

  

1. `lune_sdk_str_all_spends`
2. `lune_sdk_str_name`
3. `lune_sdk_str_avg_spend`
4. `lune_sdk_str_transactions`
5. `lune_sdk_str_trends`

# TransactionListComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/cbef54ee-1852-4982-9546-f16892a7ea17/Screenshot%202023-08-30%20at%209.05.38%20AM.png)

  

The `TransactionListComponent` shows a list of enriched transactions in a user-friendly way, with each transaction having an associated brand. To use this view, just call the `TransactionListComponent` method of your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// TransactionView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TransactionView(
    luneSDK: LuneSDKManager
) {
    luneSDK.TransactionListComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.TransactionListComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.TransactionListComponent
}
```

  

* * *

  

## Localization Keys and Analytics

  

![](https://t4627279.p.clickup-attachments.com/t4627279/fcf47394-792f-46ec-8c2d-e1c8f81ed770/CleanShot%202024-08-15%20at%2016.58.32%402x.png)

Analytics Tags

  

1. `summary_amount`
2. `date_picker_button`
3. `trends_button`
4. `filter_button`
5. `reported_switch`
6. `amount_sort_button`
7. `date_sort_button`
8. `transaction_amount`
9. `transaction_tile`

  

Localization Keys

  

1. `lune_sdk_str_transactions` ,`lune_sdk_str_no_transactions`
2. `lune_sdk_str_date`
3. `lune_sdk_str_amount`
4. `lune_sdk_str_reported`
5. `lune_sdk_str_trends`
6. `lune_sdk_str_search`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/e2255db5-4c8e-430d-832a-adbc7badb0c0/CleanShot%202024-08-15%20at%2017.06.51%402x.png)

Analytics Tags

  

1. `close_button`
2. `category_filter_pane`
3. `tag_filter_pane`
4. `apply_button`
5. `reset_button`

  

Localization Keys

  

1. `lune_sdk_str_filters`
2. `lune_sdk_str_filter_by_category`
3. `lune_sdk_str_search`
4. `lune_sdk_str_filter_by_tag`
5. `lune_sdk_str_apply`
6. `lune_sdk_str_reset`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/86cb72c1-2294-4c3e-80b7-2e97bb8ec697/CleanShot%202024-08-15%20at%2017.08.11%402x.png)

Analytics Tags

  

1. `tag_filter_option`

# TransactionDetailComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/1782b865-3d41-4580-87e5-aa115383eeb1/image.png)

  

The `TransactionDetailComponent` shows users information about a specific transaction and allows them

to update its category or Brand, in case they are inaccurate. To use this view, just call the `TransactionDetailC+`

`omponent` method of your `LuneSDKManager` instance as shown in the example below. The method takes the `id` of the transaction as an argument.

  

```kotlin
// TransactionDetailView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TransactionDetailComponent(
    luneSDK: LuneSDKManager
) {
    luneSDK.TransactionDetailComponent(id = transaction.id)
}
```

  

To use this view in a project with Activities and Fragments, set the `data` property of your view to the `transaction ID,` and the `component` property should be set to `LuneView.TransactionDetailComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)

    //set the data property to the transaction ID
    luneView.data = <transaction.id>

    // set the component property.
    luneView.component = LuneView.TransactionDetailComponent
}
```

  

* * *

  

## Localization Keys and Analytics

  

![](https://t4627279.p.clickup-attachments.com/t4627279/4a78199d-caff-4fc6-977c-88eab3ac4c2c/CleanShot%202024-08-15%20at%2017.12.38%402x.png)

Analytics Tags

  

1. `report_transaction_button`
2. `save_button`

  

Localization Keys

  

1. `lune_sdk_str_amount`
2. `lune_sdk_str_date`, `lune_sdk_str_date_time`
3. `lune_sdk_str_category`
4. `lune_sdk_str_suggested_category`
5. `lune_sdk_str_raw_transaction`
6. `lune_sdk_str_notes`
7. `lune_sdk_str_tap_to_add_notes`
8. `lune_sdk_str_tags`
9. `lune_sdk_str_tap_to_add_tags`
10. `lune_sdk_str_report_transaction`, `lune_sdk_str_cancel_report`
11. `lune_sdk_str_save`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/d06cf800-d318-42aa-8f10-562d2a3f5beb/CleanShot%202024-08-15%20at%2017.15.13%402x.png)

Analytics Tags

  

1. `close_button`
2. `incorrect_brand_tile`
3. `incorrect_logo_tile`
4. `incorrect_category_tile`
5. `report_button`

  

Localization Keys

  

1. `lune_sdk_str_report_transaction_title`
2. `lune_sdk_str_incorrect_brand_name`
3. `lune_sdk_str_suggested_brand`
4. `lune_sdk_str_incorrect_logo`
5. `lune_sdk_str_incorrect_category`
6. `lune_sdk_str_report`
7. `lune_sdk_str_brand_name`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/e3f460af-297a-42d2-8e3b-2df43df1e7f5/CleanShot%202024-08-15%20at%2017.16.35%402x.png)

Analytics Tags

  

1. `close_button`
2. `category_filter_option`
3. `custom_category_button`
4. `submit_button`

  

Localization Keys

  

1. `lune_sdk_str_report_category`
2. `lune_sdk_str_please_select_category`
3. `lune_sdk_str_search`
4. `lune_sdk_str_add_a_custom_category`
5. `lune_sdk_str_submit`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/5e7e56bd-1150-4337-ba83-d88c5952069d/CleanShot%202024-08-15%20at%2017.18.47%402x.png)

Analytics Tags

  

1. `close_button`
2. `submit_button`

  

Localization Keys

  

1. `lune_sdk_str_add_a_custom_category`
2. `lune_sdk_str_category_name_field_label`
3. `lune_sdk_str_category_name`
4. `lune_sdk_str_submit`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/732b65cc-553d-43f0-bc6f-15917a375597/CleanShot%202024-08-15%20at%2017.20.04%402x.png)

Analytics Tags

  

1. `close_button`

  

Localization Keys

  

1. `lune_sdk_str_update_done`

# BrandTrendsComponent

  

![](https://t4627279.p.clickup-attachments.com/t4627279/d68422be-9d74-4f9a-9faa-01035ee7cea1/Screenshot%202023-08-30%20at%209.00.28%20AM.png)

  

Besides showing the user's spend on each Brand, the `BrandTrendsComponent` uses a chart to show the

user's spend per day. It also allows users to filter data by category, tags and dates by default.

  

To use this view, just call the `BrandTrendsComponent` method of your `LuneSDKManager` instance as

shown in the example below.

  

```kotlin
// TrendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TrendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.BrandTrendsComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BrandTrendsComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BrandTrendsComponent
}
```

  

* * *

  

## Localization Keys and Analytics

  

![](https://t4627279.p.clickup-attachments.com/t4627279/d4ad0f6d-804a-4946-a2cb-f0ee4d05a534/CleanShot%202024-08-15%20at%2017.23.06%402x.png)

Analytics Tags

  

1. `date_picker_button`
2. `filter_button`
3. `spending_amount`
4. `brand_amount`
5. `brand_tile`

  

Localization Keys

  

1. `lune_sdk_str_brand_trends`
2. `lune_sdk_str_spending`
3. `lune_sdk_str_top_brands`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/23833778-01fb-4c2d-9c39-d9bc7f256d40/CleanShot%202024-08-16%20at%2009.55.05%402x.png)

Analytics Tags

  

1. `close_button`
2. `category_filter_option`
3. `apply_button`
4. `reset_button`

  

Localization Keys

  

1. `lune_sdk_str_filter_by_category`
2. `lune_sdk_str_search`
3. `lune_sdk_str_apply`
4. `lune_sdk_str_reset`

# CategoryTrendsComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/da749fc7-9d00-414a-87bf-3c5d90a9678d/Screenshot%202023-08-30%20at%208.59.14%20AM.png)

  

The `CategoryTrendsComponent` shows the user's spend across different categories both in a chart, and

in a list of category tiles. It allows users to filter data by dates too.

  

To use this view, just call the `CategoryTrendsComponent` method of your `LuneSDKManager` instance as

shown in the example below.

  

```kotlin
// TrendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TrendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CategoryTrendsComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CategoryTrendsComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategoryTrendsComponent
}
```

  

  

  

* * *

  

## Localization Keys and Analytics

  

![](https://t4627279.p.clickup-attachments.com/t4627279/ed5ff88f-1250-40a7-80bc-512660e2d9cf/CleanShot%202024-08-15%20at%2017.28.37%402x.png)

Analytics Tags

  

1. `filter_button`
2. `spending_amount`
3. `date_picker_button`
4. `category_amount`
5. `category_tile`

  

Localization Keys

  

1. `lune_sdk_str_categories_trends`
2. `lune_sdk_str_spending`
3. `lune_sdk_str_top_categories`

  

* * *

  

![](https://t4627279.p.clickup-attachments.com/t4627279/1944dca1-7c5c-4aff-83cb-6216e56ab3fb/CleanShot%202024-08-16%20at%2009.55.05%402x.png)

Analytics Tags

  

1. `close_button`
2. `category_filter_option`
3. `apply_button`
4. `reset_button`

  

Localization Keys

  

1. `lune_sdk_str_filter_by_category`
2. `lune_sdk_str_search`
3. `lune_sdk_str_apply`
4. `lune_sdk_str_reset`

# CategorySpendListComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/e1568f7f-d2c3-43cc-8eef-edfdcf268613/Screenshot%202023-08-30%20at%208.57.11%20AM.png)

  

The `CategorySpendListComponent` shows the user's overall spend in contrast to his budget and his

expected spend per time. To use this view, just call the `CategorySpendListComponent` method of

your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// CategorySpendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun CategorySpendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CategorySpendListComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CategorySpendListComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategorySpendListComponent
}
```

# BrandListComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/aa8ff129-7e6c-4420-8175-0b4f8b87ded2/Screenshot%202023-08-30%20at%209.04.45%20AM.png)

  

The `BrandListComponent` shows a list of brands the user has patronized. To use this view, just call the `BrandListComponent` method of your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// BrandListView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun BrandListComponent(
    luneSDK: LuneSDKManager
) {
    luneSDK.BrandListComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BrandListComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BrandListComponent
}
```

# BudgetComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/4ff35d42-a4a9-4bf0-ab36-3d9079032e32/Screenshot%202023-08-30%20at%209.01.42%20AM.png)

  

The `BudgetComponent` shows the user's spend against the budget he/she had set previously. In the case

that the user doesn't already have a budget, it allows the user to set one easily.

  

To use this view, just call the `BudgetComponent` method of your `LuneSDKManager` instance as shown

in the example below.

  

```kotlin
// BudgetView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun BudgetView(
    luneSDK: LuneSDKManager
) {
    luneSDK.BudgetComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BudgetComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BudgetComponent
}
```

# CashflowChartComponent

  

![](https://t4627279.p.clickup-attachments.com/t4627279/7af4ef3a-b88b-4012-9cd0-bc92d46e1faf/CleanShot%202024-02-02%20at%2013.21.00%402x.png)

The `CashflowChartComponent` shows the user's gross expense and income over a period of time, along with the difference between them, in a clean customizable donut chart.

  

To use this component, just call the `CashflowChartComponent` method of your `LuneSDKManager` instance, as shown in the example below. You can pass in optional `startDate` and `endDate` arguments to filter the data shown.

  

```kotlin
// CashflowView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun CashflowView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CashflowChartComponent(startDate = "",
           endDate = "", showAmountLabel = true)
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CashflowComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CashflowChartComponent
    luneView.data = CashflowDateRangeParams(startDate = "",
           endDate = "", showAmountLabel = true)
}
```

# CategorySpendChartComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/597260dd-502e-4332-8964-662b21d8206b/Screenshot%202023-08-30%20at%208.55.31%20AM.png)

  

The `CategorySpendChartComponent` shows the user's most significant spend across various expense

categories. To use this view, just call the `CategorySpendChartComponent` method of your `LuneSDKManager`

instance as shown in the example below.

  

```kotlin
// CategorySpendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun CategorySpendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CategorySpendChartComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CategorySpendChartComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategorySpendChartComponent
}
```

# CategoryTrendChartComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/cfbf62f1-e79a-4be3-af41-504965322583/CleanShot%202024-02-02%20at%2013.28.18%402x.png)

The `CategoryTrendChartComponent` shows the user's spend across different categories in a chart.

  

To use this view, call the `CategoryTrendChartComponent` method of your `LuneSDKManager` instance as shown in the example below. You can pass in optional `startDate` and `endDate` arguments to filter the data shown.

  

```kotlin
// TrendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TrendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.CategoryTrendChartComponent(startDate = "",
           endDate = "")
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.CategoryTrendChartComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategoryTrendChartComponent
    luneView.data =  CategoryTrendDateRangeParams(startDate = "", endDate = "")
}
```

# BrandTrendChartComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/00ff9276-8500-472d-809e-5c76833b01d7/CleanShot%202024-02-02%20at%2013.31.44%402x.png)

The `BrandTrendChartComponent` shows the user's spend across different brands in a chart.

  

  

To use this view, just call the `BrandTrendChartComponent` method of your `LuneSDKManager` instance as shown in the example below. You can pass in optional `startDate` and `endDate` arguments to filter the data shown.

  

```kotlin
// TrendView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun TrendView(
    luneSDK: LuneSDKManager
) {
    luneSDK.BrandTrendChartComponent(startDate = "",
           endDate = "")
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BrandTrendChartComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BrandTrendChartComponent
    luneView.data =  BrandTrendDateRangeParams(startDate = "", endDate = "")
}
```

# BudgetFormComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/2b21a6e8-c04d-4206-ab96-310278f53b6a/image.png)

  

The `BudgetFormComponent` allows the user to set up a new budget. To use this view, just call the `BudgetFormComponent` method of your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// BudgetFormView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun BudgetFormView(
    luneSDK: LuneSDKManager
) {
    luneSDK.BudgetFormComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BudgetFormComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BudgetFormComponent
}
```

# BudgetSummaryComponent

![](https://t4627279.p.clickup-attachments.com/t4627279/92bc1e25-8635-4d04-bd04-2aceb9143ead/Screenshot%202023-08-30%20at%208.56.15%20AM.png)

  

The `BudgetSummaryComponent` shows the user's overall spend in contrast to his budget and his expected

spend per time. To use this view, just call the `BudgetSummaryComponent` method of

your `LuneSDKManager` instance as shown in the example below.

  

```kotlin
// BudgetSummaryView.kt

import io.lunedata.lunesdk.library.classes.LuneSDKManager

@Composable
fun BudgetSummaryView(
    luneSDK: LuneSDKManager
) {
    luneSDK.BudgetSummaryComponent()
}
```

  

To use this view in a project with Activities and Fragments, set the `component` property of your view to `LuneView.BudgetSummaryComponent`, as shown in the example below.

  

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BudgetSummaryComponent
}
```
