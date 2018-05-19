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

`package MY.APP.NEW_ID;`

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



## Generate Signed Build

### iOS

* You will need to have an active [Apple Developer Membership](https://developer.apple.com/)
* Your app's bundle ID is registered in [iTunes Connect](https://itunesconnect.apple.com/)
* Change the `Bundle Identifier` in project inspector
* Change the `Team` in `Signing` area and check out `Automatically manage signing`


### Android

Checkout Reference from [Facebook](https://facebook.github.io/react-native/docs/signed-apk-android.html).

* Step 1

  ```keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000```

* Step 2

  Save the generated `my-release-key.keystore` file to `android/app ` in your React Native project

* Step 3

  In `android/gradle.properties` , add following lines:

  ```MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
    MYAPP_RELEASE_KEY_ALIAS=my-key-alias
    MYAPP_RELEASE_STORE_PASSWORD=*****
    MYAPP_RELEASE_KEY_PASSWORD=*****```
* Step 4
  
  Edit `android/app/build.gradle` file to add following code.

  ```android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_RELEASE_STORE_FILE')) {
                storeFile file(MYAPP_RELEASE_STORE_FILE)
                storePassword MYAPP_RELEASE_STORE_PASSWORD
                keyAlias MYAPP_RELEASE_KEY_ALIAS
                keyPassword MYAPP_RELEASE_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
  }
  ...
  ```

  Basically, you add `signingConfigs` block into the `android` block and you add `signingConfig signingConfigs.release` into `release` block under `buildTypes`

* Step 5

  Run following command in your Terminal

  `cd android && ./gradlew assembleRelease`

* Step 6

  You can find your `app-release.apk` in path `android/app/build/outputs/apk`.

  Notes: You can use following alias in your bash or zsh on macOS.

  `alias getApk=cd android && ./gradlew assembleRelease && open app/build/outputs/apk && cd ..`

  By doing so, next time you can just run `getApk` in your React Native folder and get your file directly.