# Learning React Native
## Chapter 5: Platform APIs
### Video 1: Using Asynch Storage
- In this video, we store the list of colors in persistent storage on our mobile device with the aid of the `AsynchStorage` library from React Native. More information on this deprecated library can be found at <https://reactnative.dev/docs/asyncstorage>.
  - Recommended alternative: 
    - To install: `npm install @react-native-async-storage/async-storage`
    - To import: `import AsyncStorage from '@react-native-async-storage/async-storage';`
    - For further documentation, including installation and API, go to <https://react-native-async-storage.github.io/async-storage/docs/install>
- Here is the fully modified code for `./hooks.js` that uses the `AsynchStorage` library API:    
  ```
  import { useState, useEffect } from "react";
  import { generate } from "shortid";
  import AsyncStorage from "@react-native-async-storage/async-storage";

  export const useColors = () => {
    const [colors, setColors] = useState([]);

    // Load colors that are saved on a device's persistent memory
    const loadColors = async () => {
      const colorData = await AsyncStorage.getItem("@ColorListStore:Colors");
      if (colorData) {
        const colors = JSON.parse(colorData);
        setColors(colors);
      }
    };

    // Initially load color data
    useEffect(() => {
      if (colors.length) return;
      loadColors();
    }, []);

    // Save updated color data to memory
    useEffect(() => {
      AsyncStorage.setItem("@ColorListStore:Colors", JSON.stringify(colors));
    }, [colors]);

    const addColor = (color) => {
      const newColor = { id: generate(), color };
      setColors([newColor, ...colors]);
    };
    return { colors, addColor };
  };
  ```
  - Note 1: We have moved the `useColors` hook into the `./hooks.js` file and we import it in the `./App.js` file. Here is the initial `./hooks.js` file - don't forget to export the `useColors` hook function:   
    ```
    import { useState } from "react";
    import { generate } from "shortid";

    export const useColors = () => {
      const [colors, setColors] = useState([]);
      const addColor = (color) => {
        const newColor = { id: generate(), color };
        setColors([newColor, ...colors]);
      };
      return { colors, addColor };
    };
    ```
  - Note 2: We need to create an `asynch` function to retrieve and load our color data out of the device's persistent memory:
    ```
    const loadColors = async () => {
    ...
    };
    ```
    - Note 2a: We create a constant, `colorData`, that holds the color data that we load from the device's persistent memory. We need to use `await` because the `AsynchStorage.getItem()` function returns a promise. The `getItem()` function also takes a string key to refer to the data in persistent memory:   
      `const colorData = await AsyncStorage.getItem("@ColorListStore:Colors");`
    - Note 2b: We test to see if there is any data in `colorData` - because the first time we run the app, `colorData` will be empty - and if there is, then we parse the string data into JSON format and update our state variable:
      ```
      if (colorData) {
        const colors = JSON.parse(colorData);
        setColors(colors);
      }
      ```
  - Note 3: We use the `useEffect` tool for two conditions:   
    /1. The first time we render the app view (this is triggered by the empty dependency array), we load the colors:    
    ```
    useEffect(() => {
      if (colors.length) return;
      loadColors();
    }, []);
    ```
    /2.  When the state variable `colors` array changes, we save the change to memory by calling `AsyncStorage.setItem`. We save this data as a string with `JSON.stringify`:   
    ```
    useEffect(() => {
      AsyncStorage.setItem("@ColorListStore:Colors", JSON.stringify(colors));
    }, [colors]);
    ```
- Here is the modified `App.js` file with the `useColors` custom hok extracted:
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList } from "react-native";
  import ColorButton from "./components/ColorButton";
  import ColorForm from "./components/ColorForm";
  import { useColors } from "./hooks.js";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    const { colors, addColor } = useColors();

    return (
      <>
        <ColorForm onNewColor={addColor} />
        <FlatList
          style={[styles.container, { backgroundColor }]}
          data={colors}
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

### Video 2: React Navigation
- In this video we use the `@react-navigation` libraries to allow our app to have multiple between which we can go back and forth. In our color list app, we will have a main screen that lists our colors, and by clicking on a color in the main list, we navigate to its detail screen with more information about the color.
- More information about the `@react-navigation` library, including the installation instructions and API documentation, can be found at <https://reactnavigation.org/docs/getting-started/> 
  - To install `@react-navigation`, in a terminal window in your project directory type the commands:   
    `npm install @react-navigation/native`    
    and then install the dependencies with:   
    `expo install react-native-screens react-native-safe-area-context`
- First, we will copy our `App.js` logic into a newly-created `ColorList` component file - we will have to modify some of the import references. Here is the `components/ColorList.js` file:
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList } from "react-native";
  import ColorButton from "./ColorButton";
  import ColorForm from "./ColorForm";
  import { useColors } from "../hooks.js";

  export default function ColorList() {
    const [backgroundColor, setBackgroundColor] = useState("blue");
    const { colors, addColor } = useColors();

    return (
      <>
        <ColorForm onNewColor={addColor} />
        <FlatList
          style={[styles.container, { backgroundColor }]}
          data={colors}
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
- Second, we will create the `components/ColorDetails.js` file, here is what it initially looks like with a `<Text />` stub for the color detail information:
  ```
  import React from "react";
  import { View, StyleSheet, Text } from "react-native";

  export default function ColorDetails() {
    return (
      <View style={styles.container}>
        <Text>Color Details</Text>
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      display: "flex",
      alignItems: "center",
      justifyContent: "center",
    },
  });
  ```
 - Next, we import and use the `react-navigation` library in the `App.js` file:
  ```
  
  ```
  - Note 1: we make the necessary `import` statements:    
    ```
    import React from "react";
    import ColorList from "./components/ColorList";
    import ColorDetails from "./components/ColorDetails";
    import { NavigationContainer } from "@react-navigation/native";
    import { createNativeStackNavigator } from "@react-navigation/native-stack";
    ```
  - Note 2: We deconstruct our call to `createStackNavigator` to get the `Navigator`and `Screen` component functions:    
    `const { Navigator, Screen } = createNativeStackNavigator();`
  - Note 3: Our `App` component renders a parent `<NavigationContainer />` component with (i.e., containing) a child `<Navigator />` sub-component. The `<Navigator />' child component will contain one or more `<Screen />` child components that we want to nviagte between. This is the pattern for screen navigation:    
    ```
    return (
      <NavigationContainer>
        <Navigator>
          // enter one or more <Screen /> components here
        </Navigator>
      </NavigationContainer>
    );
    ```
  - Note 4: Within the stack navigator, we render a `<Screen />` component for every screen that we want to display. The order matters. Because the home screen is listed first and the details screen second, we see home first and can not navigate yet to the details screen.    
    ```
    <NavigationContainer>
      <Navigator>
        <Screen name="Home" component={ColorList} />
        <Screen name="Details" component={ColorDetails} />
      </Navigator>
    </NavigationContainer>
    ```
