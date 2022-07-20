# Learning React Native
## Chapter 6: Building Your Project
### Video 1: Installing an IOS Simulator
- To install an IOS simulator on a Mac, 
  - /1. first, install the XCode developer tools on the Mac using the App Store application.
  - /2. Open your application in Visual Studio, e.g. ColorCatalog application, open a terminal window pane and run `npm start`. 
  - /3. When the Expo Bundler starts and the tunnel is up and running, click on button "Run on IOS simulator". Xcode will then find your default IOS simulator and run the current project within that simulator.

### Video 2: Installing an an Android AVD for Mac
- To install an Android virtual device on a Mac, 
  - /1. first, to install Android Studio, which is an IDE for developing Java applications that run on the Android platform, do a quick Google search for Android Studio. The first link should be to download Android Studio and SDK tools at <https://developer.android.com/studio>.
  - /2. Run the Android Studio .dmg file in the `Downloads` folder, and then open Android Studio
  - /3. Create a new device -- in this case we will create a Pixel XL device running version R software.
  - /4. When the Expo Bundler starts and the tunnel is up and running, click on button "Run on Android device/emulator". Xcode will then find your default Android virtual device and run the current project within that simulator.

### Video 4: Publishing your Expo Project
- Before we publish the ColorCatalog app, we will configure `app.json` to customize the "splash" and "icon" files, and add a "description" key.
  - The `app.json` file is used by Expo when it builds your application and it contains important configuration details about the application. In the file we change the values of the `icon` and `splash` keys to `cc-splash` and `cc-icon`.
  - We add a value to the "description" key in the file: "Catalog your colors. Save your favorite web colors and view the details."
  - Here is the full `app.json` file:
    ```
    {
      "expo": {
        "name": "ColorCatalog",
        "slug": "ColorCatalog",
        "description": "Catalog your colors. Save your favorite web colors in a list and view the details.",
        "version": "1.0.0",
        "platforms": ["ios", "android"],
        "orientation": "portrait",
        "icon": "./assets/cc-icon.png",
        "userInterfaceStyle": "light",
        "splash": {
          "image": "./assets/cc-splash.png",
          "resizeMode": "contain",
          "backgroundColor": "#ffffff"
        },
        "updates": {
          "fallbackToCacheTimeout": 0
        },
        "assetBundlePatterns": [
          "**/*"
        ],
        "ios": {
          "supportsTablet": true
        },
        "android": {
          "adaptiveIcon": {
            "foregroundImage": "./assets/adaptive-icon.png",
            "backgroundColor": "#FFFFFF"
          }
        },
        "web": {
          "favicon": "./assets/favicon.png"
        }
      }
    }
    ```
- To publish our application, 
  - /1. Open a terminal window within VS Code. 
  - /2. Using the Expo CLI, type in the shell command `expo publish`.
    -  Expo will build your iOS JavaScript bundle, your Android JavaScript bundle, and place all of your assets online. 
    -  To access the online assets, go to the URL for the "project page" as provided in the message from Expo in the terminal shell. For example: `https://expo.dev/@expo_username/ColorCatalog?serviceType=classic&distribution=expo-go` where `expo_username` is the developer's user name.
  -  These steps publish/host our bundle online with Expo. When we make changes to the app, we simply republish the bundle and those changes will automatically be pushed to everyone's device.

### Video 5: Building for IOS Devices
- At present we are not running the app in a standalone mode on our device or the Apple IOS device simulator. Instead, we are running Expo which fires off the ColorCatalog app.
- We want build the App and run it stanalone on a device or simulator. To do this, we need to distribute the ColorCatalog app through the Apple App Store.
- To distribute the app through the App Store, we need to:
  - Add some build details key-value pairs into the `app.json` file:
    - Add the build identifier key-value pair which is a unique identifier for the app:   
      `"bundleIdentifier": "com.moonhighway.color-organizer",`
      - Note that the `bundleIdentifier` value takes the form: `domain.organizationName.appName`
    - Add the IOS build number:   
      `"buildNumber": "1.0.0",`
    - Here is the revised `app.json` file:
      ```
      {
        "expo": {
          "name": "HelloWorld",
          "description": "Catalog your colors. Save your favorite web colors and view the details",
          "slug": "ColorCatalog",
          "platforms": ["ios", "android"],
          "version": "1.0.0",
          "orientation": "portrait",
          "icon": "./assets/cc-icon.png",
          "splash": {
            "image": "./assets/cc-splash.png",
            "resizeMode": "contain",
            "backgroundColor": "#ffffff"
          },
          "updates": {
            "fallbackToCacheTimeout": 0
          },
          "assetBundlePatterns": ["**/*"],
          "ios": {
            "supportsTablet": true,
            "bundleIdentifier": "com.moonhighway.color-organizer",
            "buildNumber": "1.0.0"
          }
        }
      }
      ```
  - Open a terminal window in the App's development folder and direct Expo to run the build:   
    `expo build:ios`
    - Expo is using Xcode in the iOS SDKs to build the application on their servers (so that you don't have to)
  - Submit the app to the App Store using your Apple App Store account. During the build process, Expo links your developer account to this build.
    - During the build process, Expo will ask whether you have a developer account to link this build. Say yes, and enter your Apple developer ID and password. Now Expo will talk to the Apple developer portal on your behalf. 
    - After responding to the Expo dialog questions, and entering the requested information, Expo queues a build for an iOS bundle, and provides build status/progress messages. For further Expo build status, click on the link that Expo provides when you submit the build request. This provides specific details about the current build, including the associated logs.
    - You will be offered the option to let Expo manage the process of building service keys for push notifications. We don't have any in the ColorCatalog app, but you can let Expo manage that process in case you want to add them in the future. 
    - You can set up your Apple provisioning on your own, but it's easier to let Expo manage that process.
    - If you kill the terminal after starting the build, then re-open a terminal window and type: `expo build:status` to find out the build status.
  - Download the zipped file that Expo built, and unpack it
    - In the terminal window, go to the `downloads` folder, and type:   
      `tar -xvcf tarFileName.tar.gz`
    - if you list your files, then you should find a file of the form `AppName.app` -- e.g. `ColorCatalog.app`
  - To install the app on your simulator, type    
    `xcrun simctl install booted AppName.app`

### Video 6: Building for Android Devices
- At present we are not running the app in a standalone mode on the Android simulator. Instead, we are running Expo which fires off the ColorCatalog app in the Android simulator.
- We can use Expo to build our application for Android devicesso that we don't have to install Android Studio, or the Android STK, to build an application that you're ready to distribute to the Google Play Store.
- To distribute the app through the Google Play Store, we need to:
  - Add an Android `package` key-value pair into the `app.json` file:
    ```
    {
      "expo": {
        "name": "HelloWorld",
        "description": "Catalog your colors. Save your favorite web colors and view the details",
        "slug": "ColorCatalog",
        "platforms": ["ios", "android"],
        "version": "1.0.0",
        "orientation": "portrait",
        "icon": "./assets/cc-icon.png",
        "splash": {
          "image": "./assets/cc-splash.png",
          "resizeMode": "contain",
          "backgroundColor": "#ffffff"
        },
        "updates": {
          "fallbackToCacheTimeout": 0
        },
        "assetBundlePatterns": ["**/*"],
        "ios": {
          "supportsTablet": true,
          "bundleIdentifier": "com.moonhighway.color-organizer",
          "buildNumber": "1.0.0"
        },
        "android": {
          "package": "com.moonhighway.color-organizer"
        }
      }
    }
    ```
  - Open a terminal window in the App's development folder and direct Expo to run the build:   
    `expo build:android`
    - After responding to the Expo dialog questions, and entering the requested information, Expo queues a build for an iOS bundle, and provides build status/progress messages. For further Expo build status, click on the link that Expo provides when you submit the build request. This provides specific details about the current build, including the associated logs.
    - Once the build has finished, we are given a link to our application. We can go ahead and click on this Expo artifacts link and download our application. That will automatically download the APK to the Downloads directory for me. So I'll go ahead and go to the Downloads directory. So in here we can see that we have our APK file. I can go ahead and run the open dot command, `open .`, in the terminal window. This opens the folder within Finder, so we can see the `.apk` file. To install the `.apk` file in an Android virtual device, simply drag the file over the device. So by dragging and dropping this APK into our Android virtual device, it is installing our standalone application. Once installed, I can view my application.

### Video 7: Ejecting from Expo
- 
