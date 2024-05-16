# Android Documentation for the Lune PFM SDK

  

📦 An SDK to embed Lune enrichment views into your Android mobile apps

  

* * *

  

## Requirements

  

- [ ]     Android Studio Arctic Fox (2020.3.1) or later
- [ ]     Kotlin 1.5.10 or later
- [ ]     Minimum Android API level 21 (Android 5)
- [ ]     Lune SDK (AAR) file

  

* * *

  

## Installation Lune PFM android SDk

  

This document illustrates how our SDK can be integrated into your Android application in simple and easy steps. Please follow the steps to integrate the Lunedata Android SDK into your application.

1.  Add the following repository to your (root) <code>build.gradle</code><br/> 
``` groovy
repositories 
{ 
  ...
  mavenCentral()

}
```
2.  And add the following to your (app) <code>build.gradle</code> and make sure you are using our [Latest Version](https://github.com/Lune-Technologies/lune-pfm-sdk-android/tags) <br/> 
``` groovy 
dependencies 
{ 
  ... 
   implementation("io.lunedata.lunesdk:lunesdk:0.1.5") 
} 
```

3. Sync your project with the Gradle files to add the Line SDK to your project.

  

* * *

  

## Initialization

  

### Activities and Fragments:

  

To initialize the SDK within a project using Activities and Fragments, Place the fully qualified class name for Lune views, `io.lunedata.lunesdk.library.classes.LuneCompatManager`, as seen in the example below, in your layout file.

  

You are free to setup the layout constraints as you deem appropriate.

  

```xml
<!-- YourLayoutFile.xml -->

<io.lunedata.lunesdk.library.classes.LuneCompatManager
    android:id="@+id/luneLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    // other attributes...
/>
```

  

In your Activity or Fragment code, you need to create an instance of `LuneSDKManager` and initialise it with the required credentials.

  

```kotlin
// YourActivity.kt

private lateinit var luneSDK: LuneSDKManager;

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // initialise the SDK manager
    luneSDK = LuneSDKManager(
        baseUrl = "<your.base.url>",
        email = "<your.email.address>",
        password = "<your.password>",
        customerId = customer.id
    )

    // proceed to next step
}
```

  

Then, you need to specify which of the components you intend to render (`ActivityComponent`, in this case), and pass arguments if required.

  

To do this, find and interact with the views using their IDs (`R.id.luneLayout`, in this case) or by using `findViewById()` and set the component field, as seen in the example below.

  

```kotlin
// YourActivity.kt

private lateinit var luneSDK: LuneSDKManager;

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // initialise the SDK manager
    luneSDK = LuneSDKManager(
        baseUrl = "<your.base.url>",
        email = "<your.email.address>",
        password = "<your.password>",
        customerId = customer.id
    )

    // Grab our luneView and set the manager and component properties.
    val luneView = findViewById<LuneCompatManager>(R.id.luneLayout)

    luneView.manager = luneSDK

    luneView.component = LuneView.ActivityComponent
}
```

  

### Jetpack Compose:

  

To initialize the SDK within a Jetpack Compose project, you simply have to create an instance of `LuneSDKManager` in the `onCreate()` method of your main activity. This instance would be used across your app.

  

Jetpack Compose Tip:

  

While you could create multiple instances of `LuneSDKManager`, we recommend that you create just one instance per app.

You could share that single instance with other views by passing it around.

  

The SDK takes in necessary credentials as arguments to the `LuneSDKManager` constructor, as shown below.

  

```kotlin
import io.lunedata.lunesdk.library.classes.LuneSDKManager

class MainActivity : ComponentActivity() {

    private lateinit var luneSDK: LuneSDKManager;

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        luneSDK = LuneSDKManager(
            baseUrl = "<your.base.url>",
            token= "<your.access.token>",
            customerId = customer.id,
        )

        setContent {
            LuneBankTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                ) {
                    RootComposable(luneSDK)
                }
            }
        }
    }
}
```

  

You can use the `LuneSDKManager` instance within any composable in your app as shown below.

  

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

  

The views automatically use any customizations that have been set up within the `Resource files`.

  

  

* * *

  

# Lune Views

  

1. TransactionListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6344](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6344))
2. TransactionDetailComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6384](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6384))
3. BrandListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6424](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6424))
4. BudgetFormComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6404](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6404))
5. BudgetSummaryComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6364](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6364))
6. CategorySpendChartComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6504](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6504))
7. CategorySpendListComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6444](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6444))
8. CashflowComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6464](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6464))
9. BrandTrendsComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6484](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6484))
10. CategoryTrendsComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6544](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6544))
11. ExpenseComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6524](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6524))
12. BudgetComponent ([https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6564](https://doc.clickup.com/d/h/4d6uf-18504/58d4685bff940d1/4d6uf-6564))

  

* * *

  

# Customization

  

The SDK has lots of configurable parameters which can be overridden by setting up a `JSON` file with a schema similar to the one below.

You could just copy the example below and modify the values you wish to change.

  

```json
{
  "meta": {
    "name": "config",
    "version": "0.12"
  },
  "global": {
    "palette": {
      "ignore-dark-mode": false,
      "light": {
        "primary": "#002E2A",
        "secondary": "#469B93",
        "error": "#E86161",
        "success": "#4BB543",
        "warning": "#FFAA59",
        "accent": "#EDF5F4",
        "active": "#469B93",
        "hint": "#66827F",
        "foreground": "#002E2A",
        "background": "#FFFFFF",
        "card-background": "#FFFFFF",
        "success-background": "#EDF5F4",
        "error-background": "#FCEFEF",
        "warn-background": "#FFF5E3",
        "badge-foreground": "#FFFFFF",
        "badge-background": "#469B93",
        "switch-color": "#469B93"
      },
      "dark": {
        "primary": "#002E2A",
        "secondary": "#469B93",
        "error": "#E86161",
        "success": "#4BB543",
        "warning": "#FFAA59",
        "accent": "#EDF5F4",
        "active": "#B3C0BF",
        "selections": "#FFFFFF",
        "hint": "#B3C0BF",
        "foreground": "#FFFFFF",
        "background": "#151515",
        "card-background": "#2A292E",
        "success-background": "#EDF5F4",
        "error-background": "#FCEFEF",
        "warn-background": "#FFF5E3",
        "badge-foreground": "#FFFFFF",
        "badge-background": "#469B93",
        "switch-color": "#469B93"
      }
    },
    "typography": {
      "h0": {
        "font": "Poppins",
        "weight": 600,
        "size": 20,
        "line-height": 30
      },
      "h1": {
        "font": "Poppins",
        "weight": 600,
        "size": 16,
        "line-height": 24
      },
      "h2": {
        "font": "Poppins",
        "weight": 500,
        "size": 16,
        "line-height": 24
      },
      "body": {
        "font": "Poppins",
        "weight": 400,
        "size": 14,
        "line-height": 14
      },
      "hint": {
        "font": "Poppins",
        "weight": 400,
        "size": 12,
        "line-height": 12
      },
      "action": {
        "font": "Poppins",
        "weight": 400,
        "size": 16,
        "line-height": 24
      },
      "summary": {
        "font": "Poppins",
        "weight": 500,
        "size": 14,
        "line-height": 24
      }
    },
    "searchfields": {
      "label-padding": 12,
      "horizontal-padding": 16,
      "vertical-padding": 13,
      "prepend-search-icon": false,
      "radius": 10,
      "border": 1,
      "show-label": true,
      "outlined": false,
      "allow-emoji": true,
      "theme": {
        "light": {
          "foreground": "#000000",
          "background": "#F2F5F7",
          "focused-background": "",
          "outline": "#F2F5F7",
          "focused-outline": "#F2F5F7",
          "hint": "#69778C"
        },
        "dark": {
          "foreground": "#FFFFFF",
          "background": "#353636",
          "focused-background": "",
          "outline": "#353636",
          "focused-outline": "#353636",
          "hint": "#69778C"
        }
      }
    },
    "textfields": {
      "label-padding": 8,
      "horizontal-padding": 10,
      "vertical-padding": 10,
      "radius": 10,
      "border": 1,
      "show-label": true,
      "outlined": true,
      "allow-emoji": true,
      "theme": {
        "light": {
          "foreground": "#000000",
          "background": "",
          "focused-background": "",
          "outline": "#E6EAEA",
          "focused-outline": "#002E2A",
          "hint": "#69778C"
        },
        "dark": {
          "foreground": "#FFFFFF",
          "background": "",
          "focused-background": "",
          "outline": "#3E3E3E",
          "focused-outline": "#FFFFFF",
          "hint": "#69778C"
        }
      }
    },
    "buttons": {
      "radius": 70,
      "vertical-padding": 12,
      "border": 1,
      "outlined": false,
      "underline-actions": true
    },
    "cards": {
      "radius-small": 10,
      "radius-medium": 15,
      "radius-large": 15,
      "shadow": {
        "radius": 40,
        "offset-x": 0,
        "offset-y": 4
      }
    },
    "tiles": {
      "use-cards": true,
      "icon-width": 44,
      "icon-padding": 12,
      "icon-border-radius": 100,
      "horizontal-spacing": 10,
      "vertical-spacing": 10
    },
    "structure": {
      "prefix-amount-currency": false,
      "prefix-amount-sign": true,
      "show-amount-sign": true,
      "use-centered-header": false,
      "show-summary-icon": false,
      "highlight-summary-amount": true,
      "use-warn-color-for-summary": false,
      "override-header-color": false,
      "use-action-for-trends": false,
      "use-modal-header": false,
      "show-empty-state-action": true,
      "use-native-switch": false,
      "minify-title-decimal-value": false
    },
    "date-range-button": {
      "structure": {
        "radius": 10,
        "horizontal-padding": 15,
        "vertical-padding": 10,
        "icon-before-label": true
      },
      "theme": {
        "light": {
          "foreground": "#000000",
          "background": "#EDF5F4"
        },
        "dark": {
          "foreground": "#002E2A",
          "background": "#B3C0BF"
        }
      }
    },
    "assets": {
      "calendar": "lune_asset_calendar",
      "filter": "lune_asset_filter",
      "arrow-up": "lune_asset_arrow_up",
      "arrow-down": "lune_asset_arrow_down",
      "arrow-left": "",
      "arrow-right": "",
      "chevron-left": "",
      "chevron-right": "",
      "income": "lune_asset_income",
      "expense": "lune_asset_expenses",
      "check": "",
      "no-transaction": "lune_asset_no_data",
      "budget-feature": "lune_asset_budget_img",
      "goal-feature": "lune_asset_goal_img",
      "no-achieved-goal": "lune_asset_no_achieved_goal_img",
      "savings-feature": "lune_asset_savings_img",
      "no-data": "lune_asset_no_data",
      "summary-bad": "",
      "summary-warn": "",
      "summary-good": "",
      "feedback-success": "lune_asset_feedback_success"
    }
  },
  "component-specific": {
    "cashflow": {
      "structure": {
        "show-top-amount": false,
        "show-chart-center-amount": true,
        "show-chart-center-label": false,
        "chart-stroke-width": 10,
        "chart-stroke-offset": 0,
        "chart-round-stroke-cap": false,
        "alert-before-tiles": true,
        "income-before-expense": false
      },
      "theme": {
        "light": {
          "expense": "#7EB8B3",
          "income": "#1A433F"
        },
        "dark": {
          "expense": "#7EB8B3",
          "income": "#ffffff"
        }
      }
    },
    "expense": {
      "structure": {
        "show-top-amount": false,
        "use-tile-divider": false
      }
    },
    "budget-summary": {},
    "budget-form": {},
    "brand-trend": {
      "structure": {
        "icon-size": 17,
        "bar-padding": 7.5,
        "use-card-chart": true,
        "use-tile-divider": false,
        "chart-stroke-width": 6,
        "stroke-cap-radius": 0
      }
    },
    "category-trend": {
      "structure": {
        "icon-size": 17,
        "bar-padding": 7.5,
        "use-card-chart": true,
        "use-tile-divider": false,
        "chart-stroke-width": 6,
        "stroke-cap-radius": 0
      }
    },
    "activity": {},
    "category-activity": {},
    "transaction-detail": {
      "structure": {
        "logo-size": 44.0,
        "logo-border-radius": 100.0,
        "show-report-field-label": true,
        "use-centered-layout": false,
        "show-report-icon": true,
        "show-category-icon": true,
        "allow-cancel-report": true,
        "use-tile-divider-for-categories": false,
        "max-tag-length": 12,
        "show-time": false
      },
      "theme": {
        "light": {
          "reported-text": "#7E7D7D",
          "reported-bg": "#F0F0F0",
          "closed-text": "#E86161",
          "closed-bg": "#FCEFEF",
          "updated-text": "#469B93",
          "updated-bg": "#DEF1EF",
          "tag-text": "#FFFFFF",
          "tag-bg": "#637185"
        },
        "dark": {
          "reported-text": "#7E7D7D",
          "reported-bg": "#F0F0F0",
          "closed-text": "#E86161",
          "closed-bg": "#FCEFEF",
          "updated-text": "#469B93",
          "updated-bg": "#DEF1EF",
          "tag-text": "#FFFFFF",
          "tag-bg": "#637185"
        }
      }
    },
    "category-spend-chart": {
      "structure": {
        "show-chart-center-amount": true,
        "show-chart-center-label": false,
        "chart-stroke-width": 10,
        "chart-stroke-offset": 0,
        "chart-round-stroke-cap": false,
        "use-cards-for-annotation": true,
        "annotation-border-radius": 100
      },
      "theme": {
        "light": {
          "segment-colors": [
            "#102D2A",
            "#3B5755",
            "#5D9993",
            "#9AC0BE",
            "#CCE0DF"
          ],
          "annotation-bg": "#FFFFFF"
        },
        "dark": {
          "segment-colors": [
            "#102D2A",
            "#3B5755",
            "#5D9993",
            "#9AC0BE",
            "#CCE0DF"
          ],
          "annotation-bg": "#000000"
        }
      }
    },
    "brand-list": {},
    "transaction-list": {
      "structure": {
        "show-trailing-rounded-indicator": true,
        "show-trailing-category-icon": true,
        "show-time": false
      }
    },
    "category-spend-list": {
      "category-budget-increment": 50
    }
  }
}



```

  

The `JSON` file should be saved as `lune_config.json` and added to the `/res/raw` directory of your Android project. In the end the file should be in located at `/res/raw/lune_config.json`.

  

Note:

  

Any fonts mentioned in the config file should be added to the project as well, within the `/res/font` directory.

e.g

  

`|- LuneBank`

`|- - app`

`|- - | - - src`

`|- - | - - | - - main`

`|- - | - - | - - | - - res`

`|- - | - - | - - | - - | - - font`

`|- - | - - | - - | - - | - - | - - Poppins.ttf` 👈🏽

`|- - | - - | - - | - - | - - raw`

`|- - | - - | - - | - - | - - | - - lune_config.json` 👈🏽

  

* * *

  

# Content Override

  

## Images

You can override any of the images below by simply giving any other images in your project the exact same name:

  

```yaml
# format: "lune_asset_<image_name>"

lune_asset_budget_img
lune_asset_arrow_down
lune_asset_arrow_left
lune_asset_arrow_right
lune_asset_arrow_up
lune_asset_calendar
lune_asset_checked
lune_asset_expenses
lune_asset_filter
lune_asset_goal_img
lune_asset_income
lune_asset_no_achieved_goal_img
lune_asset_no_data
lune_asset_no_transaction
lune_asset_savings_img
lune_asset_summary_bad
lune_asset_summary_good
lune_asset_savings_img
lune_asset_feedback_success
lune_asset_chevron_right
lune_asset_chevron_left
lune_asset_add
lune_asset_round
lune_asset_report
lune_asset_search
lune_asset_snack_success
lune_asset_trend
lune_asset_upload
lune_asset_over_spend
lune_asset_checkbox_checked
lune_asset_checkbox_unchecked
lune_asset_close
lune_asset_delete
lune_asset_file_upload_error
lune_asset_leftover
lune_asset_other_category
lune_asset_miscellaneous_brand
lune_asset_brand_logo_fallback



```

  

## Strings

In the same way, you can override any localized strings by setting up your strings with the exact same key as used in our localization files.

  

  

* * *

  

# Localization

  

If your app is already localized, the SDK would be localized as well - no configurations needed. If your app is not localized, however, the SDK respects that and stays in English to preserve consistency and uniformity across your app.

  

  

* * *

# Refresh Token

  

Upon initialization of the SDK, you should setup a callback function that would be called to refresh user tokens when they expire.

  

```kotlin
class TokenRefresher: RefreshTokenCallback {
    override fun refreshToken(): String? {
    // Implement logic to refresh the token and obtain a new access token
    // Return the new access token or null if refresh fails
     return "" // make sure to return the token here  
    }
}

// other init code you might already have with the token
luneSDK = LuneSDKManager(
    baseUrl = "",
    token =  "", // the access token for the user
    customerId = id, // the customer id
    refreshTokenCallback = TokenRefresher()
)


//you can set up the callback at a later stage like this...
luneSDK.setupRefreshCallback(callback = TokenRefresher())


```

  

  

  

  

  

  

  

## Optional

  

1. To allow buttons to float above the keyboard, add the following attribute in your `AndroidManifest.xml` file:

  

```xml
<activity android:windowSoftInputMode="adjustResize">
    <!--this allows buttons to float above the keyboard-->
</activity>
```
