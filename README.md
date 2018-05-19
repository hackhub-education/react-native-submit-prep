# Preparing for project submission


## Icon

For iOS, you need an icon that's at least 1024 x 1024 pixels. 
For Android, you will need an icon that's at least 512 x 512 pixels.

Thus it's highly recommended that you have icons in vector files or at least file at the size of 1024x1024.

### iOS

Recommended software to help you with icons: [Prepo](https://itunes.apple.com/ca/app/prepo/id476533227?mt=12)


### Android
Recommended tool: [AndroidAssetStudio](https://romannurik.github.io/AndroidAssetStudio/icons-launcher.html#foreground.type=image&foreground.space.trim=1&foreground.space.pad=0.05&foreColor=rgba(96%2C%20125%2C%20139%2C%200)&backColor=rgb(96%2C%20125%2C%20139)&crop=0&backgroundShape=square&effects=none&name=ic_launcher)



## Bundle ID / Package name

### iOS

Open `<project>.xcodeproj` from `./ios` folder.

* Choose your project in the navigation area (the left panel)
* Change **Bundle Identifier** in `identity` area.

<img src='https://github.com/webdxd/react-native-submit-prep/blob/master/img/bundle_id.png?raw=true' alt='Screenshot Bundle ID' />

### Android

Changed project' subfolder name from: `"android/app/src/main/java/MY/APP/OLD_ID/"` to: `"android/app/src/main/java/MY/APP/NEW_ID/"`

Then manually switched the old and new package ids:

In: *android/app/src/main/java/MY/APP/NEW_ID/MainActivity.java:*

package MY.APP.NEW_ID;
In *android/app/src/main/java/MY/APP/NEW_ID/MainApplication.java*:

`package MY.APP.NEW_ID;`

In *android/app/src/main/AndroidManifest.xml*:

`package="MY.APP.NEW_ID"`

And in *android/app/build.gradle*:

`applicationId "MY.APP.NEW_ID"`

**(Optional)** In *android/app/BUCK*:

```
android_build_config(
  package="MY.APP.NEW_ID"
)
android_resource(
  package="MY.APP.NEW_ID"
)
```

Gradle' cleaning in the end (in /android folder):

`./gradlew clean`
