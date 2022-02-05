# Placid Android SDK

Generate on-brand social media images automatically with [Placid](https://placid.app).

[![](https://jitpack.io/v/com.github.placidapp/android-sdk.svg)](https://jitpack.io/#com.github.placidapp/android-sdk)

## Requirements

- minSdk 21


## Installation

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

Now, add the Placid dependeny to your application level .gradle file:
```groovy
dependencies {
    ...
    implementation 'com.github.placidapp:android-sdk:x.y.z'
}
```
(Please replace ```x.y.z``` with the latest version numbers: ([![](https://jitpack.io/v/com.github.placidapp/android-sdk.svg)](https://jitpack.io/#com.github.placidapp/android-sdk))


## Usage

### Configuration

Configure the Placid SDK in your `Application` class with:
```kotlin
PlacidSDK.configure(
    licenseKey = "placid-abc-xyz",
    fileName = "placid_templates.placid",
)
```
Use the optional parameter 'forceReload' in order to enforce the extraction of the Placid Bundle at every app start. By default this only happens when the app was updated (versionCode increased).
Additionally you can optionally pass in a logger function in order to receive error logs regarding your configuration.

### Retrieving Templates

Templates can be retrieved via their unique identifier (as found in the placid web app). If the template is not found, this method will return null.
```kotlin
val template = PlacidSDK.getTemplate("abcxyz")
```

Alternatively you can also retrieve the template asynchronously. This might be useful if you need it early at app start where the initializiaton of the SDK might not be completed yet.
```kotlin
val template = PlacidSDK.getTemplateAsync("abcxyz")
```

### Modify data

Templates can be customized using their specific layers.

:warning: The same layer name as in the web-editor has to be used, otherwise the layer will be ignored. 

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
template.imageLayer("avatar") {
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

### General Properties

These properties are supported by all layer types.

```kotlin
template.textLayer("title") {
    isHidden = false
    origin = Point(0,0)
    size = Size(100, 50)
}
```

The mobile SDK supports the same layers and properties as the [API](https://placid.app/docs/2.0/rest/layers). So have a look for the full set of supported functions and properties.

### Render Images
Once all data is added to the specific template, it can be rendered to a native bitmap.

```kotlin
viewModelScope.launch {
    val bitmap = template.render() //suspended function
}
```

### Chaining calls

You can chain the calls to your template like this:

```kotlin
val bitmap = template.textLayer("title") {
    text = "New Title"
}.imageLayer("image") {
    image = bitmap
}.render()
```


## Support

For bug reports, please create a new Issue right here on Github. Otherwise have a look at our [Help section](https://placid.app/help)
