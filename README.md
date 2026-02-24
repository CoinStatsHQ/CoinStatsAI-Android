# CoinStats AI Library

An Android library that integrates AI-powered features into the CoinStats app, featuring a WebView-based interface with AI question suggestions, multiple operation modes, and file download capabilities.

## Features

- **AI WebView Interface**: Embedded WebView for AI interactions
- **Question Suggestions**: Pre-defined question suggestions with mode indicators
- **Multi-Mode Support**: Fast, Agentic, and Backtest modes with visual badges
- **File Downloads**: Download files from WebView with automatic sharing
- **Blob URL Support**: Native browser handling for blob URLs
- **Dark/Light Theme**: Full theme support with glassmorphic UI elements
- **Progress Tracking**: Visual feedback during downloads
- **Automatic Cleanup**: Cache management with auto-deletion of old files

## Installation

Add this library to your project's `build.gradle`:

```gradle
dependencies {
    implementation 'coinstats.ai:CoinStatsAI-Android:1.0.47'
}
```

## Setup

### File Download Setup

The library supports file downloads from the WebView. To enable this feature, you must configure a FileProvider in your app.

#### Configure FileProvider

Add to your app's `AndroidManifest.xml`:

```xml
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="com.yourcompany.yourapp"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_provider_paths" />
</provider>
```

Replace `com.yourcompany.yourapp` with your actual package name.

#### Create FileProvider Paths Configuration

Create a file at `res/xml/file_provider_paths.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- Cache directories for AI downloads -->
    <external-cache-path
        name="ai_downloads"
        path="ai_downloads/" />

    <cache-path
        name="ai_downloads_internal"
        path="ai_downloads/" />
</paths>
```

If you already have a FileProvider configured, simply add the cache-path entries to your existing `file_provider_paths.xml`.

## Usage

### Public API

| Method | Description |
|--------|-------------|
| `initialize(apiKey, isDebugMode, isDarkTheme)` | Configure the SDK at app startup |
| `create(activity, token, margins, ...)` | Set up the floating AI button in an Activity |
| `isEnabled` | Property to show/hide the floating AI button |
| `prepare(token)` | Update the authentication token |
| `sendWindowContext(context, metadata)` | Send contextual information to the AI |
| `open(activity, queryParams)` | Open the AI WebView programmatically |
| `setTheme(activity, isDark)` | Update the theme at runtime |
| `syncUserData()` | Sync user data with the WebView |
| `dismissQuestionsIfShown()` | Dismiss the questions panel if visible |
| `disableBlur(activity)` | Disable blur effect before fragment transactions |
| `enableBlur(activity)` | Re-enable blur effect after fragment transactions |

### Initialize

Call once at app startup (e.g., in your Application class):

```kotlin
CoinStatsAICore.initialize(
    apiKey = "your-api-key",
    isDebugMode = BuildConfig.DEBUG,
    isDarkTheme = true
)
```

### Create

Set up the floating AI button in your Activity:

```kotlin
CoinStatsAICore.create(
    activity = this,
    token = userAuthToken,
    margins = BoundsMargins(left = 16f, top = 100f, right = 16f, bottom = 100f),
    hideListener = { /* Called when user hides the button */ },
    aiClickListener = { /* Called when AI button is clicked */ },
    aiCloseListener = { /* Called when AI WebView is closed */ },
    deepLinkOpenListener = { uri -> LinkHandling.BROWSER }
)
```

### isEnabled

Enable or disable the floating AI button:

```kotlin
CoinStatsAICore.isEnabled = true  // Show
CoinStatsAICore.isEnabled = false // Hide
```

### prepare

Update the authentication token (e.g., after login/logout):

```kotlin
CoinStatsAICore.prepare(newToken)
```

### sendWindowContext

Send contextual information to the AI:

```kotlin
CoinStatsAICore.sendWindowContext(context = "coin", metadata = """{"coinId": "bitcoin"}""")
```

### open

Open the AI WebView programmatically:

```kotlin
CoinStatsAICore.open(activity)
CoinStatsAICore.open(activity, mapOf("source" to "deeplink"))
```

## Download Functionality

The library automatically handles file downloads when users interact with download links in the WebView:

1. **Regular URLs**: Files are downloaded to cache and shared via the system share sheet
2. **Blob URLs**: Handled natively by the browser using anchor tag downloads
3. **Progress**: Visual progress overlay during download
4. **Cleanup**: Files are auto-deleted 1 minute after sharing

## License

Licensed under MIT License. See LICENSE file for details.