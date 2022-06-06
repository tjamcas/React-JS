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
        `<ColorButton backgroundColor="green" onPress={setBackgroundColor} />`
        
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
  - Note 3: The `<FlatList />` component requires a `renderItem` property (as well as a `data` prop) whose value is a function that will be invoked once for each item in our list:
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
- In the last video, we coded the user interface for a form that allows a user-supplied color to be entered into an input box. In this video, we make the form work by adding the user input to a list of colors by implementing a controlled form component. With controlled form components, React controls the state of our form with the `useState` hook. The `useState` hook provides the input value and a function to change that value.
- Here is the revised `components/ColorForm.js` file:
  ```
  import React, { useState, useRef } from "react";
  import {
    StyleSheet,
    View,
    TextInput,
    Button
  } from "react-native";

  export default function ColorForm({ onNewColor = f => f }) {
    const [inputValue, setValue] = useState("");
    const input = useRef();
    return (
      <View style={styles.container}>
        <TextInput
          ref={input}
          style={styles.txtInput}
          value={inputValue}
          onChangeText={text => setValue(text)}
          autoCapitalize="none"
          placeholder="enter a color..."
        />
        <Button
          title="add"
          onPress={() => {
            input.current.blur();
            onNewColor(inputValue);
            setValue("");
          }}
        />
      </View>
    );
  }

  const styles = StyleSheet.create({
    container: {
      marginTop: 40,
      flexDirection: "row",
      alignItems: "center"
    },
    txtInput: {
      flex: 1,
      borderWidth: 2,
      fontSize: 20,
      margin: 5,
      borderRadius: 5,
      padding: 5
    }
  });
  ```
  - Note 1: We create a state variable for the text `inputValue` with the `useState` hook:   
    `const [inputValue, setValue] = useState("");`
  - Note 2: We add a `value` property to the `TextInput` component and set it to the state variable `inputValue`:   
    `value={inputValue}`
  - Note 3: We add the `onChangeText` property (<https://reactnative.dev/docs/textinput#onchangetext>) to the `TextInput` component and set is equal to the `useState` hook's `setValue` function:   
    `onChangeText={text => setValue(text)};`    
    However, we can write this in the more concise form:    
    `onChangetext={setValue}`    
    The `onChangeText` property automatically passes the changed text in the `textInput` component to a callback function handler -- which in this case is the `setValue` function -- we can simply set the prop equal to `setValue`
  - Note 4: We add a placeholder property to the `TextInput` component to direct the user to enter a color:    
    `placeholder="enter a color..."`
  - Note 5: We create a reference to the text input with the `useRef` hook (<https://reactjs.org/docs/hooks-reference.html>) so that we can reference the text input's built-in methods:   
    `const input = useRef();` and in the `TextInput` component:   
    ```
    <TextInput
      ref={input}
      ...
    />
    ```
  - Note 6: We need to capture the text input when the user presses the `add` button, so we add an `onPress` prop to the `add` button - for now, the `onPress` prop will be set to a function that logs the input to the console:       
    ```
    onPress={() => {
      input.current.blur();
      console.log(`add value ${inputValue}`);
      setValue("");
    }}
    ```
    - Note 6a: `input.current.blur` deselects the text input and disappears the keyboard.
    - Note 6b: `setValue("")` resets the state variable `inputValue` back to an empty string
  - Note 8 (See first Note 7 below): Finally, we modify the `onPress` call back function so that it passes the user's inputted color (ie. `inputValue`) to the `App` parent component's `onNewColor` function/property:
    ```
    onPress={() => {
        input.current.blur();
        onNewColor(inputValue);
        setValue("");
      }}
    ```
- Here is the modified `App.js` compent file that uses the `ColorForm` component:
  ```
  import React, { useState } from "react";
  import { StyleSheet, FlatList, Alert } from "react-native";
  import ColorButton from "./components/ColorButton";
  import ColorForm from "./components/ColorForm";

  import defaultColors from "./data/defaultColors.json";

  export default function App() {
    const [backgroundColor, setBackgroundColor] = useState(
      "blue"
    );
    return (
      <>
        <ColorForm
          onNewColor={newColor =>
            Alert.alert(`TODO: add color ${newColor}`)
          }
        />
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
      display: "flex"
    }
  });
  ```
  - Note 7: We add the `onNewColor` property to the `<ColorForm/>` component that, for now, just pops an alert screen - the `ColorForm` component will supply the `newColor` value (as `inputValue`):    
    ```
    <ColorForm
      onNewColor={newColor =>
        Alert.alert(`TODO: add color ${newColor}`)
      }
    />
    ```

### Creating a Custom Hook
- Audio text:   
  React hooks give us a way to isolate and separate presentation from functionality. In the last lesson, we created the colorForm, which will collect a new color from the user and then pass it to the app js component via the on new color property. At present, we are simply displaying an alert, letting us know that we have to add this color. So I'm going to get rid of the alert and I'm going to get rid of the defaultColors. We need to capture new colors from our users and then add them to this list of colors. So using the useState hook, we're going to create a state variable for colors. And we also get a function for changing that state variable, set colors. Don't forget to change the default colors to colors. And now when the app initially renders, we see an empty list of colors. Now on line nine, I'm going to create a new function for adding colors. All we have to do is send the color that we want to add to our list to this function and the function will do the rest. So I'm going to go ahead and create a new color instance. And this is going to be an object that contains a unique ID and the color that was passed to this function.    
  So we're going to need a package to help us create unique identifiers. I'm going to use a package called short ID So let's open up the terminal. And npm install shortid - `npm i shortid`. Short ID is used to generate unique identifiers. And these identifiers are short, they're not too long.     
  So now that we have shortid installed, I'm going to import the generate function from the shortid package. And we'll use the generate function when it's time to generate a new ID. So when a user adds a new color, we'll generate a unique identifier. And now what we want to do is take the new color and add it to the list of colors. So to do this on line 12, I'm going to invoke the setColors function, and I'm going to pass it a new array. That new array is going to contain our newColor and the rest of the colors. So that's what these three dots are for. This is the array spread operator, what it's going to do is take all of the colors that are in the current array, and then add them to the end of this new array. Calling set colors will cause the app component to rerender. 'setColors([newColor, ...colors]);`     
  So now down here, I'm going to get rid of the alert call. And I'm going to invoke our addColor function and pass it the newColor. Now again, creating a function that simply takes the argument and then passes that argument to the next function isn't needed. The new color will be passed to the addColor function. `<ColorForm onNewColor={addColor} />`     
  All right, so let's test it out. So I will come over here and use our form to add red. And now we can see red has been added to our list of colors. So adding green also displays our green button. So the code found here on line nine through 13 contains all of the functionality to create a new list of colors as well as the functionality to add colors to that list. We can isolate this functionality into a custom hook. Custom hooks are great because they separate the functionality from the presentation. And they give us a way to reuse that functionality and other components. So I'm going to create a new variable called use colors. This is our custom hook. And now what I'm going to do is, I'm going to copy line 13 through line 17. And I'm going to paste it inside of my useColors function. Now I've isolated this functionality into its own hook.    
  ```
  const useColors = () => {
    const [colors, setColors] = useState([]);
    const addColor = color => {
      const newColor = { id: generate(), color };
      setColors([newColor, ...colors]);
    };
    return { colors, addColor };
  };
  ```
  So the last thing that we need to do is we need to expose the list of colors in the addColor function to the consumer of this hook. I'll do so by returning an object with the list of colors in a function for adding colors. So on line 18, I can capture our colors in the addColor function from my useColors hook. And if we come over here to the simulator, we can see that we're still able to add colors. So we've now isolated all of the functionality for working with colors. The color functionality is no longer hard coded into the app component.
