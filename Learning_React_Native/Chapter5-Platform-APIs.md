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

### Video 3: Navigating Between Screens
- In the following code, we clean up the appearance of our screens and enable navigation between the "Home" and "Details" screen.
- In cleaning up the appearance of our screens, first, in `App.js`, we change the title of the "Home" screen so that the title does not default to the "name" property:    
  ```
  <Screen
    name="Home"
    options={{ title: "Color List" }}
    component={ColorList}
  />
  ```
- Second, we return to the `components/ColorForm.js` file to clean up the styling and remove the `marginTop` and set the `backgroundColor`:   
  ```
  const styles = StyleSheet.create({
    container: {
      backgroundColor: "white",
      flexDirection: "row",
      alignItems: "center",
    },
    txtInput: {
      ... (no changes)
    },
  });
  ```
- Third, in the `components.ColorList.js` component, we remove the logic that changes the background color of our "home", i.e. color list, screen. See the three spaces where we remove references to the `backgroundColor`:    
  ```
  export default function ColorList() {
    // 1.: const [backgroundColor, setBackgroundColor] = useState("blue");
    const { colors, addColor } = useColors();

    return (
      <>
        <ColorForm onNewColor={addColor} />
        <FlatList
          style={[styles.container //2.: , { backgroundColor }]}
          data={colors}
          renderItem={({ item }) => {
            return (
              <ColorButton
                key={item.id}
                backgroundColor={item.color}
                // 3.: onPress={setBackgroundColor}
              />
            );
          }}
        />
      </>
    );
  }
  ```
- Now that we cleaned up the appearances of the screen, we can add the navigation logic.
- The navigation (i.e., the stack) begins at the first screen that's rendered - the home screen. Recall, this is defined in the `App.js` file. When a `<Screen />` component is rendered, it actually passes more properties - in this case, the props are passed to the `<ColorList />` component. Specifically, there is a `navigation` object thatis automatically passed to props, and can be used to navigate between screens. For more information on the `navigation` prop, see <https://reactnavigation.org/docs/navigation-prop>. In the `<components/ColorList.js` file, in the `<FlatList />` component, where the color buttons are rendered, we add an `onPress` property to each color button that calls the `navigation.navigate` function to navigate to the `details` screen. See the following code in the `<components/ColorList.js` file:    
  ```
  export default function ColorList({ navigation }) {
    const { colors, addColor } = useColors();

    return (
      <>
        <ColorForm onNewColor={addColor} />
        <FlatList
          style={[styles.container]}
          data={colors}
          renderItem={({ item }) => {
            return (
              <ColorButton
                key={item.id}
                backgroundColor={item.color}
                onPress={() => navigation.navigate("Details")}
              />
            );
          }}
        />
      </>
    );
  }
  ```
  - Note 1: We deconstruct the `navigation` object from the passed `props` when we declare the `ColorList` component function:   
    `export default function ColorList({ navigation }) { ... }`
  - Note 2: The `navigation` object has a `navigate` method that we can reference for each of the items in our `<FlatList />`. Specifically, we use it in the `onPress` function to navigate to a "Details" screen:   
    ```
    <FlatList
      style={[styles.container]}
      data={colors}
      renderItem={({ item }) => {
        return (
          <ColorButton
            key={item.id}
            backgroundColor={item.color}
            onPress={() =>
              navigation.navigate("Details", { color: item.color })
            }
          />
        );
      }}
    />
    ```
    - Note 2a: The `navigation.navigate` function can take two parameters. The first argument is the name of the screen that you want to navigate to. The second argument is a parameters object, within which you add any data to be passed to the navigation request. In this case, we pass the name of the color (to the "Details" screen which is rendered by the `ColorDetails.js` component file) which is referenced within this render item function as `item.color`:    
      `onPress={() => navigation.navigate("Details", { color: item.color })}`   
      More information about `navigate` can be found at <https://reactnavigation.org/docs/navigation-prop#navigate>
- In addition to the `navigation` prop, `<Screen />` also passes `route` as a prop when it is rendered. For further information, see <https://reactnavigation.org/docs/route-prop>. As just noted above, the name of the color is passed to the "Details" screen which is rendered by the `ColorDetails.js` component file. We can access the color parameter from the `route` property object. Just like the way `App.js` passed `navigation` properties to the `ColorList.js` component through the `<Screen />` component, `App.js` passes `route` properties to `ColorDetails.js` through its `<Screen />` component. Specifically, with the `route` object that's passed to props, we will use `route.params` to grab the `color` that was passed as a parameter:    
    ```
    export default function ColorDetails({ route }) {
      return (
        <View style={styles.container}>
          <Text>Color Details: {route.params.color}</Text>
        </View>
      );
    }
    ```
  - Note 3: We deconstruct the `route` object from the passed `props` when we declare the `ColorDetails` component function:   
    `export default function ColorDetails({ route }) { ... }`
  - Note 4: We modify the displayed `<Text />` component by referencing `route.params.color` as follows:    
    `<Text>Color Details: {route.params.color}</Text>`
- Here is the modified `App.js` file:
  ```
  import React from "react";
  import ColorList from "./components/ColorList";
  import ColorDetails from "./components/ColorDetails";
  import { NavigationContainer } from "@react-navigation/native";
  import { createNativeStackNavigator } from "@react-navigation/native-stack";

  const { Navigator, Screen } = createNativeStackNavigator();

  export default function App() {
    return (
      <NavigationContainer>
        <Navigator>
          <Screen
            name="Home"
            options={{ title: "Color List" }}
            component={ColorList}
          />
          <Screen name="Details" component={ColorDetails} />
        </Navigator>
      </NavigationContainer>
    );
  }
  ```
- Here is the modified `components/ColorList.js` file:    
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList } from "react-native";
  import ColorButton from "./ColorButton";
  import ColorForm from "./ColorForm";
  import { useColors } from "../hooks.js";

  export default function ColorList({ navigation }) {
    const { colors, addColor } = useColors();

    return (
      <>
        <ColorForm onNewColor={addColor} />
        <FlatList
          style={[styles.container]}
          data={colors}
          renderItem={({ item }) => {
            return (
              <ColorButton
                key={item.id}
                backgroundColor={item.color}
                onPress={() =>
                  navigation.navigate("Details", { color: item.color })
                }
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
- Here is the modified `components/ColorDetails.js` file:   
  ```
  import React from "react";
  import { View, StyleSheet, Text } from "react-native";

  export default function ColorDetails({ route }) {
    return (
      <View style={styles.container}>
        <Text>Color Details: {route.params.color}</Text>
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

### Video 4: Final Touches using the Color API
- In this video, we write the final code for the color list app. WE add functionality to the color detail screen to change the background color of the screen to the selected color. We also display information about the color in the detail screen: its RGB, HSL, and luminosity values. We add styles to the detail text to increase its font size and to change the text color to the inverse of the color so that the text can be more easily read.
- We install the `Color` API library using the command:   
  `npm install Color`   
  And import the `Color` library into our component file:    
  `import Color from "color";`
- Here is the fully modified code for the `components/ColorDetails.js` file:    
  ```
  import React from "react";
  import { View, StyleSheet, Text } from "react-native";
  import Color from "color";

  export default function ColorDetails({ route }) {
    const { color: name } = route.params;
    const color = Color(name);
    const textColor = { fontSize: 20, color: color.negate().toString() };
    return (
      <View style={[styles.container, { backgroundColor: name }]}>
        <Text style={textColor}>{name}</Text>
        <Text style={textColor}>{color.rgb().toString()}</Text>
        <Text style={textColor}>{color.hsl().toString()}</Text>
        <Text style={textColor}>{color.luminosity()}</Text>
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
  - Note 1: at present the color details screen shows a text message with the name of the color displayed on background field of grey. This information was passed as a `route` parameter by `ColorList` to `ColorDetails`. To simplify the code, we create a `color` variable by destructuring `color: name` from `route` params, like so:    
    ```
    const { color: name } = route.params;
    const color = Color(name);
    ```
  - Note 2: we alter the style to display the selected color as the background color:   
    `<View style={[styles.container, { backgroundColor: name }]}>`
  - Note 3: next, we add `<Text />` components under the `<View />` component with the RGB, HSL, and luminosity values of the selected color by using the `Color` api:   
    ```
    <Text>{name}</Text>
    <Text>{color.rgb().toString()}</Text>
    <Text>{color.hsl().toString()}</Text>
    <Text>{color.luminosity()}</Text>
    ```
  - Note 4: we create a variable called `textColor` that is the inverse of the color that we are detailing. The `textColor` is used to style the `<Text />` components:
    ```
    const textColor = { fontSize: 20, color: color.negate().toString() };
    return (
      <View style={[styles.container, { backgroundColor: name }]}>
        <Text style={textColor}>{name}</Text>
        <Text style={textColor}>{color.rgb().toString()}</Text>
        <Text style={textColor}>{color.hsl().toString()}</Text>
        <Text style={textColor}>{color.luminosity()}</Text>
      </View>
    ```

### Video 5: Fetching Data
- Here is a new app that fetches data from the internet using asynch functions.
- We create the following code using the Expo Snack tool by going to <https://expo.dev/> and, in the `Explore` panel, selecting the `Try Snack` button.
- The default code imports `Constant`,    
  `import Constants from 'expo-constants';`   
  which allows us to get the exact status bar height for each device:   
  ```
  const styles = StyleSheet.create({
    container: {
      flex: 1,
      justifyContent: 'center',
      paddingTop: Constants.statusBarHeight,
      backgroundColor: '#ecf0f1',
      padding: 8,
    },
    ...
  });
  ```
- We import some new components:    
  `import { Text, ScrollView, SafeAreaView, StyleSheet } from 'react-native';`    
  - `SafeAreaView` renders the view in the main area of our screen.
  - `ScrollView` is like a regular view but it scrolls content that cannot fit within a single screen
- We create the state variable, `pet`, with the `useState` hook:    
  `const [pet, setPet] = useState();`
- If there is no `pet`, then we return `null` - which means nothing is rendered in the app screen:    
  `if (!pet) return null;`
- We load data from the internet as an app side effect with the `useEffect` hook and specify an empty dependency array so that the effect  function, `loadPet`, is invoked once on initial rendering:   
  `useEffect(() => {loadPet;}, []);`
- We create the `loadPet` asynchronous function that loads data from an ineternet API, `pet-library.moonhighway.com/api/randomPet`, created for this course that returns an object of JSON data about a random pet:
  ```
  
  ```
