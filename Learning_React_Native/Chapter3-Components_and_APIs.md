# Learning React Native
## Chapter 3: Components and API's
### Video 1: Native Components
- React Native comes with a library of UI components that we can immediately use to build applications.
  - We've used the `<View />` and `<Text />` components
  - The React Native API library also includes:
    - `<ActivityIndicator />` - the spinning wheel icon
    - `<ProgressViewIOS />` - a progress bar for IOS devices
    - `<Button />` - a clickable button
      - To make a button active, you must define a function that is called with the `onPress` property - see the example below
  - The full list of basic components can be found at: <https://reactnative.dev/docs/components-and-apis>
- Here is an example that uses all the UI components that we have seen so far:
  ```
  import React from "react";
  import {
    Text,
    View,
    ActivityIndicator,
    ProgressViewIOS,
    Button,
  } from "react-native";
  // import { ProgressView } from "@react-native-community/progress-view";

  export default function App() {
    console.log("MSG: App reloaded on local device!");
    const onButtonPress = () => {
      console.log(
        `... at ${new Date().toLocaleTimeString()} button was pressed!`
      );
    };
    return (
      <View style={{ padding: 50 }}>
        <Text>Hello World!</Text>
        <Text>Red</Text>
        <Text style={{ fontSize: 50 }}>Green</Text>
        <Text style={{ fontSize: 75 }}>Red</Text>
        <ProgressViewIOS progress={0.5} />
        <ActivityIndicator />
        <Button title="Click me" onPress={onButtonPress} />
      </View>
    );
  }
  ```

### Video 2: Native API's
- An Application Programmable Interface (API) is a set of classes or functions that gives developers access to an underlying operating system, service, or application.
- For example, to display an alert pop-up natively on an IOS device, We can import the `Alert` API class and call the `Alert.alert()` function (instead of `console.log()`) in our app:
  ```
  const onButtonPress = () => {
    Alert.alert(
      `... at ${new Date().toLocaleTimeString()} button was pressed!`
    );
  };
  ```
- Other API's include:
  - `Dimensions` - which provides the device's height and width dimensions
  - `Platform` - which returns whether a device is on the IOS or Android platform
    - We can test with the `Platform` API to determine which device is being used and specify the appropriate component to display in the application window
- Here are all the API's that were just reviewed in action:
  ```
  import React from "react";
  import {
    Text,
    View,
    ActivityIndicator,
    ProgressViewIOS,
    ProgressBarAndroid,
    Button,
    Alert,
    Dimensions,
    Platform,
  } from "react-native";

  const { height, width } = Dimensions.get("window");

  export default function App() {
    console.log("MSG: App reloaded on local device!");
    const onButtonPress = () => {
      Alert.alert(
        `... at ${new Date().toLocaleTimeString()} button was pressed!`
      );
    };
    return (
      <View style={{ padding: 50 }}>
        <Text>Hello World!</Text>
        <Text>Red</Text>
        <Text style={{ fontSize: 50 }}>Green</Text>
        <Text style={{ fontSize: 75 }}>Red</Text>
        {Platform.OS === "ios" && <ProgressViewIOS progress={0.5} />}
        {Platform.OS === "android" && (
          <ProgressBarAndroid
            styleAttr="horizontal"
            indeterminate={false}
            color="blue"
            progress={0.5}
          />
        )}
        <ActivityIndicator />
        <Button title="Click me" onPress={onButtonPress} />
        <Text>Height: {height}</Text>
        <Text>Width: {width}</Text>
        <Text>Device type: {Platform.OS}</Text>
      </View>
    );
  }
  ```
