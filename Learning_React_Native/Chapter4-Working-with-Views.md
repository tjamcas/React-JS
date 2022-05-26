# Learning React Native
## Chapter 4: Working with Views
### Video 1: Responding to Touches
- We can use React Native to modify our application's UI on user devices so that it responds to user touches.
- We will accomplish this through the use of the `TouchableHighlight` component from the React Native library in combination with the basic React `useState` hook.
- Here is the code that creates touchbable text that effectively behaves like a button that changes the background color of the application screen:
  ```
  import React, { useState } from "react";
  import { StyleSheet, View, Text } from "react-native";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={[styles.container, { backgroundColor }]}>
        <Text style={styles.button} onPress={() => setBackgroundColor("green")}>
          Green
        </Text>
        <Text style={styles.button} onPress={() => setBackgroundColor("red")}>
          Red
        </Text>
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
      justifyContent: "center",
      alignItems: "center",
    },
    button: {
      fontSize: 30,
      margin: 10,
      padding: 10,
      borderWidth: 2,
      borderRadius: 10,
      alignSelf: "stretch",
      textAlign: "center",
    },
  });
  ```
  - Note 1: The style sheet section positions and formats the text on the UI.
  - Note 2: The `onPress` property in the `<Text />` child component activates the button behavior.

### Video 2: Using a Touchable Highlight
- A touchable highlight is a component that activates an entire group of elements to respond to a touch.
- The `<TouchableHighlight />` component can be the parent to more than one child components. Touching anyone of the child components will be an event that can trigger further actions.
- Here is an example where a color swatch alongside some text form a group of child components to a `<TouchableHighlight />` component:
  ```
  import React, { useState } from "react";
  import { StyleSheet, View, Text, TouchableHighlight } from "react-native";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={[styles.container, { backgroundColor }]}>
        <TouchableHighlight
          style={styles.button}
          onPress={() => setBackgroundColor("yellow")}
          underlayColor="orange"
        >
          <View style={styles.row}>
            <View style={[styles.sample, { backgroundColor: "yellow" }]} />
            <Text style={styles.buttonText}>Yellow</Text>
          </View>
        </TouchableHighlight>
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
      justifyContent: "center",
      alignItems: "center",
    },
    button: {
      margin: 10,
      padding: 10,
      borderWidth: 2,
      borderRadius: 10,
      alignSelf: "stretch",
      backgroundColor: "rgba(255,255,255,0.8)",
    },
    buttonText: {
      fontSize: 30,
      textAlign: "center",
    },
    row: {
      flexDirection: "row",
      alignItems: "center",
    },
    sample: {
      height: 20,
      width: 20,
      borderRadius: 10,
      margin: 5,
      backgroundColor: "white",
    },
  });
  ```
  - Note 1: React Native doesn't use actual CSS - it's not a web browser. Instead React Native styles are based upon CSS. This means that only certain styles can apply to certain elements. For example, `<TouchableHighlight />` components can not have a font size, although a child component to a `<TouchableHighlight />` component like `<Text />` do have a font size. Therefore `<TouchableHighlight />` components cannot be assigned a style that specifies font size -- font size has to be applied to the appropriate child component. Therefore, we create a `buttonText` style that specifies font size and assign it to the `<Text/>` component.
  - Note 2: Here is the `<TouchableHighlight />` component child relationships in this example:
    - `<TouchableHighlight />` is a parent to a ...
    - `<View />` component that is a parent to ...
    - another `<View />` component and a `<Text />` component

### Video 3: Extracting a Custom Component
- The `<TouchableHighlight />` component in the example above combines multiple child components into a single color button that displays the name and swatch of a color, and has the behavior where clicking on the color button changes the background color. 
- We will extract and move this code into a reusable component called the `<ColorButton/>`.
- Here is the code that allows us to do that:
  ```
  import React, { useState } from "react";
  import { StyleSheet, View, Text, TouchableHighlight } from "react-native";

  function ColorButton({ backgroundColor, onPress = (f) => f }) {
    return (
      <TouchableHighlight
        style={styles.button}
        onPress={() => onPress(backgroundColor)}
        underlayColor="orange"
      >
        <View style={styles.row}>
          <View style={[styles.sample, { backgroundColor }]} />
          <Text style={styles.buttonText}>{backgroundColor}</Text>
        </View>
      </TouchableHighlight>
    );
  }

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={[styles.container, { backgroundColor }]}>
        <ColorButton backgroundColor="red" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="green" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="blue" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="yellow" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="purple" onPress={setBackgroundColor} />
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
      justifyContent: "center",
      alignItems: "center",
    },
    button: {
      margin: 10,
      padding: 10,
      borderWidth: 2,
      borderRadius: 10,
      alignSelf: "stretch",
      backgroundColor: "rgba(255,255,255,0.8)",
    },
    buttonText: {
      fontSize: 30,
      textAlign: "center",
    },
    row: {
      flexDirection: "row",
      alignItems: "center",
    },
    sample: {
      height: 20,
      width: 20,
      borderRadius: 10,
      margin: 5,
      backgroundColor: "white",
    },
  });
  ```
  - Note 1: We move the `<TouchableHighlight />` code into its own component, `<ColorButton />`, that is defined above and separate from the `<App />` component.
  - Note 2: We pass two arguments into the `<ColorButton />` component:
    - `backgroundColor` is the first parameter
    - The `onPress` property is the second parameter, and it is passed with the form `onPress = f => f`. By passing a default value, `f => f`, which is a "dummy" function, if the parent component does not pass an `onPress` property with a function value to `<ColorButton>`, the application will not break. The `<ColorButton />` component will just run the dummy function which does nothing.
      - In the code above, the `App` component does pass an `onPress` property with the value of the function `setBackgroundColor`:   
        <ColorButton backgroundColor="green" onPress={setBackgroundColor} />
        
### Video 4: Importing a Custom Component
- Now that we have extracted the `<ColorButton />` component, we should move it out of the `App.js` component file and place it in its own file in a newly created `components` subfolder.
- `App.js` now looks like this with the `<ColorButton />` component code block removed. Note that we also had to modify the import statements to get rid of components we are no longer using, and added an import statement to find the `./components/ColorButton.js` file.
  ```
  import React, { useState } from "react";
  import { StyleSheet, View } from "react-native";
  import ColorButton from "./components/ColorButton";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <View style={[styles.container, { backgroundColor }]}>
        <ColorButton backgroundColor="red" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="green" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="blue" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="yellow" onPress={setBackgroundColor} />
        <ColorButton backgroundColor="purple" onPress={setBackgroundColor} />
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
      justifyContent: "center",
      alignItems: "center",
    },
  });
  ```
  - Note 1: In the import statement above, we don't enclose `ColorButton` in curly braces because it is the default export in the `components/ColorButton.js` file.
- `ColorButton.js` looks like this:
  ```
  import React from "react";
  import { StyleSheet, View, Text, TouchableHighlight } from "react-native";

  export default function ColorButton({ backgroundColor, onPress = (f) => f }) {
    return (
      <TouchableHighlight
        style={styles.button}
        onPress={() => onPress(backgroundColor)}
        underlayColor="orange"
      >
        <View style={styles.row}>
          <View style={[styles.sample, { backgroundColor }]} />
          <Text style={styles.buttonText}>{backgroundColor}</Text>
        </View>
      </TouchableHighlight>
    );
  }

  const styles = StyleSheet.create({
    button: {
      margin: 10,
      padding: 10,
      borderWidth: 2,
      borderRadius: 10,
      alignSelf: "stretch",
      backgroundColor: "rgba(255,255,255,0.8)",
    },
    buttonText: {
      fontSize: 30,
      textAlign: "center",
    },
    row: {
      flexDirection: "row",
      alignItems: "center",
    },
    sample: {
      height: 20,
      width: 20,
      borderRadius: 10,
      margin: 5,
      backgroundColor: "white",
    },
  });
  ```
