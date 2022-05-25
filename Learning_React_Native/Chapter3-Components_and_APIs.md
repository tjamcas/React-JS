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

### Video 3: Creating Style Sheets
- React Native styles UI components with a component, `StyleSheet`, that's based on CSS. 
- To use the `StyleSheet` component,
  - /1. First, import the `StyleSheet` object from React Native   
    `import { StyleSheet, Text, View } from "react-native";`
  - /2. Then add your styles - a good pattern is to define `styles` as an object (using the `StyleSheet.create()` method) and place it below the component at the bottom of your `app.js` file.    
    ```
    const styles = StyleSheet.create({
    page: {
      marginTop: 40,
      backgroundColor: "#DDD"
    },
    text: {
      fontSize: 22,
      color: "red",
      backgroundColor: "yellow",
      margin: 10,
      padding: 5
    },
    selectedText: {
      backgroundColor: "red",
      color: "yellow"
    }
  });
    ```
  - /3. Then set the `style` property in your component elements equal to the appropriate `styles` object key-value pair(s). Note that we assign multiple style objects by listing them in an array, e.g., `style={[styles.text, styles.selectedText]}`
    ```
    <View style={styles.page}>
      <Text style={styles.text}>red</Text>
      <Text style={[styles.text, styles.selectedText]}>
        green
      </Text>
      <Text style={styles.text}>blue</Text>
    </View>
    ```
- More information on the React Native `StyleSheet` API with its methods and properties is found at: <https://reactnative.dev/docs/stylesheet>
- More information on how React Native can design and style your application's user interface can be found at: <https://reactnative.dev/docs/style>
- Here is the full modified `App.js` file:
  ```
  import React from "react";
  import { Text, View, Alert, Button, StyleSheet } from "react-native";

  export default function App() {
    console.log("MSG: App reloaded on local device!");
    const onButtonPress = () => {
      Alert.alert(
        `... at ${new Date().toLocaleTimeString()} button was pressed!`
      );
    };
    return (
      <View style={styles.page}>
        <Text style={styles.text}>Red</Text>
        <Text style={[styles.text, styles.selectedText]}>Green</Text>
        <Text style={styles.text}>Blue</Text>
        <Button title="Click me" onPress={onButtonPress} />
      </View>
    );
  }

  const styles = StyleSheet.create({
    page: {
      marginTop: 40,
      backGroundColor: "#DDD",
    },
    text: {
      fontSize: 22,
      color: "red",
      backgroundColor: "yellow",
      margin: 10,
      padding: 5,
    },
    selectedText: {
      color: "yellow",
      backgroundColor: "red",
    },
  });
  ```

### Video 4: Flexbox Layouts
- Layout in React Native is based upon Flexbox, a popular layout module for CSS. 
- Flexbox gives us the ability to compose styles that can position elements anywhere on a screen. 
- In Flexbox, there are flex containers and flex elements. In the following, the `<View />` node will be our flex container, and it contains three flex `<Text />` elements. 
- Flex containers either display their child elements in a row (the default) or in a column. 
  - the `flexDirection` attribute can be set to `"row"` or `"column"`: `flexDirection: "column",`
  - Setting this attribute to our `<Text />` elements, `flex: 1,`, evenly distributes the elements across a single row
- More information on Flexbox can be found in this guide at: <https://css-tricks.com/snippets/css/a-guide-to-flexbox/>
- Here is an example of `App.js` code that uses Flexbox to style the UI:
  ```
  import React from "react";
  import { Text, View, StyleSheet } from "react-native";

  export default function App() {
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={styles.page}>
        <Text style={styles.text}>Red</Text>
        <Text style={[styles.text, styles.selectedText]}>Green</Text>
        <Text style={styles.text}>Blue</Text>
      </View>
    );
  }

  const styles = StyleSheet.create({
    page: {
      flex: 1,
      flexDirection: "column",
      justifyContent: "flex-end",
      alignItems: "flex-start",
      marginTop: 40,
      backgroundColor: "#DDD",
    },
    text: {
      flex: 1,
      textAlign: "center",
      fontSize: 22,
      color: "red",
      backgroundColor: "yellow",
      margin: 10,
      padding: 5,
    },
    selectedText: {
      alignSelf: "flex-end",
      backgroundColor: "red",
      color: "yellow",
    },
  });
  ```

### Video 5: The Image Component
- To add images as part of our app's user interface, follow these steps:
  - /1. Import the file(s) you want to include in the UI:   
    `import picBiscuit from "./assets/biscuit.jpg";`
  - /2. Import the `Image` component from the `react-native library:    
    `import { Image } from "react-native";`
  - /3. Add a `<Image />` element to your component:    
    `<Image style={styles.image} source={picBiscuit} />`
  - 4. Set your image attributes in your `styles` style sheet
- Here is an example of `App.js` with images added to the UI:
  ```
  import React from "react";
  import { Image, StyleSheet, View, Dimensions } from "react-native";
  import picBiscuit from "./assets/biscuit.jpg";
  import picJungle from "./assets/jungle.jpg";

  export default function App() {
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={styles.page}>
        <Image style={styles.image} source={picBiscuit} />
        <Image style={styles.image} source={picJungle} />
      </View>
    );
  }

  const styles = StyleSheet.create({
    page: {
      flex: 1,
      justifyContent: "center",
      alignItems: "center",
    },
    image: {
      flex: 1,
      borderRadius: 50,
      margin: 10,
      width: Dimensions.get("window").width - 10,
    },
  });
  ```
