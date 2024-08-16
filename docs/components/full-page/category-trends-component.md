# Category Trends Component

![](../assets/85e35dd2b7a2cb9c57ffbbb9e205004ccdade7f3.png)

The `CategoryTrendsComponent` shows the user's spend across different
categories both in a chart, and

in a list of category tiles. It allows users to filter data by dates
too.

To use this view, just call the `CategoryTrendsComponent` method of your
`LuneSDKManager` instance as

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

To use this view in a project with Activities and Fragments, set the
`component` property of your view to `LuneView.CategoryTrendsComponent`,
as shown in the example below.

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategoryTrendsComponent
}
```

---

## Localization Keys and Analytics

![](../assets/de298534f10b99cfdbb97372b87d3e4325db3650.png)

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

---

![](../assets/8fe16b71594005405f953120c7b7ae7cc4a82715.png)

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
