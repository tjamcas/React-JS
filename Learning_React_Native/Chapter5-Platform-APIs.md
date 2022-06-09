# Learning React Native
## Chapter 5: Platform APIs
### Video 1: Using Asynch Storage
- In this video, we store the list of colors in persistent storage on our mobile device with the aid of the `AsynchStorage` library from React Native. More information on this deprecated library can be found at <https://reactnative.dev/docs/asyncstorage>.
  - Recommended alternative: 
    - To install: `npm install @react-native-async-storage/async-storage`
    - To import: `import AsyncStorage from '@react-native-async-storage/async-storage';`
    - For further documentation, including installation and API, go to <https://react-native-async-storage.github.io/async-storage/docs/install>
- We need to import the library into our project:   
  ` `
- Here is the fully modified code for `./hooks.js` that uses the `AsynchStorage` library API:    
  ```
  
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
    - Note 2a: We create a constant, `colorData`, that holds the color data that we load from the device's persistent memory. We need to use `await` because the `AsynchStorage.getItem()` function returns a promise. the `getItem()` function also takes a string key to refer to the data in persistent memory:   
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
