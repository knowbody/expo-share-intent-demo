# Expo Share Intent Demo

This project demonstrate a fonctionnal share intent in a Expo (React Native) project. It allows to use an expo auto configuration for react-native-receive-sharing-intent package.

This demo works with old **Expo SDK 47** and is for Android and iOS, using url and text sharing.

More Demo :

- Expo 46 [available here](https://github.com/achorein/expo-share-intent-demo/tree/expo46) (compatible iOS 12.4)
- Expo 48 [available here](https://github.com/achorein/expo-share-intent-demo/tree/expo48)
- Expo 49 [available here](https://github.com/achorein/expo-share-intent-demo/tree/expo49)

## Getting Started

```
yarn install
yarn prebuild
yarn ios
yarn android
```

## How

For that we use and patch two projects :

- https://github.com/ajith-ab/react-native-receive-sharing-intent
- https://github.com/timedtext/expo-config-plugin-ios-share-extension

we are using `patch-package` to auto patch, see "patches" directory for details.

### iOS Tricks

Thanks to `expo-config-plugin-ios-share-extension` package we do not need to do manual configuration as described is the original [doc](https://ajith-ab.github.io/react-native-receive-sharing-intent/docs/ios)

### Android Tricks

For Android build we call a patch after the expo prebuild command execution in the `package.json` scripts, this automate the [doc instruction](https://ajith-ab.github.io/react-native-receive-sharing-intent/docs/android/) :

```json
"scripts": {
  "prebuild": "expo prebuild --no-install && patch -s -p0 < plugins/share-extension-patch-android.diff"
}
```

we also use some plugins for updating the manifest (see app.json).

plugins

```json
    "plugins": [
      "expo-config-plugin-ios-share-extension",
      [
        "expo-build-properties",
        {
          "android": {
            "kotlinVersion": "1.6.10",
            "compileSdkVersion": 33,
            "targetSdkVersion": 33,
            "buildToolsVersion": "33.0.0"
          }
        }
      ],
      [
        "./plugins/withAndroidMainActivityAttributes",
        {
          "android:windowSoftInputMode": "adjustResize"
        }
      ]
    ]
```

manifest

```json
    "android": {
      "intentFilters": [
        {
          "action": "SEND",
          "category": "DEFAULT",
          "data": [
            {
              "mimeType": "text/*"
            }
          ]
        }
      ]
    },
```
