# Learning React Native
## Chapter 2: Working with Expo
### Video 2: Creating a Snack
- Log in to Expo at <https://snack.expo.dev>
- Here is a simple React-Native app that was created as a snack project on Expo:
  ```
  import React from 'react';
  import { Text, View } from 'react-native';

  export default function App() {
    return(
      <View style = {{ padding: 50 }}>
        <Text>Hello World!</Text>
        <Text>Red</Text>
        <Text>Red</Text>
        <Text>Red</Text>
      </View>
    )
  }
  ```
### Video 3: Creating a New Project
- To create a new local project locally:
  - /Pre-requisite:   
    Be sure to install Expo using the Node program manager:   
    `sudo npm install --global expo-cli` and enter admin/root password    
    Should only have to do this once to setup the Expo client
  - /1. Open a terminal window and change directories to where you want to locate the project
  - /2. Type `npx` command:   
    Syntax: `npx expo init projectName`   
    Example: `npx expo init ColorCatalog`   
    - Choose a template -- `blank` is a safe choice
    - This will create a folder with name `projectName` that you can open in VisualCode or some other IDE
- Start your React Native app:
  - In Bash shell window, go to app directory and run `expo start`   
    This will open an Echo web development server that is listening on port 19002. If the server isn't automatically opened, then create a new browser window or tab and open location `http://localhost:19002/`
### Video 4: Running a Project on a Device
- Use camera on local mobile device to scan the QR code from the terminal window or from the web development browser window
- Open the QR code link pop-up on mobile device 
- Note: If we write to console in our app, we can see the console message in the Web Development server browser display:    
  ```
  import React from "react";
  import { Text, View } from "react-native";

  export default function App() {
    console.log("MSG: App reloaded on local device!");
    return (
      <View style={{ padding: 50 }}>
        <Text>Hello World!</Text>
        <Text>Red</Text>
        <Text style={{ fontSize: 50 }}>Green</Text>
        <Text style={{ fontSize: 75 }}>Red</Text>
      </View>
    );
  }
  ```
