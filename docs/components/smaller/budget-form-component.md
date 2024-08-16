# Budget Form Component

![](../assets/b34378f73d1686bf0b94543877f669845223d183.png)

The `BudgetFormComponent` allows the user to set up a new budget. To use
this view, just call the `BudgetFormComponent` method of your
`LuneSDKManager` instance as shown in the example below.

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

To use this view in a project with Activities and Fragments, set the
`component` property of your view to `LuneView.BudgetFormComponent`, as
shown in the example below.

```kotlin
// YourActivity.kt

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Grab our luneView and set the component property.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)
    luneView.component = LuneView.BudgetFormComponent
}
```
