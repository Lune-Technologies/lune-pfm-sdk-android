# Category Spend List Component

![](../assets/1dfe5503de9760b54c9405cb5d8ca0d3385ffc15.png)

The `CategorySpendListComponent` shows the user's overall spend in
contrast to his budget and his

expected spend per time. To use this view, just call the
`CategorySpendListComponent` method of

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

To use this view in a project with Activities and Fragments, set the
`component` property of your view to
`LuneView.CategorySpendListComponent`, as shown in the example below.

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.CategorySpendListComponent
}
```
