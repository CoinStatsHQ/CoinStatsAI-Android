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
    implementation 'coinstats.ai:CoinStatsAI-Android:1.0.44' // See gradle.properties for latest version
}
```

For the latest version, check `gradle.properties` in the project root.

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

### Opening the AI View

```kotlin
CoinStatsAICore.open(activity)
```

### Customizing Questions

Questions are displayed as a horizontal pager with mode badges. The library automatically handles:
- Question loading and display
- Mode badge rendering (with theme support)
- Badge updates as user scrolls through questions

### Theme Support

The library respects your app's theme (light/dark mode) and automatically applies appropriate colors to all UI elements.

## Download Functionality

The library automatically handles file downloads when users interact with download links in the WebView:

1. **Regular URLs**: Files are downloaded to cache and shared via the system share sheet
2. **Blob URLs**: Handled natively by the browser using anchor tag downloads
3. **Progress**: Visual progress overlay during download
4. **Cleanup**: Files are auto-deleted 1 minute after sharing

## Troubleshooting

### Downloads Not Working

1. Verify FileProvider is declared in your manifest
2. Check that file_provider_paths.xml exists and contains cache paths
3. Ensure INTERNET permission is granted
4. Verify sufficient storage space is available

### "Couldn't find meta-data for provider..."

This error indicates FileProvider configuration issue:
1. Verify `android:authorities` matches your package name
2. Confirm `@xml/file_provider_paths` points to correct file
3. Ensure XML file exists and is valid

## Dependencies

- `androidx.core:core` - FileProvider
- `com.squareup.okhttp3:okhttp` - HTTP client for downloads
- `androidx.webkit:webkit` - WebView enhancements
- `androidx.recyclerview:recyclerview` - Question pager
- Kotlin coroutines

## License

Licensed under MIT License. See LICENSE file for details.