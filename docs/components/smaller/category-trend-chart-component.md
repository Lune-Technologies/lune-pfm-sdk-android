# Category Trend Chart Component

![](../assets/d695c154638767f9db3832c3881524c8144fd1c4.png)

The `CategoryTrendChartComponent` shows the user's spend across
different categories in a chart.

To use this view, call the `CategoryTrendChartComponent` method of your
`LuneSDKManager` instance as shown in the example below. You can pass in
optional `startDate` and `endDate` arguments to filter the data shown.

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

To use this view in a project with Activities and Fragments, set the
`component` property of your view to
`LuneView.CategoryTrendChartComponent`, as shown in the example below.

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
