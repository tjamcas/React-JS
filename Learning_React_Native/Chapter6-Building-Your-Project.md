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
- Before we publish the ColorCatalog app, we will configure `app.json` to customize the "splash" and "icon" files, and add a 'description" key.
  - The `app.json` file is used by Expo when it builds your application and it contains important configuration details about the application. So within this file we have keys for `icon` and `splash` that we change to `cc-splash` and `cc-icon`.
  - We can add a description: "Catalog your colors. Save your favorite web colors and view the details."
- To publish our application, 
  - /1. Open a terminal window within VS Code. 
  - /2. Using the Expo CLI, type in the shell command `expo publish`.
    -  Expo will build your iOS JavaScript bundle, your Android JavaScript bundle, and place all of your assets online. 
    -  To access the online assets, go to the URL for the "project page" as provided by Expo in the terminal shell. For example: `https://expo.dev/@expo_username/ColorCatalog?serviceType=classic&distribution=expo-go` where `expo_username` is the developer's user name.
  -  These steps publish/host our bundle online with Expo. When we make changes to the app, we simply republish the bundle and those changes will automatically be pushed to everyone's device.
