![Placid.app Android SDK](https://user-images.githubusercontent.com/4189032/155292903-cc64e794-c04e-4a94-b9e7-cd52b7582c8f.gif)

<div align="center">
  <h1>Placid Android SDK</h1>
  <strong>Generate custom share images on-device</strong>
  <br /><br />

<p align="center">
  <a href="https://jitpack.io/#com.github.placidapp/android-sdk"><img src="https://jitpack.io/v/com.github.placidapp/android-sdk.svg" /></a>
  <a href="#"><img src="https://img.shields.io/badge/minSdk-21-lightblue" /></a>
</p>

</div>

[Placid](https://placid.app) is a toolkit for creative automation. It lets you generate images with dynamic content from custom templates - e.g. for personalized share visuals. The Placid Android SDK offers native, on-device image generation for your apps.

* Offline, on-device image generation
* Custom templates for on-brand visuals
* Drag & drop template editor
* Dynamic content layers
* Auto-resizing for text and images
* Unlimited generated images
* Dynamic in-app previews

➡️ [Learn more about the Placid Mobile SDK](http://placid.app/solutions/mobile-sdk)

---

## ⚙️ Requirements

- minSdk 21

## 📦️ Installation

Add the jitpack maven repository to your top level .gradle file:

```groovy
allprojects {
    repositories {
        ...
        maven {
            url 'https://jitpack.io'
        }
    }
}
```

Now, add the Placid dependency to your application level .gradle file:
```groovy
dependencies {
    ...
    implementation 'com.github.placidapp:android-sdk:x.y.z'
}
```
(Please replace ```x.y.z``` with the latest version numbers: ([![](https://jitpack.io/v/com.github.placidapp/android-sdk.svg)](https://jitpack.io/#com.github.placidapp/android-sdk))

## ✨ Usage

### Setup on placid.app

1. Register on [placid.app](https://placid.app) (✅ free trial)
2. Create a project and add the Placid mobile integration
3. Add a template
4. Add a mobile license in your project settings
5. Download your template(s) to use with the SDK

### Configuration

Configure the Placid SDK in your `Application` class by providing your license key and the raw resource id of your Placid bundle that you downloaded from the Placid web app:

```kotlin
PlacidSDK.configure(
    licenseKey = "your-placid-license-key",
    bundleResId = R.raw.placid_templates,
)
```
Use the optional parameter 'forceReload' in order to enforce the extraction of the Placid Bundle at every app start. By default this only happens when the app was updated (versionCode increased).
Additionally you can optionally pass in a logger function in order to receive error logs regarding your configuration.

### Retrieve Templates

Templates can be retrieved via their unique identifier (as found in the Placid web app). If the template is not found, this method will return null.
```kotlin
val template = PlacidSDK.getTemplate("abcxyz")
```

Alternatively you can also retrieve the template asynchronously. This might be useful if you need it early at app start where the initializiaton of the SDK might not be completed yet.
```kotlin
val template = PlacidSDK.getTemplateAsync("abcxyz")
```

### Modify Layer Data

You can modify the content and appearance of dynamic layers in your template.

:warning: Use the layer names as specified in the template editor, otherwise the layer will be ignored.

#### Text

```kotlin
template.textLayer("title") {
    text = "A new title"
    textColor = Color.RED
}
```

#### Images

```kotlin
val newBitmap : Bitmap = ...
template.pictureLayer("avatar") {
    bitmap = newBitmap
}
```

#### Rectangles

```kotlin
template.rectangleLayer("rect") {
    backgroundColor = Color.RED
    borderColor = Color.BLUE
}
```

#### Browser Frames

```kotlin
template.browserFrameLayer("browser") {
    url = "www.domain.com"
    image = "https://domain.com/image.jpg"
}
```

#### General Properties

These properties are supported by all layer types.

```kotlin
template.textLayer("title") {
    isHidden = false
    origin = Point(0,0)
    size = Size(100, 50)
}
```

The mobile SDK supports the same layers and properties as the [Placid REST API](https://placid.app/docs/2.0/rest/layers), so have a look for the full set of supported functions and properties.

### Render Images

Once all data is added to the template, it can be rendered to a native bitmap. Use it in your `ImageView`s for dynamic in-app views or pass it along for social sharing.

```kotlin
viewModelScope.launch {
    val bitmap = template.renderImage() //suspended function
}
```

### Chaining calls

You can chain the calls to your template like this:

```kotlin
val bitmap = template.textLayer("title") {
    text = "New Title"
}.pictureLayer("image") {
    image = bitmap
}.renderImage()
```

## 💬 Support

For bug reports, please create a new Issue right here on Github. Otherwise have a look at our [Help section](https://placid.app/help)
