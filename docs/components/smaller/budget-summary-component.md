# Budget Summary Component

![](../assets/9cee5adc4129f8c8cbad873aa8ed3f9aaf4c8580.png)

The `BudgetSummaryComponent` shows the user's overall spend in contrast
to his budget and his expected

spend per time. To use this view, just call the `BudgetSummaryComponent`
method of

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

To use this view in a project with Activities and Fragments, set the
`component` property of your view to `LuneView.BudgetSummaryComponent`,
as shown in the example below.

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BudgetSummaryComponent
}
```
