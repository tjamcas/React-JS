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
  - Note 1: In the import statement above, we don't enclose `ColorButton` in curly braces because it is the default export in the `components/ColorButton.js` file below.
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

### Video 5: Using a Flat List
- A `<FlatList />` is a special type of view that is scrollable and is used to render a list of content. In this example, we render the `<FlatList />` from the `App` component.
- Inside the `app` `data` folder, resides the JSON document file, `defaultColors.json`, in the following format:
  ```
  [
    {
      "id": "RYffukSB",
      "color": "red"
    },
    {
      "id": "Nmj_Aalkk",
      "color": "green"
    },
    {
      "id": "dPa_1y9h2",
      "color": "blue"
    },
    ...
    {
      "id": "GiKqTrINl",
      "color": "lightred"
    }
  ]
  ```
- In the `App.js` file we will replace the `<View>...</View>` parent component with the `<FlatList />` component:
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList } from "react-native";
  import ColorButton from "./components/ColorButton";
  import defaultColors from "./data/defaultColors.json";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <FlatList
        style={[styles.container, { backgroundColor }]}
        data={defaultColors}
        renderItem={({ item }) => {
          return (
            <ColorButton
              key={item.id}
              backgroundColor={item.color}
              onPress={setBackgroundColor}
            />
          );
        }}
      />
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
    },
  });
  ```
  - Note 1: We need to modify the import statement to include `FlatList`
  - Note 2: A flat list expects an array of data. So, using the `data` property we can pass the `defaultColors` to this flat list:    
    `data = {defaultColors}`
  - Note 3: We also have to provide a `<FlatList />` with a `renderItem` property whose value is a function that will be invoked once for each item in our list:
    ```
    renderItem = {
      ({ item }) => {
        return (
          <ColorButton key={item.id} backgroundColor={item.color} onPress={setBackgroundColor} />
        )
      }
    }
    ```
  - Note 4: Each item from the `defaultColors.json` file has a "color" and an "id" which we have to pass into the `renderItem` function above -- the `key` property is always required by React when working with a list.

### Video 6: Creating a Form
- In this video, we create another component file that creates a small form that will be placed at the top of app's UI where users can add their own colors to the list.
- Here is the new component file `./components/ColorForm.js`
  ```
  import React from "react";
  import { StyleSheet, View, TextInput, Button } from "react-native";

  export default function ColorForm() {
    return (
      <View style={styles.container}>
        <TextInput style={styles.txtInput} autoCapitalize="none" />
        <Button title="add" />
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      marginTop: 40,
      flexDirection: "row",
      alignItems: "center",
    },
    txtInput: {
      flex: 1,
      borderWidth: 2,
      fontSize: 20,
      margin: 5,
      borderRadius: 5,
      padding: 5,
    },
  });
  ```
  - Note 1: We add the `<TextInput />` component which allows the app's UI to collect text input from users
  - Note 2: To make the search row visible, we change the 'container` flex direction to "row", and this aligns the text input element and the add button in a row, with the text input growing into any remaining space that the add button doesn't need
- Here is the modified `App.js` file:
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList } from "react-native";
  import ColorButton from "./components/ColorButton";
  import ColorForm from "./components/ColorForm";
  import defaultColors from "./data/defaultColors.json";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    console.log("MSG: App reloaded on local device!");

    return (
      <>
        <ColorForm />
        <FlatList
          style={[styles.container, { backgroundColor }]}
          data={defaultColors}
          renderItem={({ item }) => {
            return (
              <ColorButton
                key={item.id}
                backgroundColor={item.color}
                onPress={setBackgroundColor}
              />
            );
          }}
        />
      </>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
    },
  });
  ```
  - Note: The only change is importing the `ColorForm` component file and adding it above the `<Flatlist />` component.

### Video 7: Collecting Input
-
