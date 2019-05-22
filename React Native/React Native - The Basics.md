### React Native - The Basics

## Components we can use, and components we can't.

React is a wrapper that knows how to make components that don't care where they run. React Native patches in the elements that can be used for Native devices. 
YOU CANNOT USE WEB ELEMENTS IN A REACT NATVE APP.
A list of base components can be found here[https://facebook.github.io/react-native/docs/components-and-apis.html]

To create a new React Native App you can use the create-react-native-app cli: 
`create-react-native-app name-of-app`

Here is an example of the app entry point

```
import React from 'react'; // 
import { StyleSheet, Text, View } from 'react-native'; // These are packages brought in from React Native

export default class App extends React.Component { // App is just a regular React component
  render() {
    return (
      <View style={styles.container}>
        <Text>Open up App.js to start working on your app!</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});

```

We want